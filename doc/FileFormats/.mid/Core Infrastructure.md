# MIDI

The `.mid`/`.midi` format is a binary stream of MIDI events. The extension used for charts is `.mid`; `.midi` is not typically used and is not supported by most programs that use it for charts. For an overview on the MIDI file format itself, refer to the [MIDI General Info](MIDI%20General%20Info.md) document.

Harmonix designed a specific layout for the charts in Guitar Hero 1/2 and Rock Band using .mid files, and early fan-games were quick to analyze and adopt the format. Its use was further helped by the introduction of Rock Band Network, a service for Rock Band that allowed people to license and chart songs to be made available as DLC. This format remains prominent today, and still receives new features on occasion as the need arises.

Modern .mid charts follow a format originating from Rock Band's .mid chart format, with additional functionality added by the community in games such as Phase Shift and Clone Hero. Older .mid charts are slightly different and are based on Guitar Hero 1/2's format, though the differences are small and easily accounted for.

## Table of Contents

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
  - [Notable Specification Violations](#notable-specification-violations)
- [References](#references)

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
