# .chart DJ Hero Track Specifications (Proposal)

This document details a proposal for adding DJ Hero tracks to the .chart format.

For specifications on the .chart format itself, see the [CoreFormats .chart document.](../CoreFormats/Chart.md)

## Table of Contents

- [Section Names](#section-names)
- [Metadata](#metadata)
- [Global Events](#global-events)
- [Track Details](#track-details)
  - [Note, Modifier, and Special Phrase Type Divisions](#note-modifier-and-special-phrase-type-divisions)
  - [Turntable Tracks](#turntable-tracks)
    - [Turntable Note Types](#turntable-note-types)
    - [Turntable Special Phrase Types](#turntable-special-phrase-types)
    - [Turntable Local Events](#turntable-local-events)

## Section Names

Added difficulties:

- `Beginner`

Tracks:

- `DJTurntable` - Single-table track
- `DJDualTurntable` - Dual-table track

## Metadata

| Key                | Description                                                                           | Data type |
| :---               | :----------                                                                           | :-------- |
| `DJArtist`         | The DJ/producer who mixed the song.                                                   | string    |
| `DJGreenStream`    | Green track audio.                                                                    | file path |
| `DJRedStream`      | Red track audio.                                                                      | file path |
| `DJBlueStream`     | Blue track audio.                                                                     | file path |
| `DJSample<number>` | An audio sample.<br>`number` is the number used to refer to this sample in the chart. | file path |

## Global Events

The standard `section` and `prc_` events are extended to mark rewind checkpoints, in addition to the following events:

| Event Text                                          | Description |
| :--------:                                          | :---------- |
| `dj_rewind_checkpoint [name]`                       | Marks an additional rewind checkpoint.<br>`name` is an optional name given to the checkpoint/section. |
| `dj_megamix_intro_end`                              | Marks the point that should be skipped to when skipping a megamix's extended intro.
| `dj_megamix_transition`                             | Marks the transition point of a megamix.
| `dj_battle_lanes <green> <red> <blue>`              | Specifies which players get which lanes in a DJ battle.<br>`green` specifies which players get the green lane: `p1`, `p2`, or `both`. `red` and `blue` for the red and blue lanes follow the same syntax. |
| `djdual_battle_lanes <g1> <r1> <b1> <g2> <r2> <b2>` | Specifies which players get which lanes in a dual-table DJ battle.<br>This event's parameters work the same as `dj_battle_event`, it just has 3 additional lane parameters for dual-table. |
| `dj_battle_checkpoint [name]`                       | Marks a battle checkpoint.<br>`name` is an optional name for the checkpoint. |

## Track Details

### Note, Modifier, and Special Phrase Type Divisions

These tracks use different type divisions than the standard .chart tracks:

- Note/modifier types:

| Note Type   | Description                        |
| :--------   | :----------                        |
| 0 thru 31   | 1st table taps/scratches/freestyle |
| 32 thru 63  | 2nd table taps/scratches/freestyle |
| 64 thru 95  | Crossfade markers                  |
| 96 thru 127 | Miscellaneous                      |

- Special phrase types:

| Note Type   | Description              |
| :--------   | :----------              |
| 0 thru 31   | Standard special phrases |
| 32 thru 63  | 1st table phrases        |
| 64 thru 95  | 2nd table phrases        |
| 96 thru 127 | Miscellaneous            |

### Turntable Tracks

- `DJTurntable`
- `DJDualTurntable`

Note that this section specifies types for both tracks in a single section. Types specific to a track will be specified as such.

#### Turntable Note Types

| Note Type     | Description                                                                              |
| :-------:     | :----------                                                                              |
| First table   |                                                                                          |
| 0             | Table 1: Green (1st lane) tap                                                            |
| 1             | Table 1: Red (2nd lane) tap                                                              |
| 2             | Table 1: Blue (3rd lane) tap                                                             |
|               |                                                                                          |
| 3             | Table 1: Green scratch up                                                                |
| 4             | Table 1: Red scratch up                                                                  |
| 5             | Table 1: Blue scratch up                                                                 |
| 6             | Table 1: Green scratch down                                                              |
| 7             | Table 1: Red scratch down                                                                |
| 8             | Table 1: Blue scratch down                                                               |
| 9             | Table 1: Green scratch any direction                                                     |
| 10            | Table 1: Red scratch any direction                                                       |
| 11            | Table 1: Blue scratch any direction                                                      |
| 12            | Table 1: Green scratch zone                                                              |
| 13            | Table 1: Red scratch zone                                                                |
| 14            | Table 1: Blue scratch zone                                                               |
|               |                                                                                          |
| 15            | Table 1: Green freestyle scratch zone<br>Sample determined by a `dj_sample` local event. |
| 16            | Table 1: Red freestyle scratch zone<br>Sample determined by a `dj_sample` local event.   |
| 17            | Table 1: Blue freestyle scratch zone<br>Sample determined by a `dj_sample` local event.  |
|               |                                                                                          |
| 18            | Table 1: Green freestyle sample zone<br>Sample determined by a `dj_sample` local event.  |
| 19            | Table 1: Red freestyle sample zone<br>Sample determined by a `dj_sample` local event.    |
| 20            | Table 1: Blue freestyle sample zone<br>Sample determined by a `dj_sample` local event.   |
|               |                                                                                          |
| Second table  | These note types are only used in the `DJDualTurntable` track.                           |
| 32            | Table 2: Green (1st lane) tap                                                            |
| 33            | Table 2: Red (2nd lane) tap                                                              |
| 34            | Table 2: Blue (3rd lane) tap                                                             |
|               |                                                                                          |
| 35            | Table 2: Green scratch up                                                                |
| 36            | Table 2: Red scratch up                                                                  |
| 37            | Table 2: Blue scratch up                                                                 |
| 38            | Table 2: Green scratch down                                                              |
| 39            | Table 2: Red scratch down                                                                |
| 40            | Table 2: Blue scratch down                                                               |
| 41            | Table 2: Green scratch any direction                                                     |
| 42            | Table 2: Red scratch any direction                                                       |
| 43            | Table 2: Blue scratch any direction                                                      |
| 44            | Table 2: Green scratch zone                                                              |
| 45            | Table 2: Red scratch zone                                                                |
| 46            | Table 2: Blue scratch zone                                                               |
|               |                                                                                          |
| 47            | Table 2: Green freestyle scratch zone<br>Sample determined by a `dj_sample` local event. |
| 48            | Table 2: Red freestyle scratch zone<br>Sample determined by a `dj_sample` local event.   |
| 49            | Table 2: Blue freestyle scratch zone<br>Sample determined by a `dj_sample` local event.  |
|               |                                                                                          |
| 50            | Table 2: Green freestyle sample zone<br>Sample determined by a `dj_sample` local event.  |
| 51            | Table 2: Red freestyle sample zone<br>Sample determined by a `dj_sample` local event.    |
| 52            | Table 2: Blue freestyle sample zone<br>Sample determined by a `dj_sample` local event.   |
|               |                                                                                          |
| Crossfades    |                                                                                          |
| 64            | Green crossfade                                                                          |
| 65            | Center crossfade                                                                         |
| 66            | Blue crossfade                                                                           |
| 67            | Crossfade force spike/non-spike swap                                                     |
|               |                                                                                          |
| 68            | All-lane freestyle crossfade                                                             |
| 69            | Freestyle crossfade Green guideline markers                                              |
| 70            | Freestyle crossfade Blue guideline markers                                               |
|               |                                                                                          |
| Miscellaneous |                                                                                          |
| 96            | Effect dial spin left                                                                    |
| 97            | Effect dial spin right                                                                   |

Crossfades will automatically become spike crossfades if their length is a 1/16th note or less. These spikes can be forced as regular crossfades by using the spike/non-spike swap note. Regular fades can also be forced as spikes regardless of their length

#### Turntable Special Phrase Types

| Special Type  | Description                                                                           |
| :----------:  | :----------                                                                           |
| 2             | Euphoria phrase                                                                       |
|               |                                                                                       |
| First table   |                                                                                       |
| 32            | Table 1: Green effect zone<br>Effect type determined by a `dj_effect` local event.    |
| 33            | Table 1: Red effect zone<br>Effect type determined by a `dj_effect` local event.      |
| 34            | Table 1: Blue effect zone<br>Effect type determined by a `dj_effect` local event.     |
| 35            | Table 1: All-lane effect zone<br>Effect type determined by a `dj_effect` local event. |
|               |                                                                                       |
| Second table  | These note types are only used in the `DJDualTurntable` track.                        |
| 64            | Table 2: Green effect zone<br>Effect type determined by a `dj_effect` local event.    |
| 65            | Table 2: Red effect zone<br>Effect type determined by a `dj_effect` local event.      |
| 66            | Table 2: Blue effect zone<br>Effect type determined by a `dj_effect` local event.     |
| 67            | Table 2: All-lane effect zone<br>Effect type determined by a `dj_effect` local event. |
|               |                                                                                       |
| Miscellaneous |                                                                                       |
| 96            | Nullify crossfades and force center crossfade                                         |

#### Turntable Local Events

| Event Text                | Description |
| :--------:                | :---------- |
| `dj_effect <name>`        | Specifies the audio effect to be used by a corresponding effect zone.<br>`name` is the name of the effect to be used:<br>`BeatRoll`, `BeatRoll_AutoAdvance`, `BitReduction` (bitcrush), `Delay`, `Filter` (low/hi-pass), `Flanger`, `RingMod`, `Robot`, `Stutter`, `Wah` |
| `dj_sample <number> [lane]` | Specifies a sample number to be used by a corresponding tap, scratch, or freestyle zone.<br>`number` is the number of the sample to use, as specified in the `[Song]` section.<br>`lane` is an optional parameter that specifies the lane a sample should apply to, for cases where multiple lanes are in use.<br>The engine playing the chart may choose whether or not to support samples on taps and scratches (i.e. as hitsounds). |
