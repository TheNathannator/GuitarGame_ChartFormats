# .chart

.chart is a text-based custom chart format originating from the GH1/2 era. It was originally created as an intermediate format, meant to be converted for use in a game, but nowadays it is typically used directly.

## Table of Contents

- [Brief History](#brief-history)
- [Basic Infrastructure](#basic-infrastructure)
  - [Sections](#sections)
    - [Section Data](#section-data)
  - [Section Names](#section-names)
- [Song Section Details](#song-section-details)
  - [Common Metadata](#common-metadata)
    - [Audio Streams](#audio-streams)
  - [Legacy Metadata](#legacy-metadata)
  - [Song Section Example](#song-section-example)
- [Track Events](#track-events)
- [SyncTrack Section Details](#synctrack-section-details)
  - [Time Signatures](#time-signatures)
  - [Tempos](#tempos)
  - [Tempo Anchors](#tempo-anchors)
  - [SyncTrack Section Example](#synctrack-section-example)
- [Events Section Details](#events-section-details)
  - [Global Events](#global-events)
    - [Common Global Events](#common-global-events)
    - [Lyrics](#lyrics)
  - [Hand Positions](#hand-positions)
  - [Events Section Example](#events-section-example)
- [Instrument Section Details](#instrument-section-details)
  - [Notes and Modifiers](#notes-and-modifiers)
    - [Note and Modifier Type Divisions](#note-and-modifier-type-divisions)
  - [Special Phrases](#special-phrases)
    - [Special Phrase Type Divisions](#special-phrase-type-divisions)
  - [Local Events](#local-events)
- [Instrument Sections](#instrument-sections)
  - [5-Fret Tracks](#5-fret-tracks)
    - [5-Fret Note and Modifier Types](#5-fret-note-and-modifier-types)
    - [5-Fret Special Phrase Types](#5-fret-special-phrase-types)
    - [5-Fret Local Events](#5-fret-local-events)
  - [6-Fret Tracks](#6-fret-tracks)
    - [6-Fret Note and Modifier Types](#6-fret-note-and-modifier-types)
    - [6-Fret Special Phrase Types](#6-fret-special-phrase-types)
    - [6-Fret Local Events](#6-fret-local-events)
  - [Drums Track](#drums-track)
    - [Drums Note and Modifier Types](#drums-note-and-modifier-types)
    - [Drums Special Phrase Types](#drums-special-phrase-types)
    - [Drums Local Events](#drums-local-events)
    - [Drums Track Type Determining](#drums-track-type-determining)
  - [Instrument Section Examples](#instrument-section-examples)
- [References](#references)

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

Difficulty names:

- `Expert`
- `Hard`
- `Medium`
- `Easy`
  
Standard instrument names:

- `Single` – Lead Guitar
- `DoubleGuitar` – Co-op Guitar
- `DoubleBass` – Bass Guitar
- `DoubleRhythm` – Rhythm Guitar
- `Keyboard` – 5-lane Keys
- `Drums` – Drums/Pro Drums/5-Lane Drums
- `GHLGuitar` – Guitar Hero Live Guitar
- `GHLBass` – Guitar Hero Live Bass

Legacy instrument names:

- These are not fully documented, as they are not necessarily standard and Feedback just used the 5-fret track layout for all of the ones it supported.

- `SingleBass` - present in some older charts, but was never actually used in any games and is considered to be unsupported
- `EnhancedGuitar` - available in Feedback Editor
- `CoopLead` - available in Feedback Editor
- `CoopBass` - available in Feedback Editor
- `10KeyGuitar` - available in Feedback Editor
- `DoubleDrums` - available in Feedback Editor
- `Vocals` - available in Feedback Editor

Any other unrecognized/unknown sections should be ignored.

All sections aside from `Song` (for chart resolution) and `SyncTrack` are optional, and instrument sections may show up in any order. A missing section is equivalent to a section with no data.

## Song Section Details

The `[Song]` section contains metadata about the song and chart.

Its data follows this format:

`<Name> = <Value>`

- `Name` is the name of the metadata entry, typically written in PascalCase (though their casing may vary). These should be unique, and any duplicates should be ignored.
- `Value` is the value of the metadata entry. The data type of `Value` depends on which entry it is paired to.

The order of metadata tags may vary.

Note: .chart files made for Clone Hero often don't have this section properly filled out, they instead use an accompanying song.ini file. The metadata in the song.ini should be prioritized over the metadata in the .chart wherever possible. See [Song_ini.md](Song_ini.md) for details on the song.ini file.

The following lists of metadata tags are most likely not fully comprehensive. Unknown/unexpected tags should be ignored during parsing.

In the descriptions, GHTCP stands for Guitar Hero Three Control Panel, a tool to modify Guitar Hero 3, which includes importing songs. Feedback refers to the Feedback Editor program.

### Common Metadata

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

#### Audio Streams

Often times, paths to audio files to play during the song are included here. These paths can be relative to the chart file, or absolute. Some files are specific to an instrument and may be muted depending on the performance of the player behind the instrument.

| Key            | Description                                                                           | Data type |
| :---           | :----------                                                                           | :-------- |
| `MusicStream`  | Background audio that never gets muted by gameplay.                                   | file path |
| `GuitarStream` | Lead Guitar audio.                                                                    | file path |
| `RhythmStream` | Rhythm Guitar audio.                                                                  | file path |
| `BassStream`   | Bass Guitar audio.                                                                    | file path |
| `KeysStream`   | Keys audio.                                                                           | file path |
| `DrumStream`   | Drums kick drum audio, plus snare, tom, and cymbal audio if 2-4 are not present.      | file path |
| `Drum2Stream`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present. | file path |
| `Drum3Stream`  | Drums audio for toms, plus cymbal audio if 4 is not present.                          | file path |
| `Drum4Stream`  | Drums audio for cymbals.                                                              | file path |
| `VocalStream`  | Vocals audio.                                                                         | file path |
| `CrowdStream`  | Background crowd noise/singing audio.                                                 | file path |

Note that charts made for Clone Hero don't necessarily use these paths intentionally. Clone Hero has static names for these tracks, and dynamically includes them based on their presence. These are detailed in [Overview.md](_Overview.md).

### Legacy Metadata

These are tags that aren't commonly used nowadays, either in charts or in games that support .chart files.

| Key            | Description                                                                                                  | Data type |
| :---           | :----------                                                                                                  | :-------- |
| `ArtistText`   | (GHTCP) Text to use before the artist name.<br>Can be custom, but most commonly "by" or "as made famous by". | string    |
| `Singer`       | (GHTCP) Indicates which singer should be used in-game.                                                       | string    |
| `Bassist`      | (GHTCP) Indicates which bassist should be used in-game.                                                      | string    |
| `Boss`         | (GHTCP) Indicates whether or not this is a boss song?<br>Unsure what data is valid for this tag.             | string    |
| `Player2`      | (GHTCP) Instrument to use for player 2.<br>Valid values are `Bass`/`bass` and `Rhythm`/`rhythm`.             | string (no quotes) |
| `CountOff`     | (GHTCP) The countoff sample to use in-game.                                                                  | string    |
| `GuitarVolume` | (GHTCP) Sets the volume for the guitar audio track.                                                          | float     |
| `GuitarVol`    | (GHTCP) Alias for the above.                                                                                 | float     |
| `BandVolume`   | (GHTCP) Sets the volume for the band audio track.                                                            | float     |
| `BandVol`      | (GHTCP) Alias for the above.                                                                                 | float     |
| `HoPo`         | (GHTCP) The HOPO threshold that should be used for this chart.<br>This value is equal to `(<step size denominator>) / 4` (1/4 step = 1.00, 1/8 = 2.00, 1/2 = 0.50, etc.). | float |
| `Fretboard`    | (Feedback) File path to a fretboard image for this song.<br>At least with how Feedback saves it, the file extension is not included. | file path |
| `MediaType`    | (Unknown origins) Type of media the song released on?                                                        | string    |

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

#### Common Global Events

| Name               | Description                                                                                                     |
| :---               | :----------                                                                                                     |
| `phrase_start`     | Marks the start of a new lyrics phrase.<br>Can be used to mark a new phrase without using a prior `phrase_end`. |
| `phrase_end`       | Marks the end of the current lyrics phrase.                                                                     |
| `lyric <syllable>` | Contains a syllable of a lyrics phrase.                                                                         |
| `section <name>`   | Marks the start point of a section, used by Practice mode and post-game summary.                                |
| `prc_<name>`       | Same as the above.                                                                                              |
| `end`              | Marks the end of a song.                                                                                        |

#### Lyrics

Lyrics are laid out as `phrase_start`, `phrase_end`, and `lyric` events throughout the `Events` section.

- `phrase_start` marks the start of a phrase of lyrics A preceding `phrase_end` is not required to start a new phrase from an existing one.
- `phrase_end` marks the end of a phrase.
- Any `lyric` events between a `phrase_start` and either a `phrase_end` event or a new `phrase_start` event should be concatenated into a single line. The text of this phrase may be highlighted one syllable at a time as `lyric` events are passed.

There are various symbols used for specific things in lyrics:

| Name           | Symbol | Description                                                                                               |
| :---           | :----: | :----------                                                                                               |
| Hyphens        | `-`    | Indicates that this syllable should be combined with the next.                                            |
| Pluses         | `+`    | In Rock Band, will connect the previous note and the current note into a slide. Usually found standalone. |
| Equals         | `=`    | Indicates that a syllable should be joined with the next using a literal hyphen.                          |
| Pounds         | `#`    | In Rock Band, this marks a note as non-pitched.                                                           |
| Carets         | `^`    | In Rock Band, this marks a note as non-pitched with a more generous scoring.<br>Typically used on short syllables or syllables without sharp attacks. |
| Asterisks      | `*`    | In Rock Band, this marks a note as non-pitched, but their differentiation is unknown.                     |
| Percents       | `%`    | In Rock Band, these are a range divider marker for vocals parts with large octave ranges.                 |
| Sections       | `§`    | In Rock Band, these are used to indicate that two syllables are sung as a single syllable, used in some Spanish lyrics. These are displayed with a tie character `‿` in RB. |
| Dollars        | `$`    | In Rock Band, these are used in harmonies to mark the syllables they are part of to be hidden.<br>In one case it is also on the standard Vocals track for some reason, it appears to not do anything in RB here. |
| Slashes        | `/`    | These appear in some RB charts for an unknown reason, mainly The Beatles: Rock Band.<br>They are also used in a workaround to use quotation marks in global events in older versions of Clone Hero. |
| Underscores    | `_`    | These are replaced with a space by Clone Hero for the purpose of having a character that does that.       |
| Angle brackets | `<>`   | In Clone Hero's public test build, some [formatting tags](http://digitalnativestudios.com/textmeshpro/docs/rich-text/) are supported in lyrics. For other games that don't use these tags, they need to be stripped out. |

Syllables not joined together through any symbols should be separated by a space during parsing.

Here's how these symbols should be handled for displaying as just text:

- Strip out `-`, `+`, `#`, `^`, `*`, `%`, `$`, `/`, `<>`, and anything between `<>`.
- Replace `=` with a hyphen `-`. Make sure to not strip out hyphens added as replacement for equals.
- Replace `§`, `_` with a space.
- Join together a syllable that has `-` or `=` at the end of it with the following syllable.

### Hand Positions

Some charts may contain track events here with the `H` type code. These specify a GH1/2 or Rock Band hand position for character animations, and follow this format:

`<Position> = H <HandPos> <Length>`

- `HandPos` is a number that corresponds to each of the .mid hand position notes, as detailed in [Midi_GuitarHero1-2.md](../GuitarHero1-2/Midi_GuitarHero1-2.md#anim-notes) and [Midi_RockBand.md](../RockBand/Midi_RockBand.md#5-fret-notes). Possible values range from 0 to 19 (though Feedback only outputs up to 18 when loading a .mid file that has them).
- `Length` is how long this hand position should be held, in ticks.

As a result of these being in the Events track, they are not specific to an instrument track, and can't really be used outside of converting to a Guitar Hero 1/2 .mid chart. However, it is very much possible that some charts may have these in instrument tracks instead, though no currently known examples exist, it just seems reasonable that it might have happened or will happen.

### Events Section Example

```
[Events]
{
  576 = E "phrase_start"
  768 = E "lyric This"
  768 = E "section Notes Start"
  864 = E "lyric is"
  960 = E "lyric a"
  1056 = E "lyric lyr-"
  1152 = E "lyric ics"
  1248 = E "lyric test"
  1536 = E "phrase_end"
  2880 = E "phrase_start"
  3072 = E "lyric This"
  3168 = E "lyric is"
  3264 = E "lyric a"
  3312 = E "phrase_start"
  3360 = E "lyric Lyr-"
  3456 = E "lyric ics"
  3552 = E "lyric test"
  3840 = E "phrase_end"
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

Notes get forced as HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord. In .chart, the default threshold is `(65/192) * <chart resolution>` ticks, rounded down (192 res = 65 tick threshold, 480 res = 162.5 -> 162 tick threshold).

Notes can have their natural forcing flipped using the strum/HOPO flip modifier, referred to as forcing. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.

In .chart, sustains do not get cut off if they are shorter than a certain threshold, unlike .mid.

#### Note and Modifier Type Divisions

To help prevent issues such as Black 3 in the 6-fret notes having a non-sequential value of 8, it has been decided that moving forward, types will be divided into groups of 32:

| Note Type   | Description                                                      |
| :--------   | :----------                                                      |
| 0 thru 31   | Standard notes/modifiers                                         |
| 32 thru 63  | Guitar Hero-specific notes/modifiers such as drums Expert+ kicks |
| 64 thru 95  | Rock Band-specific notes/modifiers such as drums cymbal markers  |
| 96 thru 127 | Clone Hero-added notes/modifiers                                 |

Any games/programs that want to add custom note/modifier types should reserve the next set of 32.

### Special Phrases

Special phrase events mark phrase-type events such as Star Power. They use the `S` type code and are written like this:

`<Position> = S <Type> <Length>`

- `Type` is a the type number of this special phrase.
- `Length` is the length of this phrase in ticks.

A special phrase usually does not apply to the tick immediately after the length, i.e. the value of `(Position + Length)`. Any exceptions will be noted. They should also apply to their starting tick regardless of length, since, for example, some charts use 0-length Star Power phrases to mark single-note/chord phrases.

#### Special Phrase Type Divisions

The same type divisions of 32 as for notes/modifiers is applied here:

| Special Type | Description                                                                 |
| :----------- | :----------                                                                 |
| 0 thru 31    | Standard specials                                                           |
| 32 thru 63   | Guitar Hero-specific specials (that aren't the Face-Off Player 1/2 phrases) |
| 64 thru 95   | Rock Band-specific specials such as drums SP activation phrases             |
| 96 thru 127  | Clone Hero-added specials                                                   |

### Local Events

Local events are events that appear in instrument tracks. They use the `E` type code, and are written like this:

`<Position> = E <Text>`

`Text` is a string *without* quotes that contains the event data. Spaces are disallowed in these events by some programs, but there are likely charts with spaces in local events out there somewhere.

## Instrument Sections

This section details instrument-specific note/phrase types and common local events. Legacy tracks are not detailed, as there is no reference for how they would be used.

### 5-Fret Tracks

- `Single` – Lead Guitar
- `DoubleGuitar` – Co-op Guitar
- `DoubleBass` – Bass Guitar
- `DoubleRhythm` – Rhythm Guitar
- `Keyboard` – 5-lane Keys

#### 5-Fret Note and Modifier Types

| Note Type | Description                                      |
| :-------: | :----------                                      |
| 0         | Green (1st fret) note                            |
| 1         | Red (2nd fret) note                              |
| 2         | Yellow (3rd fret) note                           |
| 3         | Blue (4th fret) note                             |
| 4         | Orange (5th fret) note                           |
| 5         | Strum/HOPO flip modifier                         |
| 6         | Tap modifier<br>Overrides the HOPO flip modifier |
| 7         | Open note                                        |

#### 5-Fret Special Phrase Types

| Special Type | Description                    |
| :----------: | :----------                    |
| 0            | GH1/2 Face-Off player 1 phrase |
| 1            | GH1/2 Face-Off player 2 phrase |
| 2            | Star power phrase              |

#### 5-Fret Local Events

These are far from the only local events that may be seen, these are just the ones that are commonly supported.

| Event Name | Description    |
| :--------- | :----------    |
| `solo`     | Starts a solo. |
| `soloend`  | Ends a solo.   |

### 6-Fret Tracks

- `GHLGuitar` – Guitar Hero Live Guitar
- `GHLBass` – Guitar Hero Live Bass

#### 6-Fret Note and Modifier Types

| Note Type | Description                                    |
| :-------: | :----------                                    |
| 0         | White 1 (4th fret) note                        |
| 1         | White 2 (5th fret) note                        |
| 2         | White 3 (6th fret) note                        |
| 3         | Black 1 (1st fret) note                        |
| 4         | Black 2 (2nd fret) note                        |
| 5         | Strum/HOPO flip modifier                       |
| 6         | Tap modifier, overrides the HOPO flip modifier |
| 7         | Open note                                      |
| 8         | Black 3 (3rd fret) note                        |

#### 6-Fret Special Phrase Types

| Special Type | Description       |
| :----------: | :----------       |
| 2            | Star power phrase |

#### 6-Fret Local Events

These are far from the only local events that may be seen, these are just the ones that are commonly supported.

| Event Name | Description    |
| :--------- | :----------    |
| `solo`     | Starts a solo. |
| `soloend`  | Ends a solo.   |

These are remnants from the early days of Guitar Hero Live .chart charting:

| Event Name     | Description                                    |
| :---------     | :----------                                    |
| `ghl_6`        | Marks a note as a 6-fret 6th fret note.        |
| `ghl_6_forced` | Marks a note as a forced 6-fret 6th fret note. |

### Drums Track

- `Drums` – Drums/Pro Drums/5-Lane Drums

#### Drums Note and Modifier Types

| Note Type | Description                 |
| :-------: | :----------                 |
| 0         | Kick                        |
| 1         | Red                         |
| 2         | Yellow                      |
| 3         | Blue                        |
| 4         | 5-lane Orange, 4-lane Green |
| 5         | 5-lane Green                |
| 32        | 2x kick                     |
| 66        | Yellow cymbal modifier      |
| 67        | Blue cymbal modifier        |
| 68        | Green cymbal modifier       |

Additional info:

- 4-Lane:
  - Notes are toms by default in .chart. Cymbals are marked using note types 66-68, excluding red which is always a tom note.
    - This makes it impossible to have a tom and cymbal of the same color at the same position.
  - Roll lanes are not implemented in .chart yet, they are currently only available in .mid.<!-- Roll lanes are used to make imprecise/indiscernible fast rhythms such as drum rolls or cymbal swells easier to play. They prevent overhitting and only require you to hit faster than a certain threshold to hit the charted notes. -->
    <!-- - During parsing, if a 1-lane roll starts on a chord (excluding kicks), the lane should be marked on the lane matching the next non-chord note. -->
    <!-- - Roll lanes cannot be applied to kicks. -->
- 5-lane:
  - Red, blue, and green are toms, yellow and orange are cymbals.
  - Drum sustains are used for imprecise/indiscernible fast rhythms such as drum rolls or cymbal swells. They prevent overhitting and require you to hit faster than a certain threshold (around 4 hits per second in GH) to maintain the sustain.
    - Unlike 4-lane roll lanes, kicks can be drum sustains.
- Expert+ kicks are used for double-kick sections or kicks faster than around 5-6 kicks per second, alternating these faster kicks between normal and Expert+ to make these sections playable with a single pedal.
  - In gameplay, they are equivalent to normal kicks, but they should only be visible if the user enables them.
- Accent and ghost notes are not implemented in .chart yet, they are currently only available in .mid.<!-- Accent and ghost notes are notes that need to be hit either hard or soft, respectively. -->
  <!-- - (Preemptive assumption) These are marked with their respective note modifiers. -->
  <!-- - (May be unnecessary for .chart, as there's nothing that would conflict with how accents/ghosts would theoretically be marked) Clone Hero requires an `[ENABLE_CHART_DYNAMICS]`/`ENABLE_CHART_DYNAMICS` text event to be present to enable ghosts/accents, in order to preserve compatibility with charts that weren't charted with velocity in mind. Using this approach is recommended. -->

#### Drums Special Phrase Types

| Special Type | Description       |
| :----------: | :----------       |
| 2            | Star power phrase |
| 64           | Drums SP activation phrase<br>Applies to the tick after the length. The note closest to the end of the phrase that is also within the phrase should be marked as the activation note. |

#### Drums Local Events

These are far from the only local events that may be seen, these are just the ones that are commonly supported.
<!--
I would like to use a table here instead of a list for consistency, but the mix event structure just doesn't lay out well in a table
| Event Name | Description
| :--------- | :-----------
-->

- `solo` - Starts a solo.
- `soloend` - Ends a solo.

Mix events:

`mix_<difficulty>_drums<configuration><flag>` - Sets a stem configuration for drums. This event can be found both with or without brackets, and with either spaces or underscores.

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
| `easy`       | Unmutes the tom and cymbal gems on Easy.<br>- Used in sections where there are no tom or cymbal gems to unmute the kit stem or tom/cymbal stems. Not supported in RB3, it handles this automatically. |
| `easynokick` | Unmutes the other/cymbal+tom stem on Easy.<br>- Used in sections where there are no kick gems to unmute the kick stem. Not supported in RB3, it handles this automatically. |

Aside from `d` and `dnoflip`, detecting stem configuration can be done automatically and these mix events can be ignored if that approach is chosen. If these events are used for more than just disco flip in a chart, then the `configuration` number should remain constant throughout the chart.

#### Drums Track Type Determining

The Drums track doesn't have any way of specifying which kind of drums track it is from within the .chart file itself, its type must be determined manually. The same goes for .mid files.

The type can be determined using a process such as the following:

- Check if an accompanying song.ini has either the `pro_drums` or `five_lane_drums` tags. If it does, then force the drums track to be parsed as if it were that type.
- If there is no song.ini, or if the tags to force a type do not exist, check the chart for 5-lane green and cymbal markers.
- If both 5-lane and Pro are detected, it may be preferable to prioritize Pro over 5-lane.

Additionally, if you wish to convert 5-lane to 4-lane Pro, or vice versa, here are some suggested conversions:

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

### Instrument Section Examples

```
[ExpertSingle]
{
  768 = N 0 0
  768 = S 64 768
  864 = N 1 0
  864 = N 5 0
  960 = N 2 0
  960 = N 6 0
  1056 = N 3 0
  1056 = E solo
  1152 = N 4 0
  1248 = N 7 0
  1248 = E soloend
}
[HardDrums]
{
  768 = N 1 0
  768 = S 64 768
  864 = N 2 0
  960 = N 3 0
  1056 = N 4 0
  1152 = N 0 0
  1248 = N 2 0
  1248 = N 66 0
  1344 = N 3 0
  1344 = N 67 0
  1440 = N 4 0
  1440 = N 68 0
  1536 = N 32 0
}
[MediumGHLGuitar]
{
  768 = N 3 0
  864 = N 4 0
  864 = N 5 0
  960 = N 8 0
  960 = N 6 0
  1056 = N 0 0
  1056 = E solo
  1152 = N 1 0
  1248 = N 2 0
  1344 = N 7 0
  1344 = E soloend
}
```

## References

A large part of this info comes from [FireFox's .chart specifications](https://docs.google.com/document/d/1v2v0U-9HQ5qHeccpExDOLJ5CMPZZ3QytPmAG5WF0Kzs/edit?usp=sharing).

Other info comes from:

- Feedback Editor:
  - [GitHub](https://github.com/TurkeyMan/feedback-editor)
  - [ScoreHero page](https://www.scorehero.com/forum/viewtopic.php?t=7466)
- Some ScoreHero posts:
  - [GHTCP page](https://www.scorehero.com/forum/viewtopic.php?t=72367)
    - [GHTCP song properties window](https://www.scorehero.com/forum/viewtopic.php?t=72367#songproperties)
    - [HoPo metadata tag format](https://www.scorehero.com/forum/viewtopic.php?t=72367#common_06)
- Open GHTCP:
  - [some .chart metadata tags](https://github.com/szymmirr/Open-GHTCP-2021/blob/b42b060bc6074be395243ebcb785bba65793ed20/GHNamespace8/ChartParser.cs#L84)
  - Data types for those tags:
    - [its .chart file output](https://github.com/szymmirr/Open-GHTCP-2021/blob/b42b060bc6074be395243ebcb785bba65793ed20/GHNamespace8/ChartParser.cs#L398)
    - [internal data types](https://github.com/szymmirr/Open-GHTCP-2021/blob/b42b060bc6074be395243ebcb785bba65793ed20/GuitarHero.Songlist/GH3Song.cs)
