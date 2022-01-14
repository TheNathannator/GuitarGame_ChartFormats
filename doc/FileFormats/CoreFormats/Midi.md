# MIDI

A MIDI file is a file that stores a MIDI stream. It is a binary format and uses the `.mid` and `.midi` file extensions (`.midi` is not typically used for charts, however).

Modern .mid charts follow a format originating from Rock Band's .mid chart format, with some additional functionality added in by Phase Shift and Clone Hero. Older .mid charts are slightly different and are based on Guitar Hero 1/2's format, but follow similar patterns.

This document will be split up later into multiple documents, with one being a reference standard that only includes what matters for modern-day usage, and the others being more game-centric.

## Table of Contents

- [Basic Infrastructure](#basic-infrastructure)
  - [Number Conventions](#number-conventions)
  - [Chunks](#chunks)
    - [Header Chunks](#header-chunks)
    - [Track Chunks](#track-chunks)
  - [Events](#events)
    - [Event Bytes](#event-bytes)
  - [Event Types](#event-types)
    - [Channel Voice](#channel-voice)
    - [Channel Mode](#channel-mode)
    - [System Events](#system-events)
    - [System Common Events](#system-common-events)
    - [System Real-Time Events](#system-real-time-events)
    - [System Exclusive Events](#system-exclusive-events)
      - [Warning For SysEx](#warning-for-sysex)
    - [Meta Events](#meta-events)
      - [Type 1 MIDI Tempo Map Track](#type-1-midi-tempo-map-track)
- [Implementation Details](#implementation-details)
  - [Track Names](#track-names)
  - [Basic Info](#basic-info)
  - [Metadata](#metadata)
  - [SysEx Event Details](#sysex-event-details)
  - [5-Fret Tracks](#5-fret-tracks)
    - [5-Fret Notes](#5-fret-notes)
    - [5-Fret SysEx Events](#5-fret-sysex-events)
    - [5-Fret Text Events](#5-fret-text-events)
  - [6-Fret Tracks](#6-fret-tracks)
    - [6-Fret Notes](#6-fret-notes)
    - [6-Fret SysEx Events](#6-fret-sysex-events)
  - [Drums Tracks](#drums-tracks)
    - [Drums Notes](#drums-notes)
    - [Phase Shift Real Drums SysEx Events](#phase-shift-real-drums-sysex-events)
    - [Drums Text Events](#drums-text-events)
    - [Drums Track Type Determining](#drums-track-type-determining)
  - [Vocals Tracks](#vocals-tracks)
    - [Vocals Notes](#vocals-notes)
    - [Vocals Lyrics](#vocals-lyrics)
  - [Rock Band 3 Pro Keys Tracks](#rock-band-3-pro-keys-tracks)
    - [Pro Keys Notes](#pro-keys-notes)
  - [Phase Shift Real Keys Tracks](#phase-shift-real-keys-tracks)
    - [Real Keys Notes](#real-keys-notes)
  - [Rock Band 3 Pro Guitar/Bass Tracks](#rock-band-3-pro-guitarbass-tracks)
    - [Pro Guitar/Bass Notes and Channels](#pro-guitarbass-notes-and-channels)
    - [Phase Shift Pro Guitar/Bass SysEx Events](#phase-shift-pro-guitarbass-sysex-events)
  - [Phase Shift Dance Track](#phase-shift-dance-track)
    - [Dance Notes](#dance-notes)
  - [Events Track](#events-track)
    - [Events Common Text Events](#events-common-text-events)
  - [Rock Band Tracks](#rock-band-tracks)
  - [GH1 and 2 Tracks](#gh1-and-2-tracks)
- [References](#references)

## Basic Infrastructure

This section doesn't provide full technical details, but should give enough for a general idea of what's going on in a MIDI file.

__This section should not be used to build a MIDI parser from scratch, there are other references for MIDI that should be used instead, such as the MIDI Association's specifications linked in the [References section](#references).__

### Number Conventions

For the most part, numbers are stored in big-endian format.

Some numbers have a variable length, from 1 to 4 bytes. These numbers are stored with the first 7 bits (starting from the least significant bit) per byte storing the actual value, and the most significant bit of each byte set if it is not the last byte of the stored number (in other words, it is a flag indicating that there is another byte). This makes the largest possible number (after decoding) `0x0FFFFFFF`.

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

MIDI files are comprised of what are called "chunks". These chunks are distinct portions of data within the file, and are marked using the following format:

`<Type> <Length>`

- `Type` is a 4-character ASCII string indicating the chunk type.
- `Length` is the byte length of the chunk represented as a 32-bit big-endian number.

There are two types of chunks:

- Header chunks
- Track chunks

#### Header Chunks

Header chunks contain metadata about the MIDI file as a whole. Their format is as follows:

`"MThd" 06 <MIDI type> <Number of tracks> <Delta-time type/resolution>`

- Header chunks use the `MThd` type string.
- Header chunks are a static 6 bytes long.
- `MIDI type` is a 16-bit number indicating the type of MIDI file:
  - 0 - The file contains a single track.
  - 1 - The file contains one or more simultaneous tracks.
  - 2 - The file contains one or more sequential single-track patterns. These are not used in chart files.
- `Number of tracks` is a 16-bit number representing the number of tracks in the file. As this is only metadata, the actual number of tracks may be different than specified here.
- `Delta-time type/resolution` is a 16-bit number representing both what unit the resolution is measured in, and how many ticks the unit is divided in to. This is not used in chart files.
  - The most-significant bit (bit 15) is a boolean indicating the format:
    - `0` - Resolution is in ticks per quarter note.
      - The other 15 bits represent the resolution number.
    - `1` - Resolution is in ticks per SMPTE frame.
      - Bits 0-7 represent the resolution number.
      - Bits 8-14 represent the SMPTE number of the format to be used, with its sign flipped and stored as a negative two's compliment value. The value should correspond to the 4 standard SMPTE and MIDI time code formats:
        - `-24` - 24 frames per second
        - `-25` - 25 frames per second
        - `-29` - 29.97 frames per second (drop-frame)
        - `-30` - 30 frames per second

#### Track Chunks

Track chunks contain the actual MIDI data streams. Their format is as follows:

`"MTrk" <Length> <Event[]>`

- Track chunks use the `MTrk` type string.
- Track chunks do not have a static length.
- `Event[]` is one or more MIDI event.

In a format 1 file, the first track should be used exclusively for tempo, time signature, and SMPTE offset data.

### Events

MIDI events follow this general format:

`<Delta-time> <Status> <Data[]>`

- `Delta-time` is the time in ticks relative to either the start of the track, or to the previous event in the stream.
- `Status` is a byte indicating the event type.
  - MIDI files use running status, meaning that channel events can omit the status byte and re-use the status of a previous channel event. The first event must specify a status.
- `Data[]` is any number of bytes representing the data of the event.

#### Event Bytes

The byte values in events are split evenly into two categories: data bytes (`0x00`-`0x7F`), and status bytes (`0x80`-`0xFF`). Status bytes indicate the type of event, and data bytes contain the event's data.

Some System Exclusive events may contain data bytes higher than `0x7F`

The specific formats of events is beyond the scope of this guide, so they are not covered here.

### Event Types

Not all of these event types pertain to chart files, but they're listed anyways as a general overview.

#### Channel Voice

Channel voice events control a channel's voices. The bottom 4 bits of channel voice messages represent a 4-bit channel number, giving 16 channels per track (numbered 0-15 throughout this document). In the following table, this will be notated as `n` in the Byte column.

| Format                 | Name                    | Description |
| :-----                 | :---                    | :---------- |
| `8n <num> <vel>`       | Note On                 | Marks the start of a note.<br>`num` is a 7-bit note number from 0-127, `vel` is a 7-bit velocity value from 0-127.<br>A Note On with a velocity of 0 is equivalent to a Note Off. |
| `9n <num> <vel>`       | Note Off                | Marks the end of a note.<br>`num` is a 7-bit note number from 0-127, `vel` is a 7-bit velocity value from 0-127. |
| `An <num> <aft>`       | Polyphonic Key Pressure | An aftertouch value that modifies a note in some way.<br>`num` is a 7-bit note number from 0-127, `aft` is a 7-bit aftertouch value from 0-127. |
| `Bn <num> <val>`       | Control Change          | Changes the value of a control channel.<br>`num` is a 7-bit controller number from 0-127, and `val` is a 7-bit control value (0-127).<br>Controller numbers 120-127 are reserved for channel mode events, detailed later. |
| `Cn <prgm>`            | Program Change          | Changes the selected program (sound, voice, preset, etc.) of a channel.<br>`prgm` is a 7-bit program number from 0-127. |
| `Dn <aft>`             | Channel Pressure        | An aftertouch value that modifies a whole channel in some way.<br>`aft` is a 7-bit aftertouch value from 0-127. |
| `En <num> <bls> <bms>` | Pitch Bend              | Bends the pitch of a note.<br>`num` is a 7-bit note number, `bls` and `bms` are a 14-bit bend value from 0-16383, where `bls` are the least significant bits, and `bms` are the most significant bits.<br>Max negative is (LSB, MSB) 0, 0; center is 0, 64 (8,192); max positive is 127, 127 (16,383). |

#### Channel Mode

Channel mode events control parameters that affect a channel's response to voice events. These are special control change controllers, using the reserved 120-127 control channels. They are also encoded with a 4-bit channel number (0-15), but can only be sent and received on an instrument's main channel (referred to as its "Basic Channel").

`Bn <120-127> <val>`

| Format         | Name                  | Description                                                                             |
| :-----         | :---                  | :----------                                                                             |
| `Bn 120 0`     | All Sound Off         | Disables all notes and sets their volumes to zero.                                      |
| `Bn 121 <val>` | Reset All Controllers | Resets all control channels to their default state.<br>`val` should only be 0 unless otherwise allowed in a specific recommended practice. |
| `Bn 122 <val>` | Local Control         | Enables or disables a device's local controls.<br>`val` acts as a max/min boolean: enables with `FF`, disables with `00`. |
| `Bn 123 0`     | All Notes Off         | Turns off all currently active notes.                                                   |
| `Bn 124 0`     | Omni Off              | Sets a device to only respond to events on its basic channel. Also turns off all notes. |
| `Bn 125 0`     | Omni On               | Sets a device to respond to events on all channels. Also turns off all notes.           |
| `Bn 126 <val>` | Mono On (Poly Off)    | Sets a device to respond monophonically to notes on a channel. Also turns off all notes.<br>`val` is either 0 if Omni is on, or the number of channels to be allowed starting from the basic channel if Omni is off. |
| `Bn 127 0`     | Poly On (Mono Off)    | Sets a device to respond polyphonically to notes on all channels. mode. Also turns off all notes. |

#### System Events

System events affect the whole MIDI stream and are not specific to a channel (i.e. they are received on all channels). There are three types of system events: Common, Real-Time, and Exclusive.

System events, with the exception of System Exclusive events, are not supported in MIDI files, but may be escaped through a special form of Exclusive events, for purposes such as transmission to a physical MIDI device.

#### System Common Events

Common events are used for non-real-time messages such as synchronization before playback, specifying a song to be played, and requesting tuning of an analog synth. Except for the End of Exclusive even (covered in the [SysEx section](#system-exclusive-events)), these are not supported in MIDI files, so they will not be covered here. (End of Exclusive is .)

#### System Real-Time Events

Real-Time events are used for real-time synchronization and playback. These are also not supported in MIDI files, so they will not be covered here.                           |

#### System Exclusive Events

System Exclusive (often shortened as SysEx) events are flexible, and aside from the starting and ending bytes, and a manufacturer/device ID, their format is vendor-defined. They can contain any number of data bytes, and they are the only System events allowed in MIDI files.

| Format                 | Event Name          | Description                                    |
| :----:                 | :---------          | :----------                                    |
| `F0 <len> <byte[]> F7` | Standard SysEx      | Sends a SysEx event.<br>`F0` starts a SysEx event, `F7` ends a SysEx event. `len` is the number of bytes in the event including the `F7`, `byte[]` is one or more bytes.<br>The first 1 or 3 bytes are typically a numeric ID. If the first byte is 0, the next two are the ID, otherwise just the first is. |
| `F7 <len> <byte[]>`    | SysEx (No end byte) | Sends a SysEx event without using an ending byte.

There are some Universal SysEx events defined in the MIDI Association's documentation, but for simplicity's sake these will not be documented here.

##### Warning For SysEx

Standard SysEx events normally do not allow data byte values above `0x7F`, however some SysEx events used in charts violate this, so this must be accounted for in MIDI parsing.

#### Meta Events

Meta events are events that store non-MIDI data such as text. They follow this format:

`FF <type> <length> <data[]>`

- `type` is a 1-byte type code of the meta event.
- `length` is a variable-length number representing the byte length of `data[]`.
- `data[]` is any number of data bytes contained in the event.

| Format                                  | Event Name          | Description |
| :----:                                  | :---------          | :---------- |
| `FF 00 02 <num>`                        | Sequence number     | An optional track sequence number.<br>Must occur at the beginning of a track if used.<br>`num` is the sequence number as a 16-bit number. |
| `FF 01 <len> <text[]>`                  | Text Event          | Any amount of text.<br>`text` is any amount of text (typically ASCII, but may be another format) |
| `FF 02 <len> <text[]>`                  | Copyright notice    | A copyright notice.                                                    |
| `FF 03 <len> <text[]>`                  | Sequence/track name | The name of a sequence or track.                                       |
| `FF 04 <len> <text[]>`                  | Instrument name     | A description of the instrumentation for the track.                    |
| `FF 05 <len> <text[]>`                  | Lyric               | A lyric, typically a single syllable.                                  |
| `FF 06 <len> <text[]>`                  | Marker              | Marks a point in a sequence.                                           |
| `FF 07 <len> <text[]>`                  | Cue point           | Describes an event happening at this point in an accompanying media.   |
| `FF 20 01 <chn>`                        | MIDI channel prefix | Associates a MIDI channel to all following events, up until the next channel prefix event.<br>`chn` is any channel number from 0-15. |
| `FF 2F 00`                              | End of track        | Marks the exact ending point of a track. This event is *not* optional. |
| `FF 51 03 <tmp>`                        | Set tempo           | Sets a new tempo.<br>`tmp` is the tempo as a 24-bit number in microseconds per quarter note. |
| `FF 54 05 <hr> <min> <sec> <frm> <frc>` | SMPTE offset        | Sets an SMPTE time that a track should start at.<br>Every value is 1 byte: `hr` is the hour, `min` is the minute, `sec` is the second, `frm` is the frame, and `frc` is the fractional frame in 1/100ths of a frame. |
| `FF 58 04 <num> <den> <clk> <base>`     | Time signature      | Sets a new time signature.<br>`num` is the numerator, `den` is a power-of-2 exponent for the denominator, `clk` is the number of MIDI clocks in a metronome click, `base` is the number of notated 32nd notes per MIDI quarter note. |
| `FF 59 02 <shp/flt> <maj/min>`          | Key signature       | Sets a new key signature.<br>`shp/flat` is the number of sharps or flats from -7 to 7 (positive is sharps, negative is flats, 0 is none), `maj/min` is 0 for major, 1 for minor. |
| `FF 7F <len> <data[]>`                  | Sequencer-specific  | Stores a sequencer-specific meta event.                                |

##### Type 1 MIDI Tempo Map Track

Typically, a type 1 MIDI file dedicates its first track to tempo map data and other things that affect all tracks, with the rest of the tracks being normal tracks.

## Implementation Details

The following sections detail per-track tables and lists of MIDI notes, text events, and SysEx events that lay out charts.

Notes are listed in descending order to match how a piano roll view typically lays notes out. Channels and SysEx events are listed in ascending order. Note and channel lists are 0-indexed.

Please note that these should not be used as a comprehensive technical documentation for how charts should be made for Rock Band or Guitar Hero 1/2, this document is focused on the format itself and does not include most game-specific events, limitations, or technicalities. Other documents are included in this repo for that purpose.

There may also be some things missing here and there, but the core functionality for each track should be fully documented. Given the nature of .mid files being able to contain tracks and events far beyond the ones listed, unexpected tracks, notes, and other events should be ignored.

If adding new functionality to existing tracks is desired, make sure to check the other game documents to check that the notes/events to be used are not already occupied by those games. Also consider documenting those additions and submitting them here so others are aware of them.

Abbreviations that refer to specific games or entities may be found throughout this document:

- RB - Rock Band
- RBN - Rock Band Network
- GH - Guitar Hero
- PS - Phase Shift
- CH - Clone Hero
- C3 - Custom Creators Collective
- RGW - Rhythm Gaming World

### Track Names

Tracks are identified by a track name meta event at the start of the track.

Some of the tracks listed here are not documented in this particular document, rather they are game-specific and documented in that game's .mid document.

Standard tracks:

| Track Name                           | Track Description            |
| :---------                           | :----------------            |
| [`PART GUITAR`](#5-fret-tracks)      | Lead Guitar                  |
| [`PART GUITAR COOP`](#5-fret-tracks) | Co-op Guitar                 |
| [`PART GUITAR GHL`](#6-fret-tracks)  | Guitar Hero Live Guitar      |
| [`PART BASS`](#5-fret-tracks)        | Bass Guitar                  |
| [`PART BASS GHL`](#6-fret-tracks)    | Guitar Hero Live Bass        |
| [`PART RHYTHM`](#5-fret-tracks)      | Rhythm Guitar                |
| [`PART KEYS`](#5-fret-tracks)        | 5-Lane Keys                  |
| [`PART DRUMS`](#drums-tracks)        | Drums/Pro Drums/5-Lane Drums |
| [`PART VOCALS`](#vocals-tracks)      | Vocals/lyrics                |
| [`EVENTS`](#events-track)            | Global events                |

Additional tracks (from either Rock Band or Phase Shift):

| Track Name                                                                         | Track Description                      |
| :---------                                                                         | :----------------                      |
| [`PART DRUMS_2X`]                                                                  | RBN 2x Kick Drums chart<br>Meant for generating separate 1x and 2x kick files for Rock Band. Probably won't ever be seen in a publicly-available chart, but worth noting. |
| [`PART REAL_GUITAR`](#rock-band-3-pro-guitar-tracks)                               | RB3 Pro Guitar (17-fret)               |
| [`PART REAL_GUITAR_22`](#rock-band-3-pro-guitar-tracks)                            | RB3 Pro Guitar (22-fret)               |
| [`PART REAL_GUITAR_BONUS`](#rock-band-3-pro-guitar-tracks)                         | Some Pro Guitar track, details unknown |
| [`PART REAL_BASS`](#rock-band-3-pro-guitar-tracks)                                 | RB3 Pro Bass (17-fret)                 |
| [`PART REAL_BASS_22`](#rock-band-3-pro-guitar-tracks)                              | RB3 Pro Bass (22-fret)                 |
| [`PART REAL_DRUMS_PS`](#drums-tracks)                                              | PS Real Drums                          |
| [`PART REAL_KEYS_X`](#rock-band-3-pro-keys-tracks)                                 | RB3 Pro Keys Expert                    |
| [`PART REAL_KEYS_H`](#rock-band-3-pro-keys-tracks)                                 | RB3 Pro Keys Hard                      |
| [`PART REAL_KEYS_M`](#rock-band-3-pro-keys-tracks)                                 | RB3 Pro Keys Medium                    |
| [`PART REAL_KEYS_E`](#rock-band-3-pro-keys-tracks)                                 | RB3 Pro Keys Easy                      |
| [`PART KEYS_ANIM_LH`](../RockBand/Midi_RockBand.md#pro-keys-animation-track-notes) | RB3 Pro Keys left-hand animations      |
| [`PART KEYS_ANIM_RH`](../RockBand/Midi_RockBand.md#pro-keys-animation-track-notes) | RB3 Pro Keys right-hand animations     |
| [`PART REAL_KEYS_PS_X`](#phase-shift-real-keys-tracks)                             | PS Keys Real Expert                    |
| [`PART REAL_KEYS_PS_M`](#phase-shift-real-keys-tracks)                             | PS Keys Real Medium                    |
| [`PART REAL_KEYS_PS_H`](#phase-shift-real-keys-tracks)                             | PS Keys Real Hard                      |
| [`PART REAL_KEYS_PS_E`](#phase-shift-real-keys-tracks)                             | PS Keys Real Easy                      |
| [`PART DANCE`](#phase-shift-dance-track)                                           | PS 4-key dance                         |
| [`HARM1`](#vocals-tracks)                                                          | RB3 Harmony part 1                     |
| [`HARM2`](#vocals-tracks)                                                          | RB3 Harmony part 2                     |
| [`HARM3`](#vocals-tracks)                                                          | RB3 Harmony part 3                     |
| [`VENUE`](../RockBand/Midi_RockBand.md#venue-track)                                | RB venue track                         |
| [`BEAT`](../RockBand/Midi_RockBand.md#beat-track)                                  | RB upbeat/downbeat track               |

Legacy tracks:

| Track Name                                                                | Track Description                                    |
| :---------                                                                | :----------------                                    |
| [`T1 GEMS`](#5-fret-tracks)                                               | GH1 Lead Guitar                                      |
| [`ANIM`](../GuitarHero1-2/Midi_GuitarHero1-2.md#anim-track)               | GH1 guitarist animations track                       |
| [`TRIGGERS`](../GuitarHero1-2/Midi_GuitarHero1-2.md#triggers-track)       | GH1/2 lighting, venue, and practice mode drums track |
| [`BAND_BASS`](../GuitarHero1-2/Midi_GuitarHero1-2.md#band-bass-track)     | GH1/2 bassist animations track                       |
| [`BAND_DRUM`](../GuitarHero1-2/Midi_GuitarHero1-2.md#band-drum-track)     | GH1/2 drummer animations track                       |
| [`BAND_SINGER`](../GuitarHero1-2/Midi_GuitarHero1-2.md#band-singer-track) | GH1/2 vocalist animations track                      |
| [`Click`]                                                                 | Track that FoFiX supports for Lead Guitar            |
| [`Midi Out`]                                                              | Same as above                                        |
| [`PART LEAD`]                                                             | Same as above                                        |
| [`PART DRUM`]                                                             | Alternate name for `PART DRUMS` that FoFiX supports  |
| [`PART REAL GUITAR`]                                                      | `PART REAL_GUITAR` as it appears in FoFiX's code     |
| [`PART REAL BASS`]                                                        | `PART REAL_BASS` as it appears in FoFiX's code       |
| [`PART REAL DRUM`]                                                        | Pro Drums as it appears in FoFiX's code              |

### Basic Info

This section defines some nomenclature common in each section, and some specifics on how some things work.

Each track consists of various MIDI notes that serve some purpose, along with text events and possibly SysEx events. Unknown MIDI events should be ignored.

- Markers refer to a note that marks either a phrase or a modification to a note.
  - Phrase markers typically do not include notes on the same tick as their Note Off, meaning that if a Star Power phrase ends on tick 4800, it will not affect notes that start on tick 4800.
  - Note markers typically will affect all notes under their range, not just ones that start at the same time or match their length. They also typically do not include the Note Off in their affected range.

### Metadata

Metadata for .mid charts is not stored within the .mid file itself; rather, it is contained in an accompanying song.ini file, documented in [Song_ini.md](Song_ini.md).

### SysEx Event Details

SysEx events are used in some chart files to specify modifications to notes. These originate from Phase Shift, and follow this format:

`50 53 00 <message> <difficulty> <type> <value>`

- `50 53 00` is an identifier. `50 53` is the hexadecimal ASCII representation of the letters `PS`.
- `message` is the message type. Currently, there is only one message type.
  - `00` - Phrase
- `difficulty` is the difficulty this event affects as a hexadecimal number.

| Value | Description                                                   |
| :---: | :----------                                                   |
| `00`  | Easy                                                          |
| `01`  | Medium                                                        |
| `02`  | Hard                                                          |
| `03`  | Expert                                                        |
| `FF`  | All difficulties<br>This value violates standard SysEx specs. |

- `type` is the modification type code.

| Value | Description                     |
| :---: | :----------                     |
| `01`  | 5/6-fret open note              |
| `02`  | Pro Guitar slide up             |
| `03`  | Pro Guitar slide down           |
| `04`  | 5/6-fret/Pro Guitar(?) tap note |
| `05`  | Real Drums hi-hat open          |
| `06`  | Real Drums hi-hat pedal         |
| `07`  | Real Drums snare rimshot        |
| `08`  | Real Drums hi-hat sizzle        |
| `09`  | Pro Guitar palm mute            |
| `0A`  | Pro Guitar vibrato              |
| `0B`  | Pro Guitar harmonic             |
| `0C`  | Pro Guitar pinch harmonic       |
| `0D`  | Pro Guitar bend                 |
| `0E`  | Pro Guitar accent               |
| `0F`  | Pro Guitar pop                  |
| `10`  | Pro Guitar slap                 |
| `11`  | Real Drums yellow cymbal+tom    |
| `12`  | Real Drums blue cymbal+tom      |
| `13`  | Real Drums green cymbal+tom     |

- `value` is the value for this event.
  - For phrase messages, this is either `01` to start the phrase, or `01` to end the phrase.

### 5-Fret Tracks

- `T1 GEMS` - GH1 Lead Guitar
- `PART GUITAR` - Lead Guitar
- `PART GUITAR COOP` - Co-op Guitar
- `PART BASS` - Bass Guitar
- `PART RHYTHM` - Rhythm Guitar
- `PART KEYS` - 5-Lane Keys
  - 5-lane keys gets its own sub-section later because while it could be treated as just another 5-fret track to allows things like forcing, RB3 probably does not expect/allow forcing and the like.

#### 5-Fret Notes

This list excludes some notes that only matter for Rock Band or Guitar Hero 1/2.

| MIDI Note | Description                                                                                                   |
| :-------: | :----------                                                                                                   |
| Markers   |                                                                                                               |
| 127       | Trill lane marker<br>Only applies to Expert unless velocity is 50-41, then it will show up on Hard as well.   |
| 126       | Tremolo lane marker<br>Only applies to Expert unless velocity is 50-41, then it will show up on Hard as well. |
| 116       | Star Power/Overdrive marker                                                                                   |
| 104       | Tap note marker                                                                                               |
| 103       | Solo marker, or Star Power if no 116 notes exist for legacy compatibility                                     |
|           |                                                                                                               |
| Expert    |                                                                                                               |
| 102       | Expert force strum                                                                                            |
| 101       | Expert force HOPO                                                                                             |
| 100       | Expert Orange (5th lane)                                                                                      |
| 99        | Expert Blue (4th lane)                                                                                        |
| 98        | Expert Yellow (3rd lane)                                                                                      |
| 97        | Expert Red (2nd lane)                                                                                         |
| 96        | Expert Green (1st lane)                                                                                       |
| 95        | Expert Open<br>Only available if there's an `[ENHANCED_OPENS]`/`ENHANCED_OPENS` text event on the track.      |
|           |                                                                                                               |
| Hard      |                                                                                                               |
| 90        | Hard force strum                                                                                              |
| 89        | Hard force HOPO                                                                                               |
| 88        | Hard Orange (5th lane)                                                                                        |
| 87        | Hard Blue (4th lane)                                                                                          |
| 86        | Hard Yellow (3rd lane)                                                                                        |
| 85        | Hard Red (2nd lane)                                                                                           |
| 84        | Hard Green (1st lane)                                                                                         |
| 83        | Hard Open<br>Only available if there's an `[ENHANCED_OPENS]`/`ENHANCED_OPENS` text event on the track.        |
|           |                                                                                                               |
| Medium    |                                                                                                               |
| 78        | Medium force strum                                                                                            |
| 77        | Medium force HOPO                                                                                             |
| 76        | Medium Orange (5th lane)                                                                                      |
| 75        | Medium Blue (4th lane)                                                                                        |
| 74        | Medium Yellow (3rd lane)                                                                                      |
| 73        | Medium Red (2nd lane)                                                                                         |
| 72        | Medium Green (1st lane)                                                                                       |
| 71        | Medium Open<br>Only available if there's an `[ENHANCED_OPENS]`/`ENHANCED_OPENS` text event on the track.      |
|           |                                                                                                               |
| Easy      |                                                                                                               |
| 66        | Easy force strum                                                                                              |
| 65        | Easy force HOPO                                                                                               |
| 64        | Easy Orange (5th lane)                                                                                        |
| 63        | Easy Blue (4th lane)                                                                                          |
| 62        | Easy Yellow (3rd lane)                                                                                        |
| 61        | Easy Red (2nd lane)                                                                                           |
| 60        | Easy Green (1st lane)                                                                                         |
| 59        | Easy Open<br>Only available if there's an `[ENHANCED_OPENS]`/`ENHANCED_OPENS` text event on the track. Otherwise, part of the RB/GH2 left hand position animation data. |

Additional information:

- Notes must get set as HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord.
  - In .mid, the default threshold is a 1/12th step (though sources elsewhere say the threshold is, assuming a 480 tick resolution, a 1/16th (120 ticks) step or 170 ticks instead of 160, but the currently accepted threshold is a 1/12th step). This can be changed through the song.ini tag `hopo_frequency`.
  - Notes can be forced as either strums or HOPOs using the Force Strum and Force HOPO markers. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.
    - (TODO: Figure out which gets precedence and note it here) 
- Sustains shorter than a 1/12th step must be cut off and turned into a normal note. This allows charters using a DAW to not have to make their notes 1 tick long in order for it to not be a sustain.
  - This can be changed using the `sustain_cutoff_threshold` song.ini tag, though this is mainly meant for game rips where some notes that should be sustains get cut off due to being too short.
- Trill and tremolo lanes are used to make imprecise/indiscernible trills or fast strumming easier to play. They prevent overstrumming and only require you to hit faster than a certain threshold to hit the charted notes.
- Chords can have individual notes with different lengths. These are referred to as extended sustains (start at different times, end at same or different times) and disjoint chords (start at the same time, end at different times)
- As a note: Some older .mid charts contain notes that are almost on the same position, but not quite. Guitar Hero 1/2 and Rock Band have a mechanism to snap notes with a position difference of 10 ticks or less (regardless of resolution) together as a chord, with the position to snap to being the start position of the earliest note within the chord. __This is only noted here in the core document as a note/warning, doing this snapping is not particularly recommended for games since it'll rarely be an issue and sloppy charting should not be encouraged, since not all games support this.__
- For maximum compatibility with programs, the SysEx-based markers for tap notes and open notes should be used when writing charts instead of the note-based markers, as the note-based markers are relatively newer and not supported by much yet.

#### 5-Fret SysEx Events

| Description | SysEx data                                     |
| :---------- | :---------                                     |
| Open notes  | `50 53 00 00 <difficulty> 01 <enable/disable>` |
| Tap notes   | `50 53 00 00 FF 04 <enable/disable>`           |

#### 5-Fret Text Events

| Event Text                          | Description                           |
| :---------                          | :----------                           |
| `[ENHANCED_OPENS]`/`ENHANCED_OPENS` | Enables note-based open note marking. |

### 6-Fret Tracks

- `PART GUITAR GHL` - 6-Fret Lead Guitar
- `PART BASS GHL` - 6-Fret Bass Guitar

#### 6-Fret Notes

| MIDI Note | Description                 |
| :-------: | :----------                 |
| Markers   |                             |
| 116       | Star Power/Overdrive marker |
| 104       | Tap note marker             |
| 103       | Solo marker                 |
|           |                             |
| Expert    |                             |
| 102       | Expert force strum          |
| 101       | Expert force HOPO           |
| 100       | Expert Black 3 (3rd lane)   |
| 99        | Expert Black 2 (2nd lane)   |
| 98        | Expert Black 1 (1st lane)   |
| 97        | Expert White 3 (6th lane)   |
| 96        | Expert White 2 (5th lane)   |
| 95        | Expert White 1 (4th lane)   |
| 94        | Expert Open                 |
|           |                             |
| Hard      |                             |
| 90        | Hard force strum            |
| 89        | Hard force HOPO             |
| 88        | Hard Black 3 (3rd lane)     |
| 87        | Hard Black 2 (2nd lane)     |
| 86        | Hard Black 1 (1st lane)     |
| 85        | Hard White 3 (6th lane)     |
| 84        | Hard White 2 (5th lane)     |
| 83        | Hard White 1 (4th lane)     |
| 82        | Hard Open                   |
|           |                             |
| Medium    |                             |
| 78        | Medium force strum          |
| 77        | Medium force HOPO           |
| 76        | Medium Black 3 (3rd lane)   |
| 75        | Medium Black 2 (2nd lane)   |
| 74        | Medium Black 1 (1st lane)   |
| 73        | Medium White 3 (6th lane)   |
| 72        | Medium White 2 (5th lane)   |
| 71        | Medium White 1 (4th lane)   |
| 70        | Medium Open                 |
|           |                             |
| Easy      |                             |
| 66        | Easy force strum            |
| 65        | Easy force HOPO             |
| 64        | Easy Black 3 (3rd lane)     |
| 63        | Easy Black 2 (2nd lane)     |
| 62        | Easy Black 1 (1st lane)     |
| 61        | Easy White 3 (6th lane)     |
| 60        | Easy White 2 (5th lane)     |
| 59        | Easy White 1 (4th lane)     |
| 58        | Easy Open                   |

Additional information:

- 6-fret follows all of the same rules as 5-fret, with the following exclusions:
  - The `[ENHANCED_OPENS]` text event is *not* required for 6-fret note-based open notes.
  - Chord snapping is not observed as a mechanism in Guitar Hero Live.

#### 6-Fret SysEx Events

These are from Phase Shift for 5-fret, and carried over to 6-fret by Clone Hero:

| Description | SysEx data                                     |
| :---------- | :---------                                     |
| Open notes  | `50 53 00 00 <difficulty> 01 <enable/disable>` |
| Tap notes   | `50 53 00 00 FF 04 <enable/disable>`           |

### Drums Tracks

- `PART DRUMS` - Standard 4-Lane, 4-Lane Pro, and 5-Lane Drums
- `PART DRUM` - Alternate track to `PART DRUMS` that FoFiX supports
- `PART DRUMS_2X` - RBN 2x Kick drums chart
- `PART REAL_DRUMS_PS` - Phase Shift's Real Drums

#### Drums Notes

| MIDI Note | Description                                                                                             |
| :-------: | :----------                                                                                             |
| Markers   |                                                                                                         |
| 127       | 2-lane roll marker<br>Only applies to Expert unless velocity is 50-41, then it will be on Hard as well. |
| 126       | 1-lane roll marker<br>Only applies to Expert unless velocity is 50-41, then it will be on Hard as well. |
| 124       | Fill marker 1                                                                                           |
| 123       | Fill marker 2                                                                                           |
| 122       | Fill marker 3                                                                                           |
| 121       | Fill marker 4                                                                                           |
| 120       | Fill marker 5<br>All 5 must be used as a chord to mark an SP activation fill.                           |
| 116       | Star Power/Overdrive marker                                                                             |
| 112       | Green tom marker                                                                                        |
| 111       | Blue tom marker                                                                                         |
| 110       | Yellow tom marker                                                                                       |
| 109       | Flam marker<br>Should be ignored if the note is already a chord, not including kicks.                   |
| 103       | Solo marker                                                                                             |
|           |                                                                                                         |
| Expert    |                                                                                                         |
| 101       | Expert 5-lane Orange (5th lane)                                                                         |
| 100       | Expert 4-lane Green/5-lane Orange (4th lane)                                                            |
| 99        | Expert Blue (3rd lane)                                                                                  |
| 98        | Expert Yellow (2nd lane)                                                                                |
| 97        | Expert Red (1st lane)                                                                                   |
| 96        | Expert Kick                                                                                             |
| 95        | Expert+ Kick                                                                                            |
|           |                                                                                                         |
| Hard      |                                                                                                         |
| 89        | Hard 5-lane Orange (5th lane)                                                                           |
| 88        | Hard 4-lane Green/5-lane Orange (4th lane)                                                              |
| 87        | Hard Blue (3rd lane)                                                                                    |
| 86        | Hard Yellow (2nd lane)                                                                                  |
| 85        | Hard Red (1st lane)                                                                                     |
| 84        | Hard Kick                                                                                               |
|           |                                                                                                         |
| Medium    |                                                                                                         |
| 77        | Medium 5-lane Orange (5th lane)                                                                         |
| 76        | Medium 4-lane Green/5-lane Orange (4th lane)                                                            |
| 75        | Medium Blue (3rd lane)                                                                                  |
| 74        | Medium Yellow (2nd lane)                                                                                |
| 73        | Medium Red (1st lane)                                                                                   |
| 72        | Medium Kick                                                                                             |
|           |                                                                                                         |
| Easy      |                                                                                                         |
| 65        | Easy 5-lane Orange (5th lane)                                                                           |
| 64        | Easy 4-lane Green/5-lane Orange (4th lane)                                                              |
| 63        | Easy Blue (3rd lane)                                                                                    |
| 62        | Easy Yellow (2nd lane)                                                                                  |
| 61        | Easy Red (1st lane)                                                                                     |
| 60        | Easy Kick                                                                                               |

Additional info:

- 4-Lane:
  - Notes are cymbals by default in .mid. Toms are marked using notes 110-112, excluding red which is always a tom note.
    - This makes it impossible to have a tom and cymbal of the same color at the same position, unless the respective tom+cymbal SysEx event is enabled.
  - Roll lanes are used to make imprecise/indiscernible fast rhythms such as drum rolls or cymbal swells easier to play. They prevent overhitting and only require you to hit faster than a certain threshold to hit the charted notes.
    - During parsing, if a 1-lane roll starts on a chord (excluding kicks), the lane should be marked on the lane matching the next non-chord note.
    - Roll lanes cannot be applied to kicks.
- 5-lane:
  - Red, blue, and green are toms, yellow and orange are cymbals.
  - Drum sustains are used for imprecise/indiscernible fast rhythms such as drum rolls or cymbal swells. They prevent overhitting and require you to hit faster than a certain threshold (around 4 hits per second in GH) to maintain the sustain.
  - Unlike 4-lane roll lanes, kicks can be drum sustains.
- Expert+ kicks are used for double-kick sections or kicks faster than around 5-6 kicks per second, alternating these faster kicks between normal and Expert+ to make these sections playable with a single pedal.
  - In gameplay, they are equivalent to normal kicks, but they should only be visible if the user enables them.
- Accent and ghost notes are notes that need to be hit either hard or soft, respectively.
  - A note marked at a velocity of 127 is an accent note, and a note at a velocity of 1 is a ghost note.
  - Clone Hero requires an `[ENABLE_CHART_DYNAMICS]`/`ENABLE_CHART_DYNAMICS` text event to be present to enable ghosts/accents, in order to preserve compatibility with charts that weren't charted with velocity in mind. Using this approach is recommended.

#### Phase Shift Real Drums SysEx Events

| Description         | SysEx data                                     |
| :----------         | :---------                                     |
| Hi-Hat Open         | `50 53 00 00 <difficulty> 05 <enable/disable>` |
| Hi-Hat Pedal        | `50 53 00 00 <difficulty> 06 <enable/disable>` |
| Rimshot             | `50 53 00 00 <difficulty> 07 <enable/disable>` |
| Hi-Hat Sizzle       | `50 53 00 00 <difficulty> 08 <enable/disable>` |
| Yellow Cymbal + Tom | `50 53 00 00 <difficulty> 11 <enable/disable>` |
| Blue Cymbal + Tom   | `50 53 00 00 <difficulty> 12 <enable/disable>` |
| Green Cymbal + Tom  | `50 53 00 00 <difficulty> 13 <enable/disable>` |

#### Drums Text Events

| Event Text                | Description                                                                   |
| :---------                | :----------                                                                   |
| `[ENABLE_CHART_DYNAMICS]` | Enables accent/ghost marking.<br>Can be found both with and without brackets. |

Mix events:

`[mix <difficulty> drums<configuration><flag>]` - Sets a stem configuration for drums. This event can be found both with or without brackets, and with either spaces or underscores.

- `difficulty` is a number indicating the difficulty this event applies to:

| Text | Description |
| :--: | :---------- |
| `0`  | Easy        |
| `1`  | Medium      |
| `2`  | Hard        |
| `3`  | Expert      |

- `configuration` is a number corresponding to a specific stem configuration:

| Text | Description                                                                      |
| :--: | :----------                                                                      |
| `0`  | One stereo stem for the entire kit.<br>This should be used for no stem, as well. |
| `1`  | Mono kick, mono snare, stereo other.                                             |
| `2`  | Mono kick, stereo snare, stereo other.                                           |
| `3`  | Stereo kick, snare, and other.                                                   |
| `4`  | Mono kick, stereo other.                                                         |
| `5`  | Stereo kick, snare, toms, and cymbals.<br>Not part of the RBN docs, rather it is [defined as a community standard](https://strikeline.myjetbrains.com/youtrack/issue/CH-43) based on the GH stem layout. |

- `flag` is an optional string added to the end of the event that applies a modification outside of setting up the mix:

| Text         | Description |
| :--:         | :---------- |
| `d`          | Known as Disco Flip.<br>- On non-Pro, swaps the the snare and other/cymbal+tom stems, such that snare is activated by yellow, and other/cymbal+tom are activated by red.<br>- On Pro Drums, flips yellow cymbal/yellow tom and red notes to restore proper playing of the part with cymbals instead of swapping the stems.<br>- This flag is used in sections that require alternating the left and right hands quickly on the hi-hat (commonly a disco beat, hence the name), which are typically charted swapped, since playing it un-swapped in non-Pro on stock Rock Band kits tends to be uncomfortable. |
| `dnoflip`    | Swaps the snare and kit/cymbal and tom stems on both non-Pro and Pro Drums, without swapping red and yellow in Pro Drums.<br>- Used in sections where notes should not be flipped in Pro Drums, but the snare and kit/cymbal stems should still be swapped.
| `easy`       | Unmutes the tom and cymbal stems on Easy.<br>- Used in sections where there are no tom or cymbal notes to unmute the kit stem or tom/cymbal stems. |
| `easynokick` | Unmutes the other/cymbal+tom stem on Easy.<br>- Used in sections where there are no kick notes to unmute the kick stem. |

Aside from `d` and `dnoflip`, detecting stem configuration can be done automatically and these mix events can be ignored if that approach is chosen. If these events are used for more than just disco flip in a chart, then the `configuration` number should remain constant throughout the chart.

#### Drums Track Type Determining

The Drums track doesn't have any way of specifying which kind of drums track it is from within the track itself, its type must be determined manually. The same goes for .chart files.

The type can be determined using a process such as the following:

- Check if an accompanying song.ini has either the `pro_drums` or `five_lane_drums` tags. If it does, then force the drums track to be parsed as if it were that type.
- If there is no song.ini, or if the tags to force a type do not exist, check the chart for 5-lane green and cymbal markers.
- If both 5-lane and Pro are detected, it may be preferable to prioritize Pro over 5-lane.

Additionally, if converting 5-lane to 4-lane Pro or vice versa is desired, here are some suggested conversions:

- 5-lane to 4-lane Pro:

| 5-lane | 4-lane Pro    |
| :----- | :---------    |
| Red    | Red           |
| Yellow | Yellow cymbal |
| Blue   | Blue tom      |
| Orange | Green cymbal  |
| Green  | Green tom     |
| O + G  | G cym + B tom |

- 4-lane Pro to 5-lane:

| 4-lane Pro    | 5-lane |
| :---------    | :----- |
| Red           | Red    |
| Yellow cymbal | Yellow |
| Yellow tom    | Blue   |
| Blue cymbal   | Orange |
| Blue tom      | Blue   |
| Green cymbal  | Orange |
| Green tom     | Green  |
| Y tom + B tom | R + B  |
| B cym + G cym | Y + O  |

### Vocals Tracks

- `PART VOCALS` - Standard vocals track
- `HARM1` - Harmonies track 1
- `HARM2` - Harmonies track 2
- `HARM3` - Harmonies track 3

#### Vocals Notes

| MIDI Note  | Description                                                                                                         |
| :-------:  | :----------                                                                                                         |
| Markers    |                                                                                                                     |
| 116        | Star Power/Overdrive marker<br>Standard vocals and Harmonies can have independent overdrive. `HARM2` and `HARM3` get their overdrive from `HARM1`. |
| 106        | Lyrics phrase marker 2<br>Can appear with the other phrase marker at the same time (same start and end). |
| 105        | Lyrics phrase marker 1<br>Can appear with the other phrase marker at the same time (same start and end).<br>For harmonies, the `HARM1` phrase is used for all 3 harmony tracks. The `HARM2` phrase is only used to mark when harmony 2/3 lyrics shift in static vocals. `HARM3` typically has no phrases marked. |
|            |                                                                                                                     |
| Percussion |                                                                                                                     |
| 97         | Not displayed percussion                                                                                            |
| 96         | Displayed percussion                                                                                                |
|            |                                                                                                                     |
| Notes      |                                                                                                                     |
| 84         | C6 (Highest)                                                                                                        |
| 83         | B5                                                                                                                  |
| 82         | Bb5                                                                                                                 |
| 81         | A5                                                                                                                  |
| 80         | G#5                                                                                                                 |
| 79         | G5                                                                                                                  |
| 78         | F#5                                                                                                                 |
| 77         | F5                                                                                                                  |
| 76         | E5                                                                                                                  |
| 75         | Eb5                                                                                                                 |
| 74         | D5                                                                                                                  |
| 73         | C#5                                                                                                                 |
| 72         | C5                                                                                                                  |
| 71         | B4                                                                                                                  |
| 70         | Bb4                                                                                                                 |
| 69         | A4                                                                                                                  |
| 68         | G#4                                                                                                                 |
| 67         | G4                                                                                                                  |
| 66         | F#4                                                                                                                 |
| 65         | F4                                                                                                                  |
| 64         | E4                                                                                                                  |
| 63         | Eb4                                                                                                                 |
| 62         | D4                                                                                                                  |
| 61         | C#4                                                                                                                 |
| 60         | C4                                                                                                                  |
| 59         | B3                                                                                                                  |
| 58         | Bb3                                                                                                                 |
| 57         | A3                                                                                                                  |
| 56         | G#3                                                                                                                 |
| 55         | G3                                                                                                                  |
| 54         | F#3                                                                                                                 |
| 53         | F3                                                                                                                  |
| 52         | E3                                                                                                                  |
| 51         | Eb3                                                                                                                 |
| 50         | D3                                                                                                                  |
| 49         | C#3                                                                                                                 |
| 48         | C3                                                                                                                  |
| 47         | B2                                                                                                                  |
| 46         | Bb2                                                                                                                 |
| 45         | A2                                                                                                                  |
| 44         | G#2                                                                                                                 |
| 43         | G2                                                                                                                  |
| 42         | F#2                                                                                                                 |
| 41         | F2                                                                                                                  |
| 40         | E2                                                                                                                  |
| 39         | Eb2                                                                                                                 |
| 38         | D2                                                                                                                  |
| 37         | C#2                                                                                                                 |
| 36         | C2 (Lowest)                                                                                                         |
|            |                                                                                                                     |
| Shifts     |                                                                                                                     |
| 1          | Lyric Shift<br>Sets additional shift points for static lyrics.                                                      |
| 0          | Range Shift<br>Allows the note display range to shift if a vocal range changes drastically. Length determines the speed of the shift, not the duration that it should be shifted for. |

#### Vocals Lyrics

Lyrics are stored as meta events (usually text or lyric), paired up with notes in the 36 to 84 range. Lyric phrases are marked using either note 105 or note 106, or both at the same time (same start and end).

There are various symbols used for specific things in lyrics:

| Name           | Symbol | Description                                                                             |
| :---           | :----: | :----------                                                                             |
| Hyphens        | `-`    | Joins this syllable with the next.                                                      |
| Pluses         | `+`    | Connects the previous note and the current note into a slide. Usually found standalone. |
| Equals         | `=`    | Replaced with a literal hyphen, and joins to the next syllable if placed at the end.    |
| Pounds         | `#`    | Marks a note as non-pitched.                                                            |
| Carets         | `^`    | Marks a note as non-pitched with a more lenient scoring.<br>Typically used on short syllables or syllables without sharp attacks. |
| Asterisks      | `*`    | Marks a note as non-pitched. Its difference is unknown.                                 |
| Percents       | `%`    | Marks a range division for vocals parts with large octave ranges.                       |
| Sections       | `§`    | Replaced by a tie character `‿` that indicates that two syllables are sung as a single syllable. Both syllables are in the same event, with the `§` used in place of a space between the two. |
| Dollars        | `$`    | Marks syllables in harmony parts as hidden.<br>In one case these are also on the standard Vocals track for some reason, its purpose is not known (may be a charting error). |
| Slashes        | `/`    | Unknown. These appear in some Rock Band charts, mainly The Beatles: Rock Band.          |
| Underscores    | `_`    | Replaced by a space.                                                                    |
| Angle brackets | `<>`   | Holds a text formatting tag.<br>Some games may support text formatting tags in lyrics (mainly Unity games). Other games that don't use these tags need to strip them out. |

Syllables not joined together through any symbols should be separated by a space during parsing.

Here's how these symbols should be handled for displaying as just text:

- Strip out `-`, `+`, `#`, `^`, `*`, `%`, `$`, `/`, `<>`, and anything between `<>`.
- Replace `=` with a hyphen `-`. Make sure to not strip out hyphens added as replacement for equals.
- Replace `§`, `_` with a space.
- Join together a syllable that has `-` or `=` at the end of it with the following syllable.

NOTE: Some charts may have more than one lyric event at the same position. While this shouldn't be common, it has happened in some charts and needs to be accounted for in some way.

### Rock Band 3 Pro Keys Tracks

- `PART REAL_KEYS_X` - Pro Keys Expert
- `PART REAL_KEYS_H` - Pro Keys Hard
- `PART REAL_KEYS_M` - Pro Keys Medium
- `PART REAL_KEYS_E` - Pro Keys Easy

(The Pro Keys animation note tracks, as well as other notes specific to Rock Band, are detailed in [Midi_RockBand.md](../RockBand/Midi_RockBand.md))

#### Pro Keys Notes

| MIDI Note | Description                              |
| :-------: | :----------                              |
| Markers   |                                          |
| 127       | Trill marker                             |
| 126       | Glissando marker<br>Only works on Expert |
| 116       | Overdrive marker                         |
| 115       | Solo marker                              |
|           |                                          |
| Notes     |                                          |
| 72        | C3 (Highest note)                        |
| 71        | B2                                       |
| 70        | A#2/Bb2                                  |
| 69        | A2                                       |
| 68        | G#2/Ab2                                  |
| 67        | G2                                       |
| 66        | F#2/Gb2                                  |
| 65        | F2                                       |
| 64        | E2                                       |
| 63        | D#2/Eb2                                  |
| 62        | D2                                       |
| 61        | C#2/Db2                                  |
| 60        | C2                                       |
| 59        | B1                                       |
| 58        | A#1/Bb1                                  |
| 57        | A1                                       |
| 56        | G#1/Ab1                                  |
| 55        | G1                                       |
| 54        | F#1/Gb1                                  |
| 53        | F1                                       |
| 52        | E1                                       |
| 51        | D#1/Eb1                                  |
| 50        | D1                                       |
| 49        | C#1/Db1                                  |
| 48        | C1 (Lowest note)                         |
|           |                                          |
| Ranges    |                                          |
| 9         | Range shift: A1-C3                       |
| 7         | Range shift: G1-B2                       |
| 5         | Range shift: F1-A2                       |
| 4         | Range shift: E1-G2                       |
| 2         | Range shift: D1-F2                       |
| 0         | Range shift: C1-E2                       |

Additional info:

- For RB3, up to 4 notes are allowed at a time, and extended sustains/disjoint chords are allowed.
- Trill and glissando lanes are used to make imprecise/indiscernible trills and glissandi easier to play.
  - Trill markers prevent overhitting and only require the player to alternate faster than a certain threshold to hit the charted notes.
  - Glissando markers, as the Rock Band Network docs describe it, disable the scoring system for the notes between the start and end of the glissando.
- The range shift notes will shift the range of the keys shown on the track. In RB3, 17 keys out of the 25 keys are shown at a time, so if there are notes that go beyond the current range, the range must be shifted to show those notes.
  - These notes do not last the duration of the shift, they simply mark when the shift happens.
  - One of these shifts should be marked at the very beginning as the starting range.

### Phase Shift Real Keys Tracks

- `PART REAL_KEYS_PS_X` - Real Keys Expert
- `PART REAL_KEYS_PS_M` - Real Keys Hard
- `PART REAL_KEYS_PS_H` - Real Keys Medium
- `PART REAL_KEYS_PS_E` - Real Keys Easy

#### Real Keys Notes

Notes/markers:

| MIDI Note | Description                                                          |
| :-------: | :----------                                                          |
| Markers   |                                                                      |
| 127       | Trill marker                                                         |
| 126       | Glissando marker                                                     |
| 116       | Star Power/Overdrive marker                                          |
| 115       | Solo marker                                                          |
|           |                                                                      |
| Notes     |                                                                      |
| 108       | C8 (Highest Note)                                                    |
| 107       | B7                                                                   |
| 106       | A#7                                                                  |
| 105       | A7                                                                   |
| 104       | G#7                                                                  |
| 103       | G7                                                                   |
| 102       | F#7                                                                  |
| 101       | F7                                                                   |
| 100       | E7                                                                   |
| 99        | D#7                                                                  |
| 98        | D7                                                                   |
| 97        | C#7                                                                  |
| 96        | C7                                                                   |
| 95        | B6                                                                   |
| 94        | A#6                                                                  |
| 93        | A6                                                                   |
| 92        | G#6                                                                  |
| 91        | G6                                                                   |
| 90        | F#6                                                                  |
| 89        | F6                                                                   |
| 88        | E6                                                                   |
| 87        | D#6                                                                  |
| 86        | D6                                                                   |
| 85        | C#6                                                                  |
| 84        | C6                                                                   |
| 83        | B5                                                                   |
| 82        | A#5                                                                  |
| 81        | A5                                                                   |
| 80        | G#5                                                                  |
| 79        | G5                                                                   |
| 78        | F#5                                                                  |
| 77        | F5                                                                   |
| 76        | E5                                                                   |
| 75        | D#5                                                                  |
| 74        | D5                                                                   |
| 73        | C#5                                                                  |
| 72        | C5                                                                   |
| 71        | B4                                                                   |
| 70        | A#4                                                                  |
| 69        | A4                                                                   |
| 68        | G#4                                                                  |
| 67        | G4                                                                   |
| 66        | F#4                                                                  |
| 65        | F4                                                                   |
| 64        | E4                                                                   |
| 63        | D#4                                                                  |
| 62        | D4                                                                   |
| 61        | C#4                                                                  |
| 60        | C4 (Middle C)                                                        |
| 59        | B3                                                                   |
| 58        | A#3                                                                  |
| 57        | A3                                                                   |
| 56        | G#3                                                                  |
| 55        | G3                                                                   |
| 54        | F#3                                                                  |
| 53        | F3                                                                   |
| 52        | E3                                                                   |
| 51        | D#3                                                                  |
| 50        | D3                                                                   |
| 49        | C#3                                                                  |
| 48        | C3                                                                   |
| 47        | B2                                                                   |
| 46        | A#2                                                                  |
| 45        | A2                                                                   |
| 44        | G#2                                                                  |
| 43        | G2                                                                   |
| 42        | F#2                                                                  |
| 41        | F2                                                                   |
| 40        | E2                                                                   |
| 39        | D#2                                                                  |
| 38        | D2                                                                   |
| 37        | C#2                                                                  |
| 36        | C2                                                                   |
| 35        | B1                                                                   |
| 34        | A#1                                                                  |
| 33        | A1                                                                   |
| 32        | G#1                                                                  |
| 31        | G1                                                                   |
| 30        | F#1                                                                  |
| 29        | F1                                                                   |
| 28        | E1                                                                   |
| 27        | D#1                                                                  |
| 26        | D1                                                                   |
| 25        | C#1                                                                  |
| 24        | C1                                                                   |
| 23        | B0                                                                   |
| 22        | A#0                                                                  |
| 21        | A0 (Lowest Note)                                                     |
|           |                                                                      |
| Ranges    |                                                                      |
| 13        | Right hand range shift<br>Range defined using velocity, from 21-108. |
| 12        | Left hand range shift<br>Range defined using velocity, from 21-108.  |

Channels:

| MIDI Channel | Description       |
| :----------: | :----------       |
| 0            | Right hand notes  |
| 1            | Left hand notes   |

Other info:

- Lane count for each hand is defined through the song.ini tags `real_keys_lane_count_left` and `real_keys_lane_count_right`.

### Rock Band 3 Pro Guitar/Bass Tracks

- `PART REAL_GUITAR` - Pro Guitar (17-Fret)
- `PART REAL_GUITAR_22` - Pro Guitar (22-Fret)
- `PART REAL_GUITAR_BONUS` - Unknown Pro Guitar track
- `PART REAL_BASS` - Pro Bass (17-Fret)
- `PART REAL_BASS_22` - Pro Bass (22-Fret)

Pro Guitar/Bass has some rather complicated mechanics, so some things may have been missed. There are also some observed unknowns in some chart files documented here.

#### Pro Guitar/Bass Notes and Channels

Notes/markers:

| MIDI Note  | Description                                               |
| :-------:  | :----------                                               |
| Markers    |                                                           |
| 127        | Trill marker                                              |
| 126        | Tremolo marker                                            |
| 116        | Star Power/Overdrive marker                               |
| 115        | Solo marker                                               |
| 108        | Player's hand position marker<br>Marks where the player's left hand should be from this point forward and determines how fret numbers get positioned on top of the note. Fret number determined by note velocity, from 101 to 114 for 17-fret, 119 for 22-fret. |
| 107        | Force chord numbering to display all fret numbers         |
|            |                                                           |
| Expert     |                                                           |
| 106        | Expert Unknown                                            | 
| 105        | Expert string emphasis marker<br>Uses track channels to emphasize certain strings: 13 = EAD, 14 = ADGB, 15 = GBe. |
| 104        | Expert arpeggio marker<br>Requires a corresponding note/chord on track channel 2 to use as a ghost note/chord shape in-game.
| 103        | Expert slide marker<br>Slide direction determined by various rules detailed below. |
| 102        | Expert force HOPO                                         |
| 101        | Expert Purple (e string)                                  |
| 100        | Expert Yellow (B string)                                  |
| 99         | Expert Blue (G string)                                    |
| 98         | Expert Orange (D string)                                  |
| 97         | Expert Green (A string)                                   |
| 96         | Expert Red (E string)                                     |
|            |                                                           |
| Hard       |                                                           |
| 82         | Hard Unknown                                              | 
| 81         | Hard string emphasis marker                               |
| 80         | Hard Arpeggio marker                                      |
| 79         | Hard slide marker                                         |
| 78         | Hard force HOPO                                           |
| 77         | Hard Purple (e string)                                    |
| 76         | Hard Yellow (B string)                                    |
| 75         | Hard Blue (G string)                                      |
| 74         | Hard Orange (D string)                                    |
| 73         | Hard Green (A string)                                     |
| 72         | Hard Red (E string)                                       |
|            |                                                           |
| Medium     |                                                           |
| 58         | Medium Unknown                                            | 
| 56         | Medium Arpeggio marker                                    |
| 55         | Medium slide marker                                       |
| 54         | Medium force HOPO                                         |
| 53         | Medium Purple (e string)                                  |
| 52         | Medium Yellow (B string)                                  |
| 51         | Medium Blue (G string)                                    |
| 50         | Medium Orange (D string)                                  |
| 49         | Medium Green (A string)                                   |
| 48         | Medium Red (E string)                                     |
|            |                                                           |
| Easy       |                                                           |
| 34         | (Unobserved) Easy Unknown                                 | 
| 32         | Easy Arpeggio marker                                      |
| 31         | Easy slide marker                                         |
| 30         | Easy force HOPO                                           |
| 29         | Easy Purple (e string)                                    |
| 28         | Easy Yellow (B string)                                    |
| 27         | Easy Blue (G string)                                      |
| 26         | Easy Orange (D string)                                    |
| 25         | Easy Green (A string)                                     |
| 24         | Easy Red (E string)                                       |
|            |                                                           |
| Root Notes |                                                           |
| 18         | Switch the chord name from sharps to flats, or vice versa |
| 17         | Hide the chord name                                       |
| 16         | Marks slash chords, such as `Fm7/E` or `Am/G`<br>Uses the lowest string's fret number for determining what note to use after the slash. |
| 15         | Root note marker: D#/Eb<br>Root notes apply to further notes until a new root marker is placed, at which point that root will continue until the next, and so on. |
| 14         | Root note marker: D                                       |
| 13         | Root note marker: C#/Db                                   |
| 12         | Root note marker: C                                       |
| 11         | Root note marker: B                                       |
| 10         | Root note marker: A#/Bb                                   |
| 9          | Root note marker: A                                       |
| 8          | Root note marker: G#/Ab                                   |
| 7          | Root note marker: G                                       |
| 6          | Root note marker: F#/Gb                                   |
| 5          | Root note marker: F                                       |
| 4          | Root note marker: E                                       |

Channels:

| MIDI Channel | Description                       |
| :----------: | :----------                       |
| 0            | Normal notes and markers          |
| 1            | Ghost notes/arpeggio chord shapes |
| 2            | Bend notes                        |
| 3            | Muted notes<br>In RB3, if any note in a chord is marked as muted, the whole chord will become muted. Chords that contain single muted strings must omit the muted strings in charting. |
| 4            | Tapped notes                      |
| 5            | Harmonics                         |
| 6            | Pinch harmonics                   |
| 13           | String emphasis: EAD              |
| 14           | Bass slap; string emphasis: ADGB  |
| 15           | String emphasis: GBe              |

Additional info:

- The fret number of notes and markers that use fret numbers is determined by the MIDI note velocity, starting from velocity 100 and going to velocity 117 for 17-fret or 122 for 22-fret.
- There are separate tracks for 17-fret and 22-fret due to different controller models having a different amount of available frets.
- Root notes are required for chords, but they don't have to be placed on every chord. It's only necessary to place one when the root note changes.
- RB3 slide direction rules:
  - If the sustain ends within a 1/16th of a following note, the slide will follow the direction of the next note's fret number: lower to higher will slide up, higher to lower will slide down. If both are the same, it will slide down.
  - Otherwise, if the fret number for the corresponding note is 0-7, the slide will go up, otherwise if it's 8 or higher it will go down.
  - For chords, the lowest non-open (fret number of 0) string's fret number determines the direction.
  - At least for muted notes or muted chords, the direction can be reversed by placing the slide marker on channel 12.
- For RB3, overdrive must match that of standard Guitar/Bass.

#### Phase Shift Pro Guitar/Bass SysEx Events

| Description    | SysEx data                                     |
| :----------    | :---------                                     |
| Slide up       | `50 53 00 00 <difficulty> 02 <enable/disable>` |
| Slide down     | `50 53 00 00 <difficulty> 03 <enable/disable>` |
| Palm mute      | `50 53 00 00 <difficulty> 09 <enable/disable>` |
| Vibrato        | `50 53 00 00 <difficulty> 10 <enable/disable>` |
| Harmonic       | `50 53 00 00 <difficulty> 11 <enable/disable>` |
| Pinch harmonic | `50 53 00 00 <difficulty> 12 <enable/disable>` |
| Bend           | `50 53 00 00 <difficulty> 13 <enable/disable>` |
| Accent         | `50 53 00 00 <difficulty> 14 <enable/disable>` |
| Pop            | `50 53 00 00 <difficulty> 15 <enable/disable>` |
| Slap           | `50 53 00 00 <difficulty> 16 <enable/disable>` |

### Phase Shift Dance Track

- `PART DANCE` - 4-lane Dance

#### Dance Notes

Unlike the other tracks, this track has 5 difficulties.

Notes:

| MIDI Note | Description                 |
| :-------: | :----------                 |
| 116       | Star Power/Overdrive marker |
| 103       | Solo marker                 |
|           |                             |
| 99        | Challenge right             |
| 98        | Challenge up                |
| 97        | Challenge down              |
| 96        | Challenge left              |
|           |                             |
| 87        | Hard right                  |
| 86        | Hard up                     |
| 85        | Hard down                   |
| 84        | Hard left                   |
|           |                             |
| 75        | Medium right                |
| 74        | Medium up                   |
| 73        | Medium down                 |
| 72        | Medium left                 |
|           |                             |
| 63        | Easy right                  |
| 62        | Easy up                     |
| 61        | Easy down                   |
| 60        | Easy left                   |
|           |                             |
| 51        | Beginner right              |
| 50        | Beginner up                 |
| 49        | Beginner down               |
| 48        | Beginner left               |

Channels:

| MIDI Channel | Description |
| :----------: | :---------- |
| 0            | Normal note |
| 1            | Mine        |
| 2            | Lift        |
| 3            | Roll        |

Additional notes:

- Phase Shift's specifications specify that notes on this track should be placed with a velocity of 127. No other note velocities are defined, so this was probably for future-proofing in case velocities were used for extensions to the format.

### Events Track

The Events track contains text events that do various things, along with a few notes for practice mode drum samples (detailed in the RB .mid doc since it's not necessary for the core format).

Known text events specific to Rock Band or Guitar Hero 1/2 are listed in their respective .mid documents.

#### Events Common Text Events

| Event Text                        | Description                                                         |
| :---------                        | :----------                                                         |
| `[prc_<name>]`/`[section <name>]` | Marks a practice mode section. `<name>` is the name of the section. |
| `[end]`                           | Marks the end point of the chart.<br>In some charts this may be present before the actual end, only trust this event if it comes after all notes. |

Other text events specific to other games may be seen in this track. Those are documented in their respective game's MIDI documentation in this repo.

### Rock Band Tracks

These are tracks specific to Rock Band:

- `PART KEYS_ANIM_LH` - RB3 Pro Keys left-hand animations
- `PART KEYS_ANIM_RH` - RB3 Pro Keys right-hand animations
- `VENUE` - RB venue track
- `BEAT` - RB upbeat/downbeat track

These tracks are detailed in [Midi_RockBand.md](../GuitarHero1-2/Midi_RockBand.md), as they are specific to GH1/2 and don't particularly matter for the core format.

### GH1 and 2 Tracks

These are tracks specific to Guitar Hero 1/2:

- `TRIGGERS` - Lighting, venue, and practice mode assist track
- `ANIM` - GH1 guitarist left hand position track
- `BAND_DRUM` - Drummer animations track
- `BAND_BASS` - Bassist animations track
- `BAND_SINGER` - Vocalist animations track
- `GUITAR` - Guitar animations track
- `BASS` - Bass animations track (identical to `GUITAR` in content)

These tracks are detailed in [Midi_GuitarHero1-2.md](../GuitarHero1-2/Midi_GuitarHero1-2.md), as they are specific to GH1/2 and don't particularly matter for the core format.

## References

Specifications for the MIDI protocol/format itself are available from the MIDI Association here:

- [MIDI 1.0 Protocol Specifications](https://www.midi.org/specifications/midi1-specifications)
- [Summary of MIDI 1.0 Messages](https://www.midi.org/specifications-old/item/table-1-summary-of-midi-message)
- [Standard MIDI File Specifications](https://www.midi.org/specifications/file-format-specifications/standard-midi-files)

Chart format info comes from these sources:

- [C3/Rock Band Network docs](http://docs.c3universe.com/rbndocs/index.php?title=Authoring).
- [Rhythm Gaming World forums: "General Thread for PRO Guitar/Bass Questions"](https://rhythmgamingworld.com/forums/topic/general-thread-for-pro-guitarbass-questions/).
  - Since old C3 links redirect to RGW incorrectly: [Rhythm Gaming World forums: "Track Requirements: Pro Guitar/Bass"](https://rhythmgamingworld.com/forums/topic/track-requirements-pro-guitarbass/)
- [Phase Shift forums: "R.E.A.P.E.R. Plugin [5/8/2014] V7"](https://dwsk.proboards.com/thread/1652/plugin-5-8-2014-v7)
- [Phase Shift forums: "Song Standard Advancements"](https://dwsk.proboards.com/thread/404/song-standard-advancements)
- [ScoreHero forums: "Lighting/Band/Crowd Events & Notes (New lighting ideas.)"](https://www.scorehero.com/forum/viewtopic.php?t=20179).
- [ScoreHero forums: "Guitar Hero MIDI and VGS File Details"](https://www.scorehero.com/forum/viewtopic.php?t=1179)
- [ScoreHero forums: "GH2 Midi Encyclopedia / Dictionary"](https://www.scorehero.com/forum/viewtopic.php?t=5523)
- [FoFiX source code](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py)
- [Editor on Fire source code](https://github.com/raynebc/editor-on-fire/blob/3c385f0d668b3dfdc7c648377cf422b4e2671db5/src/midi.c)
