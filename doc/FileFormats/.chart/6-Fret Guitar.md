# 6-Fret Guitar in .chart

This document details the format for 6-fret guitar in .chart files.

## Table of Contents

- [Song Section](#song-section)
  - [Metadata](#metadata)
- [Instrument Sections](#instrument-sections)
  - [Section Names](#section-names)
  - [Note, Modifier, and Special Phrase Type Divisions](#note-modifier-and-special-phrase-type-divisions)
  - [Note and Modifier Types](#note-and-modifier-types)
  - [Special Phrase Types](#special-phrase-types)
  - [Local Events](#local-events)
- [Example](#example)

## Song Section

### Metadata

| Key            | Description        | Data type |
| :---           | :----------        | :-------- |
| `GuitarStream` | Lead Guitar audio. | file path |
| `BassStream`   | Bass Guitar audio. | file path |

## Instrument Sections

### Section Names

Instrument names:

- `GHLGuitar` – Guitar Hero Live Guitar
- `GHLBass` – Guitar Hero Live Bass

Difficulties:

- `Expert`
- `Hard`
- `Medium`
- `Easy`

### Note, Modifier, and Special Phrase Type Divisions

These are the note/modifier type divisions for 6-fret guitar tracks:

| Note Type   | Description                                                      |
| :--------   | :----------                                                      |
| 0 thru 31   | Standard notes/modifiers                                         |
| 32 thru 63  | Guitar Hero-specific notes/modifiers such as drums Expert+ kicks |
| 64 thru 95  | Rock Band-specific notes/modifiers such as drums cymbal markers  |
| 96 thru 127 | Clone Hero-added notes/modifiers                                 |

Any games/programs that want to add custom types should reserve the next set of 32.

### Note and Modifier Types

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

Notes get forced as HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord. In .chart, the default threshold is `(65/192) * <chart resolution>` ticks, rounded down (192 res = 65 tick threshold, 480 res = 162.5 -> 162 tick threshold).

Notes can have their natural forcing flipped using the strum/HOPO flip modifier, referred to as forcing. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.

In .chart, sustains do not get cut off if they are shorter than a certain threshold, unlike .mid.

### Special Phrase Types

| Special Type | Description       |
| :----------: | :----------       |
| 2            | Star power phrase |

### Local Events

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

They are deprecated and should only be supported for legacy purposes. They should not be used for new charts.

## Example

```
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
