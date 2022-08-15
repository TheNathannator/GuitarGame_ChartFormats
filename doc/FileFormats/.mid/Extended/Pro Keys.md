# Pro Keys in .mid

These tracks are for Rock Band 3's Pro Keys mode. It was designed to simulate a real keyboard, and is played with a 2-octave keyboard peripheral or any MIDI keyboard.

## Table of Contents

- [Track Names](#track-names)
- [Track Notes](#track-notes)
- [Note Mechanics](#note-mechanics)
- [Phrase Mechanics](#phrase-mechanics)

## Track Names

- `PART REAL_KEYS_X` - Pro Keys Expert
- `PART REAL_KEYS_H` - Pro Keys Hard
- `PART REAL_KEYS_M` - Pro Keys Medium
- `PART REAL_KEYS_E` - Pro Keys Easy

## Track Notes

| MIDI Note | Description            |
| :-------: | :----------            |
| Markers   |                        |
| 127       | Trill marker           |
| 126       | Glissando marker       |
| 120       | Big Rock Ending marker |
| 116       | Star Power marker      |
| 115       | Solo marker            |
|           |                        |
| Notes     |                        |
| 72        | C3 (Highest note)      |
| 71        | B2                     |
| 70        | A#2/Bb2                |
| 69        | A2                     |
| 68        | G#2/Ab2                |
| 67        | G2                     |
| 66        | F#2/Gb2                |
| 65        | F2                     |
| 64        | E2                     |
| 63        | D#2/Eb2                |
| 62        | D2                     |
| 61        | C#2/Db2                |
| 60        | C2                     |
| 59        | B1                     |
| 58        | A#1/Bb1                |
| 57        | A1                     |
| 56        | G#1/Ab1                |
| 55        | G1                     |
| 54        | F#1/Gb1                |
| 53        | F1                     |
| 52        | E1                     |
| 51        | D#1/Eb1                |
| 50        | D1                     |
| 49        | C#1/Db1                |
| 48        | C1 (Lowest note)       |
|           |                        |
| Ranges    |                        |
| 9         | Range shift: A1-C3     |
| 7         | Range shift: G1-B2     |
| 5         | Range shift: F1-A2     |
| 4         | Range shift: E1-G2     |
| 2         | Range shift: D1-F2     |
| 0         | Range shift: C1-E2     |

## Note Mechanics

Up to 4 notes are allowed at a time, and extended sustains/disjoint chords are allowed.

## Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

- Only Star Power marked on the Expert track will work.

Trill and glissando markers are used to make glissandi and imprecise/indiscernible trills easier to play.

- Trill markers prevent overhitting and only require the player to alternate faster than a certain threshold to hit the charted notes.
- Trill markers should only be used with two-note trills.
- Glissando markers, as the Rock Band Network docs describe it, disable the scoring system for the notes between the start and end of the glissando. This is vague, but it's all that's given to go off of.
- Glissandos only work in Expert.

The range shift markers will shift the range of the keys shown on the track. In RB3, 17 keys out of the 25 keys are shown at a time, so if there are notes that go beyond the current range, the range must be shifted to show those notes.

- These markers do not set the duration of the shift, they simply mark when the shift happens.
- One of these shifts should be marked at the very beginning of the chart as the starting range.
