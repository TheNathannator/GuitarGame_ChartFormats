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

This section details how .chart files are structured.

Note that the usage of C-style comments in examples throughout this document is not reflective of the actual format, comments are not officially supported.

### Sections

.chart files are segmented into sections. Sections start with their name in square brackets, which is usually written in PascalCase. The contents of sections are encapsulated by curly brackets.

Sections do not have to be separated with line breaks, and can show up in any order, at least for instrument tracks.

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

#### Section Data

Section data is comprised of key-value pairs, each one separated by a line break and typically indented by a couple spaces or a tab, stored in this general format:

`<Key> = <Value[]>`

- `Key` is the identifier to use to access this value. It may or may not need to be unique, depending on the specific type of data that the key-value pair holds.
- `Value[]` is one or more space-separated values that hold the data for the key. Quotation marks are used for string values, which allow for spaces.

```
[SectionOne]
{
  Key1 = Value1
  Key2 = Value2 Value3
}
```

Each section has different data, and a data entry in one section may mean something different in another, but there are common structures shared between sections.

#### Value Types

Value types used throughout these .chart documents are as follows:

| Value type    | Description                                                                                                         |
| :---------    | :----------                                                                                                         |
| `number`      | An integer number written in base 10.                                                                               |
| `decimal`     | A decimal-allowing number written in base 10.                                                                       |
| `string`      | Any amount of text surrounded by quotes.                                                                            |
| `bare string` | Any amount of text *not* surrounded by quotes.                                                                      |
| `boolean`     | Either `true` or `false`.                                                                                           |
| `file path`   | A file path surrounded by quotes.<br>Can be either relative to the chart file, or absolute to the OS's file system. |

#### Named Entries

Named entries contain data that should be referred to by name.

`<Name> = <Value>`

- `Name` is the name of the entry, typically written in PascalCase (though their casing may vary).
- `Value` is the value of the entry. The value type depends on which entry it is paired to.

Named entries should be unique relative to the section they are in, and their order/position within a section may vary.

#### Track Events

Track events contain data for a note, phrase, or other event that happens at a certain tick within the chart.

`<Position> = <Type Code> <Value[]>`

- `Position` is a number indicating which tick this event is located at.
- `Type Code` is a bare string (typically one or two characters) which marks the type of event.
- `Value[]` is a set of one or more individual values. The amount of values and type of each value varies per event type.

More than one event may occur at the same tick. Some events may modify other events at the same tick. Some events are phrases that modify multiple events throughout a provided length value, usually not including the very last tick (i.e. the tick at the position of `(start position + length value)`).

Events within the same section should be written in increasing order of tick position. Out-of-order events may cause issues in or be rejected by some games/programs.

Track events in the same section should be written in increasing order of tick position. Some charts have some events out of order, these charts may cause issues in or be rejected by some games/programs.

### Section Names

Typically, the first few sections are:

- `[Song]` – Song/chart metadata
- `[SyncTrack]` – Time signatures and tempo markers
- `[Events]` – Global events and practice mode sections

Following these are the instrument tracks. Typically, each difficulty of every instrument is its own section, with the standard nomenclature of these sections' names being the name of the difficulty followed by a code name for the instrument:

`[<Difficulty><Instrument>]`

For example, Hard on Drums is written as `[HardDrums]`.

Following this nomenclature for new specifications is recommended.

Available instrument section names can be found in each game type's respective document. Any unrecognized/unknown sections should be ignored.

Any other unrecognized/unknown sections should be ignored.

All sections aside from `Song` (for chart resolution, and name/artist if not using an accompanying song.ini) and `SyncTrack` are optional. Instrument sections may show up in any order. A missing section is equivalent to a section with no data.

## Song Section Details

The `[Song]` section contains metadata about the song and chart. It uses named entries to store the metadata.

The order of metadata tags may vary, and they must be unique.

This list only covers basic metadata. Other metadata is covered in other documents. Unknown/unexpected tags should be ignored.

### Basic Metadata

| Entry Name     | Description                                                                          | Value type |
| :---------     | :----------                                                                          | :--------- |
| `Name`         | (Required) Title of the song.                                                        | string     |
| `Artist`       | (Required) Artist(s) or band(s) behind the song.                                     | string     |
| `Album`        | Title of the album the song is featured in.                                          | string     |
| `Genre`        | Genre of the song.                                                                   | string     |
| `Year`         | Year of the song’s release.<br>Typically preceded by a comma and space, for example `, 2002`, to make importing into GHTCP quicker. | string |
| `Charter`      | Community member who charted the song.                                               | string     |
| `Resolution`   | (Required) Number of positional ticks between each 1/4th note in the chart.          | number     |
| `Difficulty`   | Estimated difficulty of the song.                                                    | number     |
| `Offset`       | Start time of the audio, in seconds.<br>A higher value makes the audio start sooner. | decimal    |
| `PreviewStart` | Time of the song, in seconds, where the song preview should start.                   | decimal    |
| `PreviewEnd`   | Time of the song, in seconds, where the song preview should end.                     | decimal    |
| `MusicStream`  | The main audio stream.<br>When other audio stems are present, this is background audio not in the other tracks and/or instruments not charted. | file path |

### song.ini

Most modern .chart files use an accompanying song.ini file, carried over from the .mid format, instead of the `[Song]` section. The metadata in the song.ini should be prioritized over the metadata in the .chart in cases such as these, as this is what's typically used in modern .chart files. See [Song_ini.md](Song_ini.md) for details on the song.ini file.

### Song Section Example

```
[Song]
{
  Name = "Example Song"
  Artist = "Example Artist"
  Album = "Example Album"
  Genre = "Example Genre"
  Year = ", 2021"
  Charter = "Example Charter"
  Resolution = 192
  Difficulty = 4
  Offset = 0.56
  PreviewStart = 45.28
  PreviewEnd = 75.28
  MusicStream = "Example Song.ogg"
}
```

## SyncTrack Section Details

The `[SyncTrack]` section contains tempo and time signature data for the chart.

### Time Signatures

Time signature markers use the `TS` type code, and are written like this:

`<Position> = TS <Numerator> <Denominator exponent>`

- `Numerator` is the numerator to use for the time signature.
- `Denominator exponent` is optional, and specifies a power of 2 to use for the denominator of the time signature.
  - Defaults to 2 (`x/4`) if unspecified.
  - This value is limited to a minimum of 0, for a resulting denominator of `x/1`.

A time signature marker must exist at tick 0 in the chart to set the initial time signature.

Examples:

```
0 = TS 4       // 4/4
0 = TS 4 2     // Also 4/4
768 = TS 7 4   // 7/16
1104 = TS 3 3  // 3/8
```

### Tempos

Tempo markers use the `B` type code, and are written like this:

`<Position> = B <Tempo>`

- `Tempo` is a whole number representation of the desired tempo. The first 3 digits starting from the right are the decimals, giving tempos a maximum of 3 decimal places.

A tempo marker must exist at tick 0 in the chart to set the initial tempo.

Examples:

```
0 = B 120000     // 120 BPM
768 = B 60000    // 60 BPM
1104 = B 150325  // 150.325 BPM
```

### Tempo Anchors

Tempo anchors lock an accompanying tempo marker to a time position relative to the audio. They use the `A` type code, and are written like this:

`<Position> = A <Audio time position>`

- `Audio time position` is the time relative to the audio that the associated tempo marker should be set to, in microseconds.

This event is typically only used for chart editing, and should be ignored otherwise. A chart editor should adjust the tempo of the preceding tempo marker to maintain this lock if any of the preceding tempo markers are adjusted.

Example:

```
768 = A 2250000 // 2.25 seconds
768 = B 60000  // 60 BPM
```

### SyncTrack Section Example

```
[SyncTrack]
{
  0 = TS 4         // 4/4
  0 = B 120000     // 120 BPM
  768 = TS 7 4     // 7/16
  768 = A 2250000  // 2.25 seconds
  768 = B 60000    // 60 BPM
  1104 = TS 3 3    // 3/8
  1104 = B 150325  // 150.325 BPM
}
```

## Events Section Details

The `[Events]` section contains global events.

### Global Events

Global events are events that apply to the chart as a whole. They use the `E` type code, and are written like this:

`<Position> = E "<Text>"`

- `Text` is a string containing event data.

Some events may have \[square brackets\] surrounding them (such as `[end]`), and some may be found in either square bracketed or plain versions.

Quotation marks are not allowed in global events, as these are used to open and close strings, and there are no escape characters available to escape them. However, there have been workarounds in the past to get quotation marks in global events to work in certain games for things like lyrics. This necessitates the ability to parse quotation marks inside of global events properly, or at least be aware of them and not break when they happen.

Local events also use the `E` type code, but local events are only in instrument sections, whereas global events are only in the `Events` section.

#### Basic Global Events

| Name               | Description                                                                                                     |
| :---               | :----------                                                                                                     |
| `section <name>`   | Marks the start point of a section, used by Practice mode and post-game summary.                                |
| `prc_<name>`       | Same as the above. Introduced in Rock Band 3; will be present in mid-to-chart conversions of these charts.      |
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

Modifiers are applied to existing notes, but listed as separate events. A modifier applies to all notes on the same tick, so different notes on the same tick cannot have different modifiers unless the modifier specifically targets a single color/type.

### Special Phrases

Special phrase events mark phrase or phrase-like events. They use the `S` type code and are written like this:

`<Position> = S <Type> <Length>`

- `Type` is the type number of this special phrase.
- `Length` is the length of this phrase in ticks.

More than one event may occur at the same tick. Some events may modify other events at the same tick. Some events are phrases that modify multiple events throughout their length, usually not including the very last tick (i.e. the tick at the position of `(start position + event length)`).

As specified in the [Track Events](#track-events) section,

A special phrase usually does not apply to the tick immediately after the length, i.e. the value of `(Position + Length)`. Any exceptions will be noted. They should also apply to their starting tick regardless of length.

### Note, Modifier, and Special Phrase Type Divisions

To help prevent issues with future additions to existing specifications creating nonsensical ordering for note/modifier/special types, it has been decided that going forward types should be divided into sections of 32 to help organize things. These divisions will be specified in each track type's document.

### Local Events

Local events are events that appear in instrument tracks. They use the `E` type code, and are written like this:

`<Position> = E <Text>`

- `Text` is a *bare* string that contains the event data.

Much like how global events disallow quotation marks in their text due to track event format reasons, spaces are not allowed in local events since values in section data are space-separated. However, there are likely charts with spaces in local events out there somewhere that necessitate allowing these spaces in parsing, or at the very least rejecting charts with them when encountered.

## References

A large part of the info for .chart comes from [FireFox's .chart specifications](https://docs.google.com/document/d/1v2v0U-9HQ5qHeccpExDOLJ5CMPZZ3QytPmAG5WF0Kzs/edit?usp=sharing).

Other info comes from:

- Feedback Editor:
  - [GitHub](https://github.com/TurkeyMan/feedback-editor)
  - [ScoreHero page](https://www.scorehero.com/forum/viewtopic.php?t=7466)
- Some ScoreHero posts:
  - [Guitar Hero Three Control Panel page](https://www.scorehero.com/forum/viewtopic.php?t=72367)
    - [GHTCP song properties window](https://www.scorehero.com/forum/viewtopic.php?t=72367#songproperties)
    - [HoPo metadata tag format](https://www.scorehero.com/forum/viewtopic.php?t=72367#common_06)
- Open GHTCP:
  - [some .chart metadata tags](https://github.com/szymmirr/Open-GHTCP-2021/blob/b42b060bc6074be395243ebcb785bba65793ed20/GHNamespace8/ChartParser.cs#L84)
  - Data types for those tags:
    - [its .chart file output](https://github.com/szymmirr/Open-GHTCP-2021/blob/b42b060bc6074be395243ebcb785bba65793ed20/GHNamespace8/ChartParser.cs#L398)
    - [internal data types](https://github.com/szymmirr/Open-GHTCP-2021/blob/b42b060bc6074be395243ebcb785bba65793ed20/GuitarHero.Songlist/GH3Song.cs)
