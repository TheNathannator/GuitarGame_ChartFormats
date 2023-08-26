# 6-Fret Guitar in .chart

This document details the format for 6-fret guitar in .chart files.

## Table of Contents

- [Song Section](#song-section)
  - [Metadata](#metadata)
- [Instrument Sections](#instrument-sections)
  - [Section Names](#section-names)
  - [Note, Modifier, and Special Phrase Type Divisions](#note-modifier-and-special-phrase-type-divisions)
  - [Note and Modifier Types](#note-and-modifier-types)
    - [Note Mechanics](#note-mechanics)
  - [Special Phrase Types](#special-phrase-types)
    - [Special Phrase Mechanics](#special-phrase-mechanics)
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
- `GHLRhythm` - Guitar Hero Live Rhythm
- `GHLCoop` - Guitar Hero Live Co-op

Difficulties:

- `Expert`
- `Hard`
- `Medium`
- `Easy`

### Note, Modifier, and Special Phrase Type Divisions

| Note Type    | Reservation
| :--------    | :----------
| 0 thru 31    | Standard
| 32 thru 63   | Guitar Hero
| 64 thru 95   | Rock Band
| 96 thru 127  | Clone Hero/Strikeline

### Note and Modifier Types

| Note Type | Description                                    |
| :-------: | :----------                                    |
| Notes     |                                                |
| 0         | White 1 (4th fret) note                        |
| 1         | White 2 (5th fret) note                        |
| 2         | White 3 (6th fret) note                        |
| 3         | Black 1 (1st fret) note                        |
| 4         | Black 2 (2nd fret) note                        |
| 8         | Black 3 (3rd fret) note                        |
| 7         | Open note                                      |
|           |                                                |
| Modifiers |                                                |
| 5         | Strum/HOPO flip modifier                       |
| 6         | Tap modifier, overrides the HOPO flip modifier |

#### Note Mechanics

Notes are strum notes by default. They get turned into HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord. In .chart, the default threshold is `(65/192) * <chart resolution>` ticks, rounded down (192 res = 65 tick threshold, 480 res = 162.5 -> 162 tick threshold).

Notes can have their natural state flipped using the strum/HOPO flip modifier, referred to as forcing. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.

Sustains do not get cut off if they are shorter than a certain threshold, unlike .mid.

Chords may have individual notes with different lengths. These are referred to as extended sustains (start at different times, end at same or different times) and disjoint chords (start at the same time, end at different times).

### Special Phrase Types

| Special Type | Description       |
| :----------: | :----------       |
| Star Power   |                   |
| 2            | Star Power phrase |

#### Special Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

### Local Events

These are far from the only local events that may be seen, these are just the ones that are commonly supported.

| Event Name | Description    |
| :--------- | :----------    |
| `solo`     | Starts a solo. |
| `soloend`  | Ends a solo.<br>Any notes at the same position are included in the solo. |

These are remnants from the early days of Guitar Hero Live .chart charting. They are deprecated and were only used to be able to export to the .mid 6-fret format before .chart got its own 6-fret format. They should not be used for new charts.

| Event Name     | Description                                    |
| :---------     | :----------                                    |
| `ghl_6`        | Marks a note as a 6-fret 6th fret note.        |
| `ghl_6_forced` | Marks a note as a forced 6-fret 6th fret note. |

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
