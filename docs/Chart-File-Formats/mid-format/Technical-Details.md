# Technical Format Details

This file gives a general overview of how MIDI files are structured. It was not meant to be fully comprehensive, but ends up covering pretty much everything anyways.

For a full reference, see documents such as [the MIDI Association's specifications](https://www.midi.org/specifications/midi1-specifications).

## Number Conventions

For the most part, numbers are stored in big-endian format (most significant byte first).

Some numbers have a variable length. These numbers have their value stored across the bottom 7 bits of multiple bytes, with the most significant bit of each byte set if there is a following byte with more of the value. These numbers are limited to a length of 4 bytes to allow all values to fit into 32 bits, making the largest possible number after decoding `0x0FFFFFFF`.

Variable-length number examples from the Standard MIDI File Specifications:

| Number       | Stored Bytes    |
| :-----       | :-----------    |
| `0x00000000` | `0x00`          |
| `0x00000040` | `0x40`          |
| `0x0000007F` | `0x7F`          |
| `0x00000080` | `0x81 00`       |
| `0x00002000` | `0xC0 00`       |
| `0x00003FFF` | `0xFF 7F`       |
| `0x00004000` | `0x80 80 00`    |
| `0x00100000` | `0xC0 80 00`    |
| `0x001FFFFF` | `0xFF FF 7F`    |
| `0x00200000` | `0x81 80 80 00` |
| `0x08000000` | `0xC0 80 80 00` |
| `0x0FFFFFFF` | `0xFF FF FF 7F` |

## Chunks

MIDI files are comprised of segments called chunks. These chunks follow this general format:

`<Type> <Length> <Data[]>`

- `Type` is a 4-character ASCII string indicating the chunk type.
- `Length` is the number of bytes in the chunk represented as a 32-bit big-endian number. All 4 bytes of this number are always present, it is not a variable-length quantity.
- `Data[]` is the data of the chunk.

### Header Chunks

Header chunks contain metadata about the MIDI file as a whole. They use the `MThd` type string, and their format is as follows:

`"MThd" <Length> <Format type> <Number of tracks> <Delta-time type/resolution>`

- `Length` is usually `0x00 00 00 06`.
  - Even though this length is constant, the MIDI Association docs state that the value in the file must be respected, as there could potentially be additions in the future that change the length.
- `Format type` is a 16-bit number indicating the Format of the file:
  - `0x00 00` - The file contains a single track.
  - `0x00 01` - The file contains one or more simultaneous tracks.
  - `0x00 02` - The file contains one or more sequential single-track patterns.
- `Number of tracks` is a 16-bit number representing the number of tracks in the file. As this is only metadata, the actual number of tracks may be different than specified here.
- `Delta-time type/resolution` is a 16-bit number representing both what unit the resolution is measured in, and how many ticks the unit is divided in to.
  - The most-significant bit (bit 15) is a boolean indicating the format:
    - `0` - Resolution is in ticks per quarter note.
      - The other 15 bits contain the resolution number.
    - `1` - Resolution is in ticks per SMPTE frame.
      - Bits 0-7 contain the resolution number.
      - Bits 8-14 represent the SMPTE number of the format to be used, stored as a negative two's compliment value. The value should correspond to one of the 4 standard SMPTE and MIDI time code formats:
        - `-24` - 24 frames per second
        - `-25` - 25 frames per second
        - `-29` - 29.97 frames per second (30 FPS drop-frame)
        - `-30` - 30 frames per second

### Track Chunks

Track chunks contain the actual MIDI data streams. They use the `MTrk` type string, and their format is as follows:

`"MTrk" <Length> <Event[]>`

- `Event[]` is one or more MIDI event.

In a format 1 file, the first track should be used exclusively for tempos, time signatures, SMPTE offsets, and other data that affects all tracks.

## Events

MIDI events follow this general format:

`<Delta-time> <Status> <Data[]>`

- `Delta-time` is the time in ticks relative to either the start of the track, or to the previous event in the stream.
- `Status` is a byte indicating the event type.
  - MIDI files use running status, meaning that channel events can omit the status byte and re-use the status of a previous channel event. The first event must specify a status.
- `Data[]` is any number of bytes representing the data of the event.

### Event Bytes

The byte values in events are split evenly into two categories: data bytes (`0x00`-`0x7F`), and status bytes (`0x80`-`0xFF`). Status bytes indicate the type of event, and data bytes contain the event's data.

Some events may contain data bytes greater than `0x7F`. While this is technically non-standard, the events that it may occur in are structured such that it can easily be accounted for.

### Running Status

Events may leave out the `Status` byte and just provide the data bytes if the prior event has the same status byte. This is referred to as running status.

Running status resets after a System Exclusive or meta event, and does not apply to those event types.

## Event Types

### Channel Voice

Channel voice events control a channel's voices. The bottom 4 bits of channel voice messages represent a 4-bit channel number, giving 16 channels per track (numbered 0-15 throughout this document). In the following table, this will be notated as `n` in the Byte column.

| Format           | Name             | Description |
| :-----           | :---             | :---------- |
| `8n <num> <vel>` | Note Off         | Marks the end of a note.<br>`num` is a note number from 0-127, `vel` is a velocity value from 0-127. |
| `9n <num> <vel>` | Note On          | Marks the start of a note.<br>`num` is a note number from 0-127, `vel` is a velocity value from 0-127.<br>A Note On with a velocity of 0 is equivalent to a Note Off. |
| `An <num> <prs>` | Polyphonic Key Pressure | An aftertouch value that modifies an active note in some way.<br>`num` is a note number from 0-127, `prs` is a pressure value from 0-127. |
| `Bn <num> <val>` | Control Change   | Changes the value of a control channel.<br>`num` is a controller number from 0-127, and `val` is a control value from 0-127.<br>Controller numbers 120-127 are reserved for channel mode messages, see below. |
| `Cn <prgm>`      | Program Change   | Changes the selected program (sound, voice, preset, etc.) of a channel.<br>`prgm` is a program number from 0-127. |
| `Dn <prs>`       | Channel Pressure | An aftertouch value that modifies the whole channel in some way.<br>`prs` is a pressure value from 0-127. |
| `En <lsb> <msb>` | Pitch Bend       | Bends the pitch of a channel.<br>`lsb` and `msb` are a 14-bit bend value from 0-16383, where `lsb` are the least significant bits, and `msb` are the most significant bits, excluding the top (7th) bit of each byte.<br>Max negative is (LSB, MSB) 0, 0; center is 0, 64 (8,192); max positive is 127, 127 (16,383). |

### Channel Mode

Channel mode events control parameters that affect a channel's response to voice events. These are special control change controllers, using the reserved control channels 120-127. Since they are just control change messages, they can be on any channel, however a device can only send and receive them on a specific channel (referred to as its basic channel).

`Bn <120-127> <val>`

| Format         | Name                  | Description                                                                             |
| :-----         | :---                  | :----------                                                                             |
| `Bn 120 0`     | All Sound Off         | Disables all notes and sets their volumes to zero.                                      |
| `Bn 121 <val>` | Reset All Controllers | Resets all control channels to their default state.<br>`val` should only be 0 unless otherwise allowed in a specific recommended practice. |
| `Bn 122 <val>` | Local Control         | Enables or disables a device's local controls.<br>`val` acts as a max/min boolean: enables with `0x7F`, disables with `0x00`. |
| `Bn 123 0`     | All Notes Off         | Turns off all currently active notes.                                                   |
| `Bn 124 0`     | Omni Off              | Sets a device to only respond to events on its basic channel. Also turns off all notes. |
| `Bn 125 0`     | Omni On               | Sets a device to respond to events on all channels. Also turns off all notes.           |
| `Bn 126 <val>` | Mono On (Poly Off)    | Sets a device to respond monophonically to notes on a channel. Also turns off all notes.<br>`val` is either 0 if Omni is on, or the number of channels to be allowed starting from the basic channel if Omni is off. |
| `Bn 127 0`     | Poly On (Mono Off)    | Sets a device to respond polyphonically to notes on all channels. mode. Also turns off all notes. |

### System Events

System events affect the whole MIDI stream and are not specific to a channel (i.e. they are received on all channels). There are three types of system events: Common, Real-Time, and Exclusive.

System events, with the exception of System Exclusive and End-of-Exclusive events, are not supported in MIDI files, and thus are not documented here. However, they may be escaped through a special form of Exclusive events, for purposes such as transmission to a physical MIDI device.

### System Exclusive Events

System Exclusive (often shortened as SysEx) events are flexible, and aside from the starting and ending bytes, and a manufacturer/device ID, their format is vendor-defined. They can contain any number of data bytes, and they are the only System events allowed in MIDI files.

| Format                 | Event Name          | Description                                       |
| :----:                 | :---------          | :----------                                       |
| `F0 <len> <byte[]> F7` | Standard SysEx      | Sends a SysEx event.<br>`F0` starts a SysEx event, `F7` ends a SysEx event. `len` is the number of bytes in the event including the `F7`, stored as a variable-length number. `byte[]` is one or more bytes.<br>The first 1 or 3 bytes are typically a numeric ID. If the first byte is 0, the next two are the ID, otherwise just the first is. |
| `F7 <len> <byte[]>`    | SysEx (No end byte) | Sends a SysEx event without using an ending byte. This can be used to escape events that would otherwise be invalid. |

Data bytes above `0x7F` that are not System Real-Time status bytes (`0xF8`-`0xFF`) are not allowed, and are defined to be interpreted as an end-of-exclusive marker.

There are some Universal SysEx events defined in the MIDI Association's documentation, but for simplicity's sake these will not be documented here.

### Meta Events

Meta events are events that store non-MIDI data such as text. They follow this format:

`FF <type> <length> <data[]>`

- `type` is a type byte of the meta event, ranging from 0-127.
- `length` is a variable-length number representing the number of bytes in `data[]`, or 0 if there is no data.
- `data[]` is any number of data bytes contained in the event.

| Format                                        | Event Name          | Description |
| :----:                                        | :---------          | :---------- |
| `0xFF 0x00 0x02 <num>`                        | Sequence number     | An optional track sequence number.<br>Must occur at the beginning of a track if used.<br>`num` is the sequence number as a 16-bit number. |
| `0xFF 0x01 <len> <text[]>`                    | Text Event          | Any amount of text.<br>`text` is any amount of text (typically ASCII, but may be another format).<br>Meta event types `0x02` through `0x0F` are reserved for specialized text events. |
| `0xFF 0x02 <len> <text[]>`                    | Copyright notice    | A copyright notice. |
| `0xFF 0x03 <len> <text[]>`                    | Sequence/track name | The name of a sequence or track.<br>Must occur at the beginning of a track. |
| `0xFF 0x04 <len> <text[]>`                    | Instrument name     | A description of the instrumentation for the track. |
| `0xFF 0x05 <len> <text[]>`                    | Lyric               | A lyric, generally a single syllable. |
| `0xFF 0x06 <len> <text[]>`                    | Marker              | Marks a point in a sequence.<br>In a type-1 file, this is typically only on the first track. |
| `0xFF 0x07 <len> <text[]>`                    | Cue point           | Describes an event happening at this point in an accompanying media. |
| `0xFF 0x20 0x01 <chn>`                        | MIDI channel prefix | Associates a MIDI channel to all following events, including SysEx and other meta events, up until the next normal channel event or another channel prefix event.<br>`chn` is any channel number from 0-15. |
| `0xFF 0x2F 0x00`                              | End of track        | Marks the exact ending point of a track. Per the specs, this event is *not* optional, it must be the last event in a track. |
| `0xFF 0x51 0x03 <tmp>`                        | Set tempo           | Sets a new tempo.<br>`tmp` is the tempo as a 24-bit number, in microseconds per quarter note.<br>If tempo is not specified, it is assumed to be 120 BPM. |
| `0xFF 0x54 0x05 <hr/fmt> <min> <sec> <frm> <frc>` | SMPTE offset    | Sets an SMPTE time that a track should start at.<br>Every value is 1 byte: `hr` is the hour, `min` is the minute, `sec` is the second, `frm` is the frame, and `frc` is the fractional frame in 1/100ths of a frame.<br>The hour value also encodes the SMPTE format in its top 2 bytes (in 7-bit terms): `0b0ffhhhhh`, where `ff` is the SMPTE format (`00` = 24 FPS, `01` = 25, `10` = 30 drop-frame, `11` = 30), and `hhhhh` is the actual hour value. |
| `0xFF 0x58 0x04 <num> <den> <clk> <base>`     | Time signature      | Sets a new time signature.<br>`num` is the numerator, `den` is a power-of-2 exponent for the denominator, `clk` is the number of MIDI clocks in a metronome click, `base` is the number of notated 32nd notes per MIDI quarter note.<br>If a time signature is not specified, it is assumed to be 4/4. |
| `0xFF 0x59 0x02 <shp/flt> <maj/min>`          | Key signature       | Sets a new key signature.<br>`shp/flat` is the number of sharps or flats from -7 to 7 (positive is sharps, negative is flats, 0 is none), `maj/min` is 0 for major, 1 for minor. |
| `0xFF 0x7F <len> <data[]>`                    | Vendor-defined      | Stores a vendor-defined meta event. |

## Notable Specification Violations

Some charts don't reset running status after a System Exclusive or meta event and continue the running status of prior non-Exclusive/meta events, which breaks some MIDI parsers (notably NAudio).<!-- Thanks, Phase Shift. -->

Charts commonly make use of a `0xFF` byte in SysEx events. This is *barely* technically spec-compliant, but some MIDI parsers might have issues with this regardless.
