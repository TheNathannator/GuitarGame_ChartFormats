# MIDI

The `.mid`/`.midi` format is a binary stream of MIDI events. The extension used for charts is `.mid`; `.midi` is not typically used and is not supported by most programs that use it for charts. For an overview on the MIDI file format itself, refer to the [MIDI General Info](MIDI%20General%20Info.md) document.

Harmonix designed a specific layout for the charts in Guitar Hero 1/2 and Rock Band using .mid files, and early fan-games were quick to analyze and adopt the format. Its use was further helped by the introduction of Rock Band Network, a service for Rock Band that allowed people to license and chart songs to be made available as DLC. This format remains prominent today, and still receives new features on occasion as the need arises.

Modern .mid charts follow a format originating from Rock Band's .mid chart format, with additional functionality added by the community in games such as Phase Shift and Clone Hero. Older .mid charts are slightly different and are based on Guitar Hero 1/2's format, though the differences are small and easily accounted for.

## Table of Contents

- [Glossary](#glossary)
- [File Structure Details](#file-structure-details)
  - [MIDI Format](#midi-format)
  - [Song Metadata](#song-metadata)
  - [Audio Files](#audio-files)
- [Tracks](#tracks)
  - [Track Events](#track-events)
    - [Track Notes](#track-notes)
    - [Track Phrases](#track-phrases)
    - [Track Modifiers](#track-modifiers)
  - [Track Difficulties](#track-difficulties)
  - [Other Track Data](#other-track-data)
  - [Text Events](#text-events)
    - [Global and Local Events](#global-and-local-events)
  - [SysEx Events](#sysex-events)
    - [Phase Shift SysEx Event Specification](#phase-shift-sysex-event-specification)
  - [Notable Specification Violations](#notable-specification-violations)
- [References](#references)

## Glossary

A quick overview of general nomenclature used in these docs. Terms with significant importance to the structure of the chart are also given dedicated sections which elaborate more on this information.

- "MIDI note": A Note On/Off event pair in a MIDI track.
- ["Note"](#track-notes): A MIDI event that specifies a note in an instrument track.
  - Without the qualification of "MIDI note", it does *not* refer to a MIDI note.
- ["Phrase"](#track-phrases): An in-game mechanic that lasts for a duration.
- ["Modifier"](#track-modifiers): An in-game mechanic that may modify one or more other events.
- "Marker": A less specific term that refers to any event that specifies ("marks") a [phrase](#track-phrases) or [modifier](#track-modifiers).
- ["Text event"](#text-events): Any of the meta events in the type range `0x01` through `0x0F`, unless otherwise specified.
- ["Global event"](#global-and-local-events): A text event placed on the `EVENTS` track, which either affects all instruments or does not affect any particular instrument.
- ["Local event"](#global-and-local-events): A text event which is placed on an instrument track.

The use of `<angle brackets>` in an event definition indicates something that is variable. The brackets are not part of the end result (unless otherwise specified), they are simply used as a delimiter.

## File Structure Details

The files that comprise a .mid chart are grouped into a folder, and contain the following at minimum:

- `notes.mid`: Tempo map, tracks, notes, and other chart data.
- [`song.ini`](../song.ini/Core%20Infrastructure.md): Song metadata
- At least one [audio file](../Audio%20Files.md).

### MIDI Format

The `notes.mid` file is written using MIDI format type 1 (multi-track) and ticks-per-quarter resolution. Type 0/2 and SMPTE resolution are not supported.

### Song Metadata

No song metadata is stored within the .mid file itself. Instead, an accompanying [`song.ini` file](../song.ini/Core%20Infrastructure.md) is used. This file typically stores things like song title and artist name, but a few properties also affect how the `notes.mid` file is processed, which are noted where relevant.

### Audio Files

.mid doesn't have any way of naming specific audio files to be loaded. Instead, it relies on a [specific list of audio file names](../Audio%20Files.md).

## Tracks

Tracks are identified by their track name meta event, which is the first event of each track.

Instrument track names usually follow a basic nomenclature of `PART <instrument name in all caps>`. For example, guitar has a track name of `PART GUITAR`, and drums has a track name of `PART GUITAR`. Not all tracks follow this, however.

Miscellaneous/global tracks typically use an un-prefixed name which describes their purpose (for example, `EVENTS` and `VENUE`).

Since any name can be used for a track, unknown tracks should be ignored.

Details for specific tracks may be found within the [`Standard`](Standard/) folder next to this document. Note that this folder primarily only covers details that are commonly supported within the community; for details on the original Harmonix-created formats, refer to the [`Guitar Hero 1 and 2`](Miscellaneous/Guitar%20Hero%201%20and%202/) and [`Rock Band`](Miscellaneous/Rock%20Band/) folders within the [`Miscellaneous`](Miscellaneous/) folder.

### Track Events

Tracks can make use of any MIDI events as data for the track. Most tracks use note events and text events, with some using SysEx events as well, but other tracks are more complex and make use of channel numbers and velocity as well.

There are no fully standardized ways that a track is laid out. Every track may have its own definitions for its notes, modifiers, etc. depending on its needs.

In addition to this, some tracks may have notes not specified in these docs, either due to authoring error or due to incomplete documentation. Unknown events should be ignored.

#### Track Notes

An event which defines a note on an instrument is referred to as a note.

While this description should be obvious, it is provided to ensure clarity in the case of confusion: "Note" refers to any event which defines the placement of objects that travel down the instrument track and are required to be hit within a certain timing window.

This terminology may be confused with MIDI notes, so an additional distinction is made here: "note" refers to a note on the instrument, while "MIDI note" refers to a MIDI note within the instrument's MIDI track.

#### Track Phrases

An event which defines some other in-game mechanic that has a length is referred to as a phrase.

Phrases often apply a secondary effect or mechanic to notes, or define another timed mechanic beyond the scope of notes. Typically, they do not affect the tick they end on, but there are important exceptions which are noted where relevant.

#### Track Modifiers

An event which modifies some other event at the same position (or within its length) is referred to as a modifier.

Most modifiers affect all relevant events which fall under their own length. Like phrases, they don't typically affect the tick they end on, unless otherwise specified.

### Track Difficulties

Usually, all difficulties of an instrument are contained within a single track. Difficulties typically span a single octave, with the lowest note of the octave being the first lane of the difficulty.

Some instruments don't adhere to these ranges though, and may use more ranges, different ranges, or even dedicated tracks for each difficulty. Some may also specify markers which apply across all difficulties within the range of a specific difficulty, but these never conflict with notes.

The common difficulty note ranges are as follows:

- Expert: 96-107
- Hard: 84-95
- Medium: 72-83
- Easy: 60-71

### Other Track Data

Tracks may contain other data in the notes outside of the difficulty ranges. These can be anything from animation data to modifiers that apply to all difficulties.

Typically, the top two octaves (notes 108-120), plus the few notes above it (notes 108-127), are reserved for this miscellaneous track data. Various notes below the difficulty ranges are often used as well.

### Text Events

"Text event" refers to any meta event on a track which contains text (i.e. meta event types `0x01` through `0x0F`). They are used to mark things that would be impractical to use notes for, such as venue parameters or character animation triggers.

Typically, the text of these events are surrounded by \[square brackets\]. Some events might not have brackets, but it is highly recommended that all text events use these brackets. For simplicity, only the bracketed versions are listed in these docs.

#### Global and Local Events

Global events are text events placed on an `EVENTS` track. These events affect all tracks or define things that don't pertain to any specific instrument, such as practice mode sections.

Local events are text events placed on an instrument track. These events affect only the track they're placed on.

### SysEx Events

Tracks which use SysEx events typically use a format that Phase Shift introduced for its own extensions to the format. It is not likely to be extended in the future, as it's clunky to author manually, has a history of problematic support in certain games/programs, and is overall disliked by both developers and authors alike.

#### Phase Shift SysEx Event Specification

Phase Shift SysEx events have a payload of 7 bytes (excluding the SysEx start/end bytes), which are defined as follows:

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
  
  - A small quirk: unlike most other phrases, guitar tap note phrases (`0x04`) *do* affect the tick they end on. A tap phrase ending on tick 480 will make any notes also on tick 480 become taps. This behavior does not apply to open note markers; the other phrases have not been checked yet.

- `Value` is the value for this event. As modifier events are the only event type defined, these are the only available values:

  | Value  | Description  |
  | :---:  | :----------  |
  | `0x00` | Phrase end   |
  | `0x01` | Phrase start |

### Notable Specification Violations

Some charts don't reset running status after a System Exclusive or meta event and continue the running status of prior non-Exclusive/meta events, which breaks some MIDI parsers (notably NAudio).<!-- Thanks, Phase Shift. -->

Charts commonly make use of a `0xFF` byte in SysEx events. This is *barely* technically spec-compliant, but some MIDI parsers might have issues with this regardless.

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
