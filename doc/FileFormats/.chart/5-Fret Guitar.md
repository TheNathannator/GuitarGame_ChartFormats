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
    - [Note Mechanics](#note-mechanics)
  - [Special Phrase Types](#special-phrase-types)
    - [Special Phrase Mechanics](#special-phrase-mechanics)
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
  - While this track is not named for a guitar-type instrument, the game it comes from allows for playing it as if it were one.

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

| Note Type | Description                                       |
| :-------: | :----------                                       |
| Notes     |                                                   |
| 0         | Green (1st fret) note                             |
| 1         | Red (2nd fret) note                               |
| 2         | Yellow (3rd fret) note                            |
| 3         | Blue (4th fret) note                              |
| 4         | Orange (5th fret) note                            |
| 7         | Open note                                         |
|           |                                                   |
| Modifiers |                                                   |
| 5         | Strum/HOPO flip modifier                          |
| 6         | Tap modifier<br>Overrides the HOPO flip modifier. |

#### Note Mechanics

Notes are strum notes by default. They get turned into HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord. In .chart, the default threshold is `(65/192) * <chart resolution>` ticks, rounded down (192 res = 65 tick threshold, 480 res = 162.5 -> 162 tick threshold).

Notes can have their natural state flipped using the strum/HOPO flip modifier, referred to as forcing. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.

Sustains do not get cut off if they are shorter than a certain threshold, unlike .mid.

Chords may have individual notes with different lengths. These are referred to as extended sustains (start at different times, end at same or different times) and disjoint chords (start at the same time, end at different times).

### Special Phrase Types

| Special Type | Description                    |
| :----------: | :----------                    |
| Face-Off     |                                |
| 0            | GH1/2 Face-Off player 1 phrase |
| 1            | GH1/2 Face-Off player 2 phrase |
|              |                                |
| Star Power   |                                |
| 2            | Star Power phrase              |

#### Special Phrase Mechanics

Face-Off phrases designate a section of the chart to be played in a face-off/versus mode by one or both players. In this mode, players play against each other, trading off between sections or playing at the same time.

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

### Local Events

These are far from the only local events that may be seen, these are just the ones that are commonly supported.

| Event Name | Description    |
| :--------- | :----------    |
| `solo`     | Starts a solo. |
| `soloend`  | Ends a solo.<br>Any notes at the same position are included in the solo. |

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
