# Rock Band Drums Track

- `PART DRUMS` - Drums
- `PART DRUMS_2X` - RBN 2x Kick drums chart
  - This track isn't actually supported by Rock Band, it's used for more easily creating 1x kick and 2x kick versions of a song since Rock Band doesn't have proper 2x kick support.

Only the new/unsupported events and other differences are listed here, to avoid duplicating information.

## MIDI Notes

New notes:

| MIDI Note | Description                  |
| :-------: | :----------                  |
| Animation |                              |
| 51        | Floor tom, right hand        |
| 50        | Floor tom, left hand         |
| 49        | Tom 2, right hand            |
| 48        | Tom 2, left hand             |
| 47        | Tom 1, right hand            |
| 46        | Tom 1, left hand             |
| 45        | Crash 2, left hand, soft     |
| 44        | Crash 2, left hand, hard     |
| 43        | Ride cymbal, left hand       |
| 42        | Ride cymbal, right hand      |
| 41        | Crash 2, choke               |
| 40        | Crash 1, choke               |
| 39        | Crash 2, right hand, soft    |
| 38        | Crash 2, right hand, hard    |
| 37        | Crash 1, right hand, soft    |
| 36        | Crash 1, right hand, hard    |
| 35        | Crash 1, left hand, soft     |
| 34        | Crash 1, left hand, hard     |
| 32        | Other percussion, right hand |
| 31        | Hi-hat, right hand           |
| 30        | Hi-hat, left hand            |
| 29        | Snare, right hand, soft      |
| 28        | Snare, left hand, soft       |
| 27        | Snare, right hand, hard      |
| 26        | Snare, left hand, hard       |
| 25        | Hi-hat, open                 |
| 24        | Kick, right foot             |

Not supported:

| MIDI Note | Description                  |
| :-------: | :----------                  |
| 109       | Flam marker                  |
|           |                              |
| 101       | Expert 5-lane Green          |
| 95        | Expert+ Kick                 |
| 89        | Hard 5-lane Green            |
| 77        | Medium 5-lane Green          |
| 65        | Easy 5-lane Green            |

## Text Events

Character animations:

| Event Text             | Description                                                                           |
| :---------             | :----------                                                                           |
| `[idle]`               | Character idles during a part with no notes                                           |
| `[idle_realtime]`      | Character idles in real-time (not synced to the beat).                                |
| `[idle_intense]`       | Character idles intensely.                                                            |
| `[play]`               | Character starts playing.                                                             |
| `[mellow]`             | Character plays in a mellow manner.                                                   |
| `[intense]`            | Character plays in an intense manner.                                                 |
| `[ride_side_<enable>]` | Character uses a side-swipe to hit the ride.<br>`enable` is either `true` or `false`. |

Not supported:

| Event Text                | Description                         |
| :---------                | :----------                         |
| `[ENABLE_CHART_DYNAMICS]` | Enables accent/ghost marking.       |

### Phrase Mechanics

Roll lanes:

- Starting a roll on a chord is not supported in Rock Band.

Fills:

- Rock Band 1 and 2 only have freestyle phrases. Rock Band 3 defaults to freestyle but has an option for static phrases.
