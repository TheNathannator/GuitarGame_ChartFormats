# Rock Band Pro Keys

- `PART REAL_KEYS_X` - Pro Keys Expert
- `PART REAL_KEYS_H` - Pro Keys Hard
- `PART REAL_KEYS_M` - Pro Keys Medium
- `PART REAL_KEYS_E` - Pro Keys Easy

- `PART KEYS_ANIM_LH` - Pro Keys left-hand animations
- `PART KEYS_ANIM_RH` - Pro Keys right-hand animations

Only the new/unsupported events and other differences are listed here, to avoid duplicating information.

## Text Events

Character animation text events are inherited from the `PART KEYS` track, they are not placed on any of the Pro Keys tracks.

Song trainers:

- Not usable in RBN2/custom charts, only in custom Pro upgrades

| Event Text                              | Description                                                  |
| :---------                              | :----------                                                  |
| `[begin_key song_trainer_key_<number>]` | Marks the start of a Pro Keys trainer section.<br>`number` is an index for the trainer, starting at 1. |
| `[end_key song_trainer_key_<number>]`   | Marks the end of a Pro Keys trainer section.                 |
| `[key_norm song_trainer_key_<number>]`  | Marks a seamless loop point for a Pro Keys trainer section.  |

## Hand Animation Track Notes

| MIDI Note | Description       |
| :-------: | :----------       |
| 72        | C3 (Highest note) |
| 71        | B2                |
| 70        | A#2/Bb2           |
| 69        | A2                |
| 68        | G#2/Ab2           |
| 67        | G2                |
| 66        | F#2/Gb2           |
| 65        | F2                |
| 64        | E2                |
| 63        | D#2/Eb2           |
| 62        | D2                |
| 61        | C#2/Db2           |
| 60        | C2                |
| 59        | B1                |
| 58        | A#1/Bb1           |
| 57        | A1                |
| 56        | G#1/Ab1           |
| 55        | G1                |
| 54        | F#1/Gb1           |
| 53        | F1                |
| 52        | E1                |
| 51        | D#1/Eb1           |
| 50        | D1                |
| 49        | C#1/Db1           |
| 48        | C1 (Lowest note)  |

The character only gets 3 octaves on the keyboard, so the middle octave is shared between the left and right hand:

```
LLLLLLLLLLLLLLLLLLLLLLLLL
            RRRRRRRRRRRRRRRRRRRRRRRRR
```
