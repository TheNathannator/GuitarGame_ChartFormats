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
    - [4-Lane Note Mechanics](#4-lane-note-mechanics)
    - [5-Lane Note Mechanics](#5-lane-note-mechanics)
  - [Special Phrase Types](#special-phrase-types)
    - [Special Phrase Mechanics](#special-phrase-mechanics)
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

1. Check for the `pro_drums` and `five_lane_drums` song.ini tags.
   - If one is present, and set to true, force the drums track to be parsed as that type.
   - If neither are present, fall back to the note type detection.
   - Both being set to true is an invalid state, and either should not be accepted, or one is preferred over the other.
1. Check the chart for notes that indicate the type:
   - If cymbal markers are present, it's a 4-lane Pro track.
   - If the 5-lane green note is present, it's a 5-lane track.
   - If both or neither are present, fall back to standard 4-lane.

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
| Notes     |                             |
| 0         | Kick                        |
| 1         | Red                         |
| 2         | Yellow                      |
| 3         | Blue                        |
| 4         | 5-lane Orange, 4-lane Green |
| 5         | 5-lane Green                |
| 32        | Expert+ kick / 2x kick      |
|           |                             |
| Cymbals   |                             |
| 66        | Yellow cymbal modifier      |
| 67        | Blue cymbal modifier        |
| 68        | Green cymbal modifier       |
|           |                             |
| ??        | Accent and ghost modifiers  |

#### 4-Lane Note Mechanics

4-lane notes are toms by default in .chart. Cymbals are marked using the cymbal modifiers. It is impossible to have a cymbal and tom of the same color on the same tick, and red notes do not have cymbal markers and are always toms.

Expert+ / 2x kick notes are carried over from 5-lane. They are used for kicks faster than around 5-6 kicks per second (most commonly double-kick pedal sections), though there is some nuance to this regarding how long the kick sections last. These are used by alternating these faster kicks between normal kick notes and 2x Kick notes, to make double-kick notes opt-in and allow charts to be feasibly playable on kicks with only a single pedal.

Accent and ghost notes are also carried over from 5-lane. They are notes that notate relatively louder or quieter notes within the section. Hitting them correctly is optional, and typically just grants bonus points. They have not been added to .chart yet, but they are planned.

#### 5-Lane Note Mechanics

In 5-lane, Red, blue, and green are toms, and yellow and orange are cymbals. While in Guitar Hero games they were not distinguished visually in-game, on the drum kit peripheral this is how they are laid out, and this is how they were charted.

Drum sustains are used for imprecise/indiscernible fast rhythms such as drum rolls or cymbal swells. They prevent overhitting and require you to hit faster than a certain threshold (around 4 hits per second in GH) to maintain the sustain. Unlike 4-lane roll lanes, kicks can be drum sustains.

Expert+ / 2x kick notes are used for kicks faster than around 5-6 kicks per second (most commonly double-kick pedal sections), though there is some nuance to this regarding how long the kick sections last. These are used by alternating these faster kicks between normal kick notes and 2x Kick notes, to make double-kick notes opt-in and allow charts to be feasibly playable on kicks with only a single pedal.

Accent and ghost notes are notes that notate relatively louder or quieter notes within the section. Hitting them correctly is optional, and typically just grants bonus points. They have not been added to .chart yet, but they are planned.

### Special Phrase Types

| Special Type | Description                        |
| :----------: | :----------                        |
| Star Power   |                                    |
| 2            | Star Power phrase                  |
| 64           | Drums Star Power activation phrase |
|              |                                    |
| Roll lanes   |                                    |
| ??           | Drum roll markers                  |

#### Special Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

Star Power activation phrases mark a section of a song as an activation point, to allow drummers to activate Star Power without using a pad/cymbal combination or a dedicated button.

- Unlike other special phrases, this phrase includes the very last tick (that is, the tick at the position of `<start tick> + <phrase length>`).
- The length of the phrase is not important in its base functionality, rather, the end of the phrase is used to mark a note as an activation note that when hit will activate Star Power, and when missed will not break combo.
  - The note can either be generated at the exact end-point (overriding the existing notes), or can be an existing note in the chart which is both within the phrase, and closest to the end of the phrase (heed the note about the inclusion of the last tick as part of the phrase, as this phrase is usually marked across an entire measure or two without going slightly into the measure ahead of it).
- Optionally, the length of the phrase may also be used to replicate the original freestyle fill feature from Rock Band that this phrase emulates.

Roll lanes are a 4-lane mechanic used to make imprecise/indiscernible fast rhythms such as drum rolls or cymbal swells easier to play. They prevent overhitting and only require you to hit faster than a certain threshold to hit the charted notes. This threshold can be either a static time threshold, or it can be based off of the actual charted notes. They have not been added to .chart yet, but they are planned.

- Roll lanes are not specific to any lanes, rather they are applied dynamically to a lane based on the notes that are present.
- There are two variants of roll lanes: single-lane, and double-lane.
  - Single-lane rolls are used for drum or cymbal rolls that only apply to a single lane at a time.
  - Double-lane rolls are used for cymbal swells that alternate between two different lanes.
- Roll lanes cannot be applied to kicks.
- If a single-lane roll starts on a chord (excluding kicks), the lane should be marked on the lane matching the next non-chord note.

### Local Events

These are far from the only local events that may be seen, these are just the ones that are commonly supported.

| Event Name                                    | Description                                                       |
| :---------                                    | :----------                                                       |
| `solo`                                        | Starts a solo.                                                    |
| `soloend`                                     | Ends a solo.                                                      |
| `mix_<difficulty>_drums<configuration><flag>` | Sets a specific stem configuration for the chart, detailed below. |

The `mix` event comes from Rock Band, and follows this format:

`mix_<difficulty>_drums<configuration><flags>`

In .chart, this event can be found both with or without brackets, and with either spaces or underscores.

- `difficulty` is a number indicating the difficulty this event applies to:

| Text | Description |
| :--: | :---------- |
| `0`  | Easy        |
| `1`  | Medium      |
| `2`  | Hard        |
| `3`  | Expert      |

- `configuration` is a number corresponding to a specific stem configuration:

| Text                | Description                                                                         |
| :--:                | :----------                                                                         |
| `0`                 | One stereo stem for the entire kit.<br>This is typically used for no stem, as well. |
| `1`                 | Mono kick, mono snare, stereo "other".                                              |
| `2`                 | Mono kick, stereo snare, stereo "other".                                            |
| `3`                 | Stereo kick, snare, and "other".                                                    |
| `4`                 | Mono kick, stereo "other".                                                          |
| `5`                 | Stereo kick, snare, toms, and cymbals.<br>Not part of the RBN docs, rather it is [defined as a community standard](https://strikeline.myjetbrains.com/youtrack/issue/CH-43) based on the GH stem layout. |
| Missing/unspecified | No stems are provided, and configuration should not be applied.                     |

- `flags` is an optional string added to the end of the event that applies modifications outside of setting up the mix:

| Text         | Description |
| :--:         | :---------- |
| `d`          | Known as Disco Flip.<br>On non-Pro, swaps the the snare and other/cymbal+tom stems, such that snare is activated by yellow, and other/cymbal+tom are activated by red.<br>On Pro Drums, flips yellow cymbal/yellow tom and red notes to restore proper playing of the part with cymbals instead of swapping the stems. |
| `dnoflip`    | Swaps the snare and kit/cymbal and tom stems on both non-Pro and Pro Drums, without swapping notes in Pro Drums.<br>- Used in sections where notes should not be flipped in Pro Drums, but the snare and kit/cymbal stems should still be swapped. |
| `easy`       | Unmutes the tom and cymbal stems on Easy.<br>Used in sections where there are no tom or cymbal notes to unmute the "other" stem or tom/cymbal stems. Not supported in RB3, it handles this automatically. |
| `easynokick` | Unmutes the kick stem on Easy.<br>Used in sections where there are no kick notes to unmute the kick stem. Not supported in RB3, it handles this automatically. |

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
