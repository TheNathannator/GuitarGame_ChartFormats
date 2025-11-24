# Real Keys in .mid

Real Keys was created in Phase Shift as a grander Pro Keys mode. It is played using a full MIDI keyboard and takes up two full highways, one each for the left and right hands.

## Track Names

- `PART REAL_KEYS_PS_X` - Real Keys Expert
- `PART REAL_KEYS_PS_M` - Real Keys Hard
- `PART REAL_KEYS_PS_H` - Real Keys Medium
- `PART REAL_KEYS_PS_E` - Real Keys Easy

## Track Notes

Notes/markers:

| MIDI Note | Description            |
| :-------: | :----------            |
| Markers   |                        |
| 127       | Trill marker           |
| 126       | Glissando marker       |
| 116       | Star Power marker      |
| 115       | Solo marker            |
|           |                        |
| Notes     |                        |
| 108       | C8 (Highest Note)      |
| 107       | B7                     |
| 106       | A#7                    |
| 105       | A7                     |
| 104       | G#7                    |
| 103       | G7                     |
| 102       | F#7                    |
| 101       | F7                     |
| 100       | E7                     |
| 99        | D#7                    |
| 98        | D7                     |
| 97        | C#7                    |
| 96        | C7                     |
| 95        | B6                     |
| 94        | A#6                    |
| 93        | A6                     |
| 92        | G#6                    |
| 91        | G6                     |
| 90        | F#6                    |
| 89        | F6                     |
| 88        | E6                     |
| 87        | D#6                    |
| 86        | D6                     |
| 85        | C#6                    |
| 84        | C6                     |
| 83        | B5                     |
| 82        | A#5                    |
| 81        | A5                     |
| 80        | G#5                    |
| 79        | G5                     |
| 78        | F#5                    |
| 77        | F5                     |
| 76        | E5                     |
| 75        | D#5                    |
| 74        | D5                     |
| 73        | C#5                    |
| 72        | C5                     |
| 71        | B4                     |
| 70        | A#4                    |
| 69        | A4                     |
| 68        | G#4                    |
| 67        | G4                     |
| 66        | F#4                    |
| 65        | F4                     |
| 64        | E4                     |
| 63        | D#4                    |
| 62        | D4                     |
| 61        | C#4                    |
| 60        | C4 (Middle C)          |
| 59        | B3                     |
| 58        | A#3                    |
| 57        | A3                     |
| 56        | G#3                    |
| 55        | G3                     |
| 54        | F#3                    |
| 53        | F3                     |
| 52        | E3                     |
| 51        | D#3                    |
| 50        | D3                     |
| 49        | C#3                    |
| 48        | C3                     |
| 47        | B2                     |
| 46        | A#2                    |
| 45        | A2                     |
| 44        | G#2                    |
| 43        | G2                     |
| 42        | F#2                    |
| 41        | F2                     |
| 40        | E2                     |
| 39        | D#2                    |
| 38        | D2                     |
| 37        | C#2                    |
| 36        | C2                     |
| 35        | B1                     |
| 34        | A#1                    |
| 33        | A1                     |
| 32        | G#1                    |
| 31        | G1                     |
| 30        | F#1                    |
| 29        | F1                     |
| 28        | E1                     |
| 27        | D#1                    |
| 26        | D1                     |
| 25        | C#1                    |
| 24        | C1                     |
| 23        | B0                     |
| 22        | A#0                    |
| 21        | A0 (Lowest Note)       |
|           |                        |
| Ranges    |                        |
| 13        | Right hand range shift |
| 12        | Left hand range shift  |

## Track Channels

| MIDI Channel | Description       |
| :----------: | :----------       |
| 0            | Right hand notes  |
| 1            | Left hand notes   |

## Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

- Only Star Power marked on the Expert track will work.

Trill and glissando markers are used to make glissandi and imprecise/indiscernible trills easier to play.

- Trill markers prevent overhitting and only require the player to alternate faster than a certain threshold to hit the charted notes.
- Trill markers should only be used with two-note trills.
- Glissando markers, as the Rock Band Network docs describe it, disable the scoring system for the notes between the start and end of the glissando. This is vague, but it's all that's given to go off of.
- Glissandos only work in Expert.

The range shift markers will shift the range of the keys shown on the track. The position to shift to is encoded using the note's velocity, ranging from 21 to 108.

- These markers do not set the duration of the shift, they simply mark when the shift happens.
- One of these shifts should be marked at the very beginning of the chart as the starting range.

## Lane Counts

Displaying the full 88-key range within the limitations of two Rock Band-style highways isn't very practical. Most charts also won't use the full 88-key range. As a result, Real Keys defines two song.ini tags for setting the number of lanes to display at once: `real_keys_lane_count_left` and `real_keys_lane_count_right`. The shift markers will shift this range across the track.

Maintaining a balance of minimal shifting and minimal range is important. Shifts can be disorienting, but a wide range can be difficult to read. Keeping shifting to a minimum is especially important, so it's recommended to increase the range if the number of shifts can be reduced considerably.
