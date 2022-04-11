# 5-Fret Guitar in .chart

This document details the format for 5-fret guitar in .chart files.

## Table of Contents

- [Song Section](#song-section)
  - [Metadata](#metadata)
- [Events Section](#events-section)
  - [Hand Positions](#hand-positions)
- [Instrument Sections](#instrument-sections)
  - [Section Names](#section-names)
  - [Note, Modifier, and Special Phrase Type Divisions](#note-modifier-and-special-phrase-type-divisions)
  - [Note and Modifier Types](#note-and-modifier-types)
  - [Special Phrase Types](#special-phrase-types)
  - [Local Events](#local-events)
- [Example](#example)

## Song Section

### Metadata

| Key            | Description          | Data type |
| :---           | :----------          | :-------- |
| `GuitarStream` | Lead Guitar audio.   | file path |
| `RhythmStream` | Rhythm Guitar audio. | file path |
| `BassStream`   | Bass Guitar audio.   | file path |
| `KeysStream`   | Keys audio.          | file path |

## Events Section

### Hand Positions

Some charts may contain track events in the `[Events]` section with the `H` type code. These specify a GH1/2 or Rock Band hand position for character animations, and follow this format:

`<Position> = H <HandPos> <Length>`

- `HandPos` is a number that corresponds to each of the .mid hand position notes, as detailed in the .mid docs. Possible values range from 0 to 19 (though Feedback, the only editor to make use of them, only outputs up to 18 when loading a .mid file that has them).
- `Length` is how long this hand position should be held, in ticks.

As a result of these being in the Events track, they are not specific to an instrument track, and can't really be used outside of converting to a Guitar Hero 1/2 .mid chart. However, it would be safe to assume that some charts may have these in instrument tracks instead, though no currently known examples exist.

## Instrument Sections

### Section Names

Instrument names:

- `Single` – Lead Guitar
- `DoubleGuitar` – Co-op Guitar
- `DoubleBass` – Bass Guitar
- `DoubleRhythm` – Rhythm Guitar
- `Keyboard` – 5-lane Keys
  - While this track is not named for a guitar-type part, it uses the same note/phrase types and may be parsed as one.

Difficulties:

- `Expert`
- `Hard`
- `Medium`
- `Easy`

### Note, Modifier, and Special Phrase Type Divisions

These are the note/modifier type divisions for 5-fret guitar tracks:

| Note Type   | Description                                                      |
| :--------   | :----------                                                      |
| 0 thru 31   | Standard notes/modifiers                                         |
| 32 thru 63  | Guitar Hero-specific notes/modifiers such as drums Expert+ kicks |
| 64 thru 95  | Rock Band-specific notes/modifiers such as drums cymbal markers  |
| 96 thru 127 | Clone Hero-added notes/modifiers                                 |

Any games/programs that want to add custom types should reserve the next set of 32.

### Note and Modifier Types

| Note Type | Description                                       |
| :-------: | :----------                                       |
| 0         | Green (1st fret) note                             |
| 1         | Red (2nd fret) note                               |
| 2         | Yellow (3rd fret) note                            |
| 3         | Blue (4th fret) note                              |
| 4         | Orange (5th fret) note                            |
| 5         | Strum/HOPO flip modifier                          |
| 6         | Tap modifier<br>Overrides the HOPO flip modifier. |
| 7         | Open note                                         |

Notes get forced as HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord. In .chart, the default threshold is `(65/192) * <chart resolution>` ticks, rounded down (192 res = 65 tick threshold, 480 res = 162.5 -> 162 tick threshold).

Notes can have their natural forcing flipped using the strum/HOPO flip modifier, referred to as forcing. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.

Sustains do not get cut off if they are shorter than a certain threshold, unlike .mid.

### Special Phrase Types

| Special Type | Description                    |
| :----------: | :----------                    |
| 0            | GH1/2 Face-Off player 1 phrase |
| 1            | GH1/2 Face-Off player 2 phrase |
| 2            | Star power phrase              |

### Local Events

These are far from the only local events that may be seen, these are just the ones that are commonly supported.

| Event Name | Description    |
| :--------- | :----------    |
| `solo`     | Starts a solo. |
| `soloend`  | Ends a solo.   |

Other local events that can be found come from Guitar Hero or Rock Band charts.

## Example

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
```
