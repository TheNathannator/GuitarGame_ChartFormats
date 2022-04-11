# .chart

.chart is a text-based custom chart format originating from the GH1/2 era. It was originally created as an intermediate format, meant to be converted for use in a game, but nowadays it is typically used directly.

This document lays out the core infrastructure of the .chart format. To keep these docs easily extensible, specifications for different game/instrument types will go in separate documents, and only the base infrastruture will be detailed here.

## Table of Contents

- [Brief History](#brief-history)
- [Basic Infrastructure](#basic-infrastructure)
  - [Sections](#sections)
    - [Section Data](#section-data)
  - [Section Names](#section-names)
- [Song Section Details](#song-section-details)
  - [Basic Metadata](#basic-metadata)
    - [Audio Streams](#audio-streams)
  - [Song Section Example](#song-section-example)
- [Track Events](#track-events)
- [SyncTrack Section Details](#synctrack-section-details)
  - [Time Signatures](#time-signatures)
  - [Tempos](#tempos)
  - [Tempo Anchors](#tempo-anchors)
  - [SyncTrack Section Example](#synctrack-section-example)
- [Events Section Details](#events-section-details)
  - [Global Events](#global-events)
    - [Basic Global Events](#basic-global-events)
  - [Events Section Example](#events-section-example)
- [Instrument Section Details](#instrument-section-details)
  - [Notes and Modifiers](#notes-and-modifiers)
  - [Special Phrases](#special-phrases)
    - [Note, Modifier, and Special Phrase Type Divisions](#note-modifier-and-special-phrase-type-divisions)
  - [Local Events](#local-events)

## Brief History

Paraphrased and extended from [FireFox's .chart specifications](https://docs.google.com/document/d/1v2v0U-9HQ5qHeccpExDOLJ5CMPZZ3QytPmAG5WF0Kzs/edit?usp=sharing):

In the early days of Guitar Hero, a ScoreHero user by the name of Turkeyman collaborated with several members of the ScoreHero community to reverse-engineer the original Guitar Hero 1 archive format.
Soon after, a user by the name Katamakel developed a user interface to rebuild these archives, while Turkeyman worked on an editor to modify the data contained within them. This later turned into Feedback Editor.

Support for Feedback Editor dropped around the release of Guitar Hero 3 due to Turkeyman aiming to develop the program into a fully standalone game.
Remnants of this can be seen with the inclusion of the Drums and Keys track in Feedback Editor, which were implemented before Guitar Hero: World Tour or Rock Band were announced.
There was also a secondary version of the .chart file format in development for this game known as .chart-v2.
This format would have addressed the current limitations that came with designing the original .chart format for GH1/2, but it was never adopted by the community due to it only being available in Turkeyman’s game, which never released.

Modding for Guitar Hero 3 also hit a standstill at this time due to increased security within the game’s code.
It wasn’t until the PC release of GH3 that the modding scene picked up again, spawning Guitar Hero Three Control Panel, which became the main modding tool for the game.
Soon after Guitar Hero: World Tour and Rock Band were announced, development for Feedback dropped entirely.

The .chart format was later updated to support GH3 mods by ExileLord, such as tap notes and open notes.
Due to Feedback’s outdated nature, these mechanics had to be implemented using workarounds that were extremely tedious and prone to errors and crashes.
It was also around this time that Moonscraper Chart Editor began development by FireFox in order to re-support the .chart format and the new mods with it, as well as make the charting process easier and more accessible.

Over time, people found exhaustion in the entire modding process for GH3, and soon a fan-game called Clone Hero began development by a user named srylain.
This game read the entire .chart file format directly into the game, which made playing custom Guitar Hero songs much more accessible, as it avoided any complicated game modification processes.
Clone Hero would go on to implement its own mechanics into the .chart file format, such as the format for lyrics by mdsitton, Guitar Hero Live track data, and drums cymbal marking/SP activation phrases.

Even today, the format continues to evolve.
There are still features to add to reach full drums parity with the .mid chart format, and a new proposal for DJ Hero tracks within .chart files has been made from the DJ Hero community.
Despite all this though, others have gripes with the format, and some want to create a new format free of the burdens of past mistakes, both of the format itself, and of specifications made for some of the tracks.
The ultimate fate of the .chart format remains to be seen.

## Basic Infrastructure

### Sections

.chart files are segmented into sections. Sections start with their name in square brackets, and follow with their contents encapsulated by curly brackets. Section names are written in PascalCase. Sections do not have to be separated with line breaks, and can show up in any order, at least for instrument tracks.

```
[SectionOne]
{
  // Section data goes here
}
[SectionTwo]
{
  // Section data goes here
}
```

(Note that comment marking is not explicitly supported by the format. Any cases where it works are purely coincidental.)

#### Section Data

Section data is comprised of key-value pairs, each one separated by a line break and typically indented by a couple spaces or a tab, stored in this general format:

`<Key> = <Value[]>`

```
[SectionOne]
{
  Key1 = Value1
  Key2 = Value2 Value3
}
```

The value type of both the key and the value, as well as the number of values, depends on the section. Whether or not a key must be unique also depends on the section.

Value type info:

- `number` fields are whole numbers written in base 10.
- `float` fields allow decimal places and are also written in base 10.
- `string` fields typically, but don't always, use quotes. Strings that don't use quotes will be marked as such.
- `boolean` fields are either `true` or `false`.
- `file path` fields use quotes and can be either relative to the chart file, or absolute to the file system.

### Section Names

Typically, the first few sections are:

- `[Song]` – Song/chart metadata
- `[SyncTrack]` – Time signatures and tempo markers
- `[Events]` – Global events and practice mode sections

Following these are the instrument tracks. Each difficulty of every instrument is its own section. The nomenclature of these sections' names is the name of the difficulty followed by a code name for the instrument:

`[<Difficulty><Instrument>]`

For example, Hard on Drums is written as `[HardDrums]`.

Available instrument section names can be found in each game type's respective document. Any unrecognized/unknown sections should be ignored.

Any other unrecognized/unknown sections should be ignored.

All sections aside from `Song` (for chart resolution) and `SyncTrack` are optional, and instrument sections may show up in any order. A missing section is equivalent to a section with no data.

## Song Section Details

The `[Song]` section contains metadata about the song and chart.

Its data follows this format:

`<Name> = <Value>`

### Basic Metadata

| Key            | Description                                                                                      | Data type |
| :---           | :----------                                                                                      | :-------- |
| `Name`         | (Required) Title of the song.                                                                    | string    |
| `Artist`       | (Required) Artist(s) or band(s) behind the song.                                                 | string    |
| `Charter`      | Community member who charted the song.                                                           | string    |
| `Album`        | Title of the album the song is featured in.                                                      | string    |
| `Genre`        | Genre of the song.                                                                               | string    |
| `Year`         | Year of the song’s release.<br>Typically preceded by a comma and space, for example `, 2002`, to make importing into GHTCP quicker. | string |
| `Offset`       | Start time of the audio in seconds.<br>A higher value makes the audio start sooner.              | float (seconds.milliseconds) |
| `Resolution`   | (Required) Number of positional ticks between the start of one beat and the start of the next.   | number    |
| `Difficulty`   | Estimated difficulty of the song.                                                                | number    |
| `PreviewStart` | Time of the song in seconds where the song preview should start.                                 | float (seconds.milliseconds) |
| `PreviewEnd`   | Time of the song in seconds where the song preview should end.                                   | float (seconds.milliseconds) |
| `MusicStream`  | The main audio stream.<br>When other audio stems are present, this is background audio not in the other tracks and/or instruments not charted. | file path  |

### Song Section Example

```
[Song]
{
  Name = "Example .chart"
  Artist = "Example Artist"
  Charter = "Example Charter"
  Album = "Example Album"
  Year = ", 2021"
  Offset = 0
  Resolution = 192
  Player2 = bass
  Difficulty = 4
  PreviewStart = 0
  PreviewEnd = 0
  Genre = "Example Genre"
  MediaType = "Example Media Type"
  MusicStream = "song.ogg"
}
```

---

## Track Events

Track events contain data for the SyncTrack, Events, and instrument sections, and follow this format:

`<Position> = <Type Code> <Value[]>`

- `Position` is a number indicating which tick this event is located at.
- `Type Code` is a string (typically one or two characters, no quotation marks) which indicates the type of event.
- `Value[]` is a set of one or more individual values. The amount of values and type of each value varies per event type.

Track events in the same section should be written in increasing order of tick position. Some charts have some events out of order, these charts may cause issues in or be rejected by some games/programs.

## SyncTrack Section Details

The `[SyncTrack]` section contains tempo and time signature data for the chart.

### Time Signatures

Time signature markers use the `TS` type code, and are written like this:

`<Position> = TS <Numerator> <Denominator exponent>`

- `Numerator` is the numerator to use for the time signature.
- `Denominator exponent` is optional, and specifies a power of 2 to use for the denominator of the time signature.
  - Defaults to 2 (`x/4`) if unspecified.
  - This value is limited to a minimum of 0, for a resulting denominator of `x/1`.

Examples:

- 4/4 is `TS 4` or `TS 4 2`.
- 7/16 is `TS 7 4`.
- 3/8 is `TS 3 3`.

### Tempos

Tempo markers use the `B` type code, and are written like this:

`<Position> = B <Tempo>`

- `Tempo` is a whole number representation of the desired tempo. The first 3 digits starting from the right are the decimals, giving tempos a maximum of 3 decimal places.

Examples:

- 120 BPM = `B 120000`
- 60 BPM = `B 60000`
- 150.325 BPM = `B 150325`

### Tempo Anchors

Tempo anchors lock a tempo marker's tick position to a time position relative to the audio. They use the `A` type code, and are written like this:

`<Position> = A <Audio time position>`

- `Audio time position` is the time relative to the audio that the associated tempo marker should be set to, in microseconds.

This event is typically only used for chart editing. A chart editor should adjust the tempo of the preceding tempo marker to maintain this lock if any of the preceding tempo markers are adjusted.

Example:

This will anchor a 120 BPM tempo marker at tick position 864 to the time position at 2.25 seconds.

```
864 = A 2250000
864 = B 120000
```

### SyncTrack Section Example

```
[SyncTrack]
{
  0 = TS 3
  0 = B 120000
  384 = A 1000000
  384 = B 150000
  576 = TS 2 3
  768 = TS 4
  768 = B 120000
}
```

## Events Section Details

The `[Events]` section contains global events.

### Global Events

Global events are events that apply to the chart as a whole. They use the `E` type code, and are written like this:

`<Position> = E "<Text>"`

- `Text` is a quote-delimited string containing event data.

Some notes:

- Some events may have \[square brackets\] surrounding them (such as `[end]`), and some may be found in either square bracketed or plain versions. It would be a good idea to check for square brackets for all events.
- As a result of the event data being surrounded by quotation marks, global events should not have quotation marks in their data. However, charters found workarounds to this (namely, `/"` or `"/"` for the start of the string, and `"/` for the end) that necessitate the ability to parse events that contain quotation marks in the data. These were likely *not* intentional, they just so happened to work in Clone Hero. (This workaround is no longer necessary, the CH public test build can handle & display quotation marks as-is.)
- Local events also use the `E` type code, but local events are found in instrument sections, whereas global events are only in the `Events` section.

#### Basic Global Events

| Name               | Description                                                                                                     |
| :---               | :----------                                                                                                     |
| `section <name>`   | Marks the start point of a section, used by Practice mode and post-game summary.                                |
| `prc_<name>`       | Same as the above. Introduced in Rock Band 3; added to .chart for compatibility with .mid charts that use this. |
| `end`              | Marks the end of a song.                                                                                        |

### Events Section Example

```
[Events]
{
  768 = E "section Start"
  3840 = E "prc_End"
  9216 = E "end"
}
```

## Instrument Section Details

This section details the base events found in instrument sections.

### Notes and Modifiers

Notes and modifiers use the `N` type code, and are written like this:

`<Position> = N <Type> <Length>`

- `Type` is the type number of this note/modifier.
- `Length` is the length of this note in ticks. This value typically doesn't do anything for modifiers.

Modifiers are applied to existing notes but are listed as separate events. A modifier applies to all notes on the same tick, so different notes on the same tick cannot have different modifiers unless the modifier specifically targets a single color/type.

### Special Phrases

Special phrase events mark phrase or phrase-like events. They use the `S` type code and are written like this:

`<Position> = S <Type> <Length>`

- `Type` is the type number of this special phrase.
- `Length` is the length of this phrase in ticks.

A special phrase usually does not apply to the tick immediately after the length, i.e. the value of `(Position + Length)`. Any exceptions will be noted. They should also apply to their starting tick regardless of length.

### Note, Modifier, and Special Phrase Type Divisions

To help prevent issues with future additions to existing specifications creating nonsensical ordering for note/modifier/special types, it has been decided that going forward types should be divided into sections of 32 to help organize things. These divisions will be specified in each track type's document.

### Local Events

Local events are events that appear in instrument tracks. They use the `E` type code, and are written like this:

`<Position> = E <Text>`

`Text` is a string *without* quotes that contains the event data. Spaces are disallowed in these events by some programs, but there are likely charts with spaces in local events out there somewhere.
