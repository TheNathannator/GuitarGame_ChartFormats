# MIDI

The `.mid`/`.midi` format is a binary stream of MIDI events. The extension used for charts is `.mid`; `.midi` is not typically used and is not supported by most programs that use it for charts.

Harmonix designed a specific layout for the charts in Guitar Hero 1/2 and Rock Band using .mid files, and early fan-games were quick to analyze and adopt the format. Its use was further helped by the introduction of Rock Band Network, a service for Rock Band that allowed people to license and chart songs to be made available as DLC. This format remains prominent today, and still receives new features on occasion as the need arises.

Modern .mid charts follow a format originating from Rock Band's .mid chart format, with additional functionality added by the community in games such as Phase Shift and Clone Hero. Older .mid charts are slightly different and are based on Guitar Hero 1/2's format, though the differences are small and easily accounted for.

## Table of Contents

- [Basic Infrastructure of MIDI Files](#basic-infrastructure-of-midi-files)
  - [Number Conventions](#number-conventions)
  - [Chunks](#chunks)
    - [Header Chunks](#header-chunks)
    - [Track Chunks](#track-chunks)
  - [Events](#events)
    - [Event Bytes](#event-bytes)
    - [Running Status](#running-status)
  - [Event Types](#event-types)
    - [Channel Voice](#channel-voice)
    - [Channel Mode](#channel-mode)
    - [System Events](#system-events)
    - [System Exclusive Events](#system-exclusive-events)
    - [Meta Events](#meta-events)
  - [Notable Specification Violations](#notable-specification-violations)
- [Chart Format Infrastructure](#chart-format-infrastructure)
  - [Nomenclature](#nomenclature)
  - [Audio Files](#audio-files)
  - [Metadata](#metadata)
  - [Track Names](#track-names)
  - [Track Events](#track-events)
  - [Track Difficulties](#track-difficulties)
  - [Other Track Data](#other-track-data)
  - [Text Events](#text-events)
  - [Global and Local Events](#global-and-local-events)
    - [Basic Global Events](#basic-global-events)
  - [Phase Shift SysEx Event Specification](#phase-shift-sysex-event-specification)
- [References](#references)

## Basic Infrastructure of MIDI Files

This section gives a general but detailed overview of what goes on in MIDI files. It is not meant to be fully comprehensive however; for a more detailed reference see documents such as [the MIDI Association's specifications](https://www.midi.org/specifications/midi1-specifications).

Note that there are some explicit warnings about format violations that some charts may have. Some information may also be wrong, this section should not be used as a primary source for MIDI format information.

### Number Conventions

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

### Chunks

MIDI files are comprised of segments called chunks. These chunks follow this general format:

`<Type> <Length> <Data[]>`

- `Type` is a 4-character ASCII string indicating the chunk type.
- `Length` is the number of bytes in the chunk represented as a 32-bit big-endian number. All 4 bytes of this number are always present, it is not a variable-length quantity.
- `Data[]` is the data of the chunk.

#### Header Chunks

Header chunks contain metadata about the MIDI file as a whole. They use the `MThd` type string, and their format is as follows:

`"MThd" <Length> <Format type> <Number of tracks> <Delta-time type/resolution>`

- `Length` is usually `0x00 00 00 06`.
  - Even though this length is constant, the MIDI Association docs state that the value in the file must be respected, as there could potentially be additions in the future that change the length.
- `Format type` is a 16-bit number indicating the Format of the file:
  - `0x00 00` - The file contains a single track.
  - `0x00 01` - The file contains one or more simultaneous tracks.
  - `0x00 02` - The file contains one or more sequential single-track patterns. (These are not used in chart files.)
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

#### Track Chunks

Track chunks contain the actual MIDI data streams. They use the `MTrk` type string, and their format is as follows:

`"MTrk" <Length> <Event[]>`

- `Event[]` is one or more MIDI event.

In a format 1 file, the first track should be used exclusively for tempos, time signatures, SMPTE offsets, and other data that affects all tracks.

### Events

MIDI events follow this general format:

`<Delta-time> <Status> <Data[]>`

- `Delta-time` is the time in ticks relative to either the start of the track, or to the previous event in the stream.
- `Status` is a byte indicating the event type.
  - MIDI files use running status, meaning that channel events can omit the status byte and re-use the status of a previous channel event. The first event must specify a status.
- `Data[]` is any number of bytes representing the data of the event.

#### Event Bytes

The byte values in events are split evenly into two categories: data bytes (`0x00`-`0x7F`), and status bytes (`0x80`-`0xFF`). Status bytes indicate the type of event, and data bytes contain the event's data.

Some events may contain data bytes greater than `0x7F`. While this is technically non-standard, the events that it may occur in are structured such that it can easily be accounted for.

#### Running Status

Events may leave out the `Status` byte and just provide the data bytes if the prior event has the same status byte. This is referred to as running status.

Running status resets after a System Exclusive or meta event, and does not apply to those event types.

### Event Types

Not all of these event types pertain to chart files, but they're listed anyways as a general overview.

#### Channel Voice

Channel voice events control a channel's voices. The bottom 4 bits of channel voice messages represent a 4-bit channel number, giving 16 channels per track (numbered 0-15 throughout this document). In the following table, this will be notated as `n` in the Byte column.

| Format                 | Name             | Description |
| :-----                 | :---             | :---------- |
| `8n <num> <vel>`       | Note Off         | Marks the end of a note.<br>`num` is a note number from 0-127, `vel` is a velocity value from 0-127. |
| `9n <num> <vel>`       | Note On          | Marks the start of a note.<br>`num` is a note number from 0-127, `vel` is a velocity value from 0-127.<br>A Note On with a velocity of 0 is equivalent to a Note Off. |
| `An <num> <prs>`       | Polyphonic Key Pressure | An aftertouch value that modifies an active note in some way.<br>`num` is a note number from 0-127, `prs` is a pressure value from 0-127. |
| `Bn <num> <val>`       | Control Change   | Changes the value of a control channel.<br>`num` is a controller number from 0-127, and `val` is a control value from 0-127.<br>Controller numbers 120-127 are reserved for channel mode messages, see below. |
| `Cn <prgm>`            | Program Change   | Changes the selected program (sound, voice, preset, etc.) of a channel.<br>`prgm` is a program number from 0-127. |
| `Dn <prs>`             | Channel Pressure | An aftertouch value that modifies the whole channel in some way.<br>`prs` is a pressure value from 0-127. |
| `En <num> <lsb> <msb>` | Pitch Bend       | Bends the pitch of a note.<br>`num` is a note number, `lsb` and `msb` are a 14-bit bend value from 0-16383, where `lsb` are the least significant bits, and `msb` are the most significant bits, excluding the top (7th) bit of each byte.<br>Max negative is (LSB, MSB) 0, 0; center is 0, 64 (8,192); max positive is 127, 127 (16,383). |

#### Channel Mode

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

#### System Events

System events affect the whole MIDI stream and are not specific to a channel (i.e. they are received on all channels). There are three types of system events: Common, Real-Time, and Exclusive.

System events, with the exception of System Exclusive and End-of-Exclusive events, are not supported in MIDI files, and thus are not documented here. However, they may be escaped through a special form of Exclusive events, for purposes such as transmission to a physical MIDI device.

#### System Exclusive Events

System Exclusive (often shortened as SysEx) events are flexible, and aside from the starting and ending bytes, and a manufacturer/device ID, their format is vendor-defined. They can contain any number of data bytes, and they are the only System events allowed in MIDI files.

| Format                 | Event Name          | Description                                       |
| :----:                 | :---------          | :----------                                       |
| `F0 <len> <byte[]> F7` | Standard SysEx      | Sends a SysEx event.<br>`F0` starts a SysEx event, `F7` ends a SysEx event. `len` is the number of bytes in the event including the `F7`, stored as a variable-length number. `byte[]` is one or more bytes.<br>The first 1 or 3 bytes are typically a numeric ID. If the first byte is 0, the next two are the ID, otherwise just the first is. |
| `F7 <len> <byte[]>`    | SysEx (No end byte) | Sends a SysEx event without using an ending byte. This can be used to escape events that would otherwise be invalid. |

Data bytes above `0x7F` that are not System Real-Time status bytes (`0xF8`-`0xFF`) are not allowed, and are defined to be interpreted as an end-of-exclusive marker.

There are some Universal SysEx events defined in the MIDI Association's documentation, but for simplicity's sake these will not be documented here.

#### Meta Events

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
| `0xFF 0x54 0x05 <hr> <min> <sec> <frm> <frc>` | SMPTE offset        | Sets an SMPTE time that a track should start at.<br>Every value is 1 byte: `hr` is the hour, `min` is the minute, `sec` is the second, `frm` is the frame, and `frc` is the fractional frame in 1/100ths of a frame. |
| `0xFF 0x58 0x04 <num> <den> <clk> <base>`     | Time signature      | Sets a new time signature.<br>`num` is the numerator, `den` is a power-of-2 exponent for the denominator, `clk` is the number of MIDI clocks in a metronome click, `base` is the number of notated 32nd notes per MIDI quarter note.<br>If a time signature is not specified, it is assumed to be 4/4. |
| `0xFF 0x59 0x02 <shp/flt> <maj/min>`          | Key signature       | Sets a new key signature.<br>`shp/flat` is the number of sharps or flats from -7 to 7 (positive is sharps, negative is flats, 0 is none), `maj/min` is 0 for major, 1 for minor. |
| `0xFF 0x7F <len> <data[]>`                    | Vendor-defined      | Stores a vendor-defined meta event. |

### Notable Specification Violations

Some charts don't reset running status after a System Exclusive or meta event and continue the running status of prior non-Exclusive/meta events, which breaks some MIDI parsers (notably NAudio).<!-- Thanks, Phase Shift. -->

Charts commonly make use of a `0xFF` byte in SysEx events. This is *barely* technically spec-compliant, but some MIDI parsers might have issues with this regardless.

## Chart Format Infrastructure

This section goes over the general format of .mid chart files.

### Nomenclature

To clarify some things said in these docs, some nomenclature will be defined here.

- "Note" refers to a MIDI event that specifies a note in the chart.
- "MIDI note" refers to a Note On/Off event pair.
- "Phrase" refers to an in-game mechanic that lasts for a duration. Unless specified otherwise, phrases do not affect the tick they end on.
- "Modifier" refers to an in-game mechanic that may modify one or more other events.
- "Marker" refers to an event that specifies a phrase or modifier.
- "Text event" refers to any meta event of type `0x01` through `0x0F`.
- Any time `<angle brackets>` are used marks something that is not constant. The brackets are not part of the end result, they are used only as a variable name delimiter

### Audio Files

.mid doesn't have any way of naming specific audio files to be loaded for specific tracks. Instead, it uses the reserved file names listed [here](../Audio%20Files.md).

### Metadata

Song metadata is not stored within the .mid file itself. Instead, an accompanying song.ini file is used, the infrastructure for which is detailed [here](../song.ini/Core%20Infrastructure.md).

### Track Names

Tracks are identified by their track name meta event, which is the first event of each track.

Instrument track names usually follow a basic nomenclature of `PART <instrument name in all caps>`. For example, Guitar has a track name of `PART GUITAR`. Not all tracks follow this nomenclature, though.

Other tracks have no typical nomenclature, and just have a specific name.

Since any name can be used for a track, unknown tracks should be ignored.

### Track Events

Tracks can make use of any MIDI events as data for the track. Most tracks use note events and text events, with some using SysEx events as well, but other tracks are more complex and make use of channel numbers and velocity as well.

There are no fully standardized ways to lay out an instrument, only general guidelines and recommendations. Every instrument may have its own definitions for its notes, modifiers, etc. depending on its needs.

In addition to this, some tracks may have notes not specified in these docs, either due to error or due to incomplete documentation. Unknown events should be ignored.

### Track Difficulties

Usually, all difficulties of an instrument are contained within a single track. Difficulties typically span a single octave, with the lowest note of the octave being the first lane of the difficulty.

The common difficulty note ranges are as follows:

- Expert: 96-107
- Hard: 84-95
- Medium: 72-83
- Easy: 60-71

Some instruments don't adhere to these ranges though, and may use more ranges, different ranges, or even dedicated tracks for each difficulty. Some may also specify all-difficulty markers within the range of a specific difficulty, but these never conflict with notes.

### Other Track Data

Tracks may contain other data in the notes outside of the difficulty ranges. These can be anything from animation data to modifiers that apply across the whole track.

Typically, the top two octaves plus the few notes above it (notes 109-127) are reserved for this miscellaneous track data. Various notes below the difficulty ranges are often used as well.

Note that some song.ini tags affect how chart files should be parsed. These will be noted where relevant.

### Text Events

Text events refer to any meta events on a track. They are used to markup things that would be impractical to use notes for, such as venue parameters or character animation triggers. Typically, the text of these events are surrounded by \[square brackets\]. For simplicity, only the bracketed versions are listed in text event listings.

### Global and Local Events

Global events are text events placed on an `EVENTS` track. These events affect all tracks or define things that don't pertain to any specific instrument, such as practice mode sections.

Local events are text events placed on an instrument track. These events affect only the track they're placed on.

#### Basic Global Events

| Event Text         | Description              |
| :---------         | :----------              |
| `[section <name>]` | Marks the start point of a section, used by Practice mode and post-game summary.<br>Note that some of these events may have an underscore `_` instead of a space before the section name. |
| `[prc_<name>]`     | Same as the above.       |
| `[end]`            | Marks the end of a song. |

### Phase Shift SysEx Event Specification

The use of SysEx events in charts was introduced by Phase Shift, in an attempt to keep FoF(iX) from breaking with the addition of new charting features (which didn't work). This format is not likely to be extended in the future, as it's clunky and generally disliked.

The format they defined is as follows:

`'P' 'S' '\0' <Type> <Difficulty> <Modifier> <Value>`

- `'P' 'S' '\0'` is a 3-character ASCII string (3 bytes) which acts as a header.
- `Type` is the type of event that this event marks. Currently, there is only one type.

  | Value  | Description     |
  | :---:  | :----------     |
  | `0x00` | Modifier phrase |

- `Difficulty` is the difficulty that the event applies to.

  | Value  | Description                                                                      |
  | :---:  | :----------                                                                      |
  | `0x00` | Easy                                                                             |
  | `0x01` | Medium                                                                           |
  | `0x02` | Hard                                                                             |
  | `0x03` | Expert                                                                           |
  | `0xFF` | All difficulties<br>This value is only known to appear on the tap note event, it should be considered invalid for other events. |

- `Modifier` is the modification that the event applies.

  | Value  | Description                                                               |
  | :---:  | :----------                                                               |
  | `0x01` | 5/6-fret open note                                                        |
  | `0x02` | Pro Guitar slide up                                                       |
  | `0x03` | Pro Guitar slide down                                                     |
  | `0x04` | 5/6-fret tap note<br>May also apply to Pro Guitar? Needs to be confirmed. |
  | `0x05` | Real Drums hi-hat open                                                    |
  | `0x06` | Real Drums hi-hat pedal                                                   |
  | `0x07` | Real Drums snare rimshot                                                  |
  | `0x08` | Real Drums hi-hat sizzle                                                  |
  | `0x09` | Pro Guitar palm mute                                                      |
  | `0x0A` | Pro Guitar vibrato                                                        |
  | `0x0B` | Pro Guitar harmonic                                                       |
  | `0x0C` | Pro Guitar pinch harmonic                                                 |
  | `0x0D` | Pro Guitar bend                                                           |
  | `0x0E` | Pro Guitar accent                                                         |
  | `0x0F` | Pro Guitar pop                                                            |
  | `0x10` | Pro Guitar slap                                                           |
  | `0x11` | Real Drums yellow cymbal+tom                                              |
  | `0x12` | Real Drums blue cymbal+tom                                                |
  | `0x13` | Real Drums green cymbal+tom                                               |

- `Value` is the value for this event. As modifier events are the only event type defined, these are the only available values:

  | Value  | Description  |
  | :---:  | :----------  |
  | `0x00` | Phrase end   |
  | `0x01` | Phrase start |

## References

These are the references for all of the .mid docs.

Specifications for the MIDI protocol/format itself are available from the MIDI Association here:

- [MIDI 1.0 Protocol Specifications](https://www.midi.org/specifications/midi1-specifications)
- [Summary of MIDI 1.0 Messages](https://www.midi.org/specifications-old/item/table-1-summary-of-midi-message)
- [Standard MIDI File Specifications](https://www.midi.org/specifications/file-format-specifications/standard-midi-files)

Chart format info comes from these sources:

- [C3/Rock Band Network docs](http://docs.c3universe.com/rbndocs/index.php?title=Authoring).
- [Rhythm Gaming World forums: "General Thread for PRO Guitar/Bass Questions"](https://rhythmgamingworld.com/forums/topic/general-thread-for-pro-guitarbass-questions/).
- [Rhythm Gaming World forums: "Track Requirements: Pro Guitar/Bass"](https://rhythmgamingworld.com/forums/topic/track-requirements-pro-guitarbass/)
- [Phase Shift forums: "R.E.A.P.E.R. Plugin [5/8/2014] V7"](https://dwsk.proboards.com/thread/1652/plugin-5-8-2014-v7)
- [Phase Shift forums: "Song Standard Advancements"](https://dwsk.proboards.com/thread/404/song-standard-advancements)
- [ScoreHero forums: "Lighting/Band/Crowd Events & Notes (New lighting ideas.)"](https://www.scorehero.com/forum/viewtopic.php?t=20179).
- [ScoreHero forums: "Guitar Hero MIDI and VGS File Details"](https://www.scorehero.com/forum/viewtopic.php?t=1179)
- [ScoreHero forums: "GH2 Midi Encyclopedia / Dictionary"](https://www.scorehero.com/forum/viewtopic.php?t=5523)
- [FoFiX source code](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py)
- [Editor on Fire source code](https://github.com/raynebc/editor-on-fire/blob/3c385f0d668b3dfdc7c648377cf422b4e2671db5/src/midi.c)
