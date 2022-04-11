# Drums in .chart

This document details the format for drums in .chart files.

## Table of Contents

- [Song Section](#song-section)
  - [Metadata](#metadata)
- [Instrument Names](#instrument-names)
  - [Section Names](#section-names)
  - [Drums Track Types](#drums-track-types)
    - [Determining Track Type](#determining-track-type)
  - [Note, Modifier, and Special Phrase Type Divisions](#note-modifier-and-special-phrase-type-divisions)
  - [Note and Modifier Types](#note-and-modifier-types)
  - [Special Phrase Types](#special-phrase-types)
  - [Local Events](#local-events)
- [Example](#example)

## Song Section

### Metadata

| Key            | Description                                                                              | Data type |
| :---           | :----------                                                                              | :-------- |
| `DrumStream`   | Drums audio for the kick drum, plus snare, tom, and cymbal audio if 2-4 are not present. | file path |
| `Drum2Stream`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.    | file path |
| `Drum3Stream`  | Drums audio for toms, plus cymbal audio if 4 is not present.                             | file path |
| `Drum4Stream`  | Drums audio for cymbals.                                                                 | file path |

## Instrument Names

### Section Names

Instrument names:

- `Drums` – Drums/Pro Drums/5-Lane Drums

Difficulties:

- `Expert`
- `Hard`
- `Medium`
- `Easy`

### Drums Track Types

There are three types of drum tracks that may be found in .chart:

- Standard 4-lane
  - Four lanes, drums and cymbals as the same note type
- 4-lane Pro
  - Four lanes, drums and cymbals as separate notes
  - Four drums, three cymbals
- 5-lane
  - Five lanes, drums and cymbals as distinct lanes
  - Three drums, two cymbals

When Pro Drums were added to .chart, no additional specifications were done to allow explicitly distinguishing between different types of drum tracks by instrument name, so the track type must be determined through heuristics.

#### Determining Track Type

The type of a Drums track can be determined using a process such as the following:

- If an accompanying song.ini exists, check if it has either of the `pro_drums` or `five_lane_drums` tags, and whether it is set to true or false. If set to true, then force the drums track to be parsed as if it were that type.
- If there is no song.ini, or if the tags to force a type do not exist, check the chart for 5-lane green and cymbal markers.
  - If 5-lane green is detected,
- If both 5-lane and Pro are detected, it may be preferable to prioritize Pro over 5-lane.

Additionally, if you wish to convert 5-lane to 4-lane Pro or vice versa to allow all charts to be played on all drum kit types, here are some suggested conversions:

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

### Note, Modifier, and Special Phrase Type Divisions

These are the note/modifier type divisions for drums tracks:

| Note Type   | Description                                                      |
| :--------   | :----------                                                      |
| 0 thru 31   | Standard notes/modifiers                                         |
| 32 thru 63  | Guitar Hero-specific notes/modifiers such as drums Expert+ kicks |
| 64 thru 95  | Rock Band-specific notes/modifiers such as drums cymbal markers  |
| 96 thru 127 | Clone Hero-added notes/modifiers                                 |

Any games/programs that want to add custom types should reserve the next set of 32.

### Note and Modifier Types

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

### Special Phrase Types

| Special Type | Description       |
| :----------: | :----------       |
| 2            | Star power phrase |
| 64           | Drums SP activation phrase<br>Applies to the tick after the length. The note closest to the end of the phrase that is also within the phrase should be marked as the activation note. |

### Local Events

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

## Example

```
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
```
