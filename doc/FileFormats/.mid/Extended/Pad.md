# Pad in .mid

This track is for gamepad modes specifically designed for controller. Debutting in Fortnite Festival and being usable in Encore.

So far the only implementations of this is in Encore, and will only have documentation on how it works in Encore.
Because of Encore supporting json and INI formats of songs, some things will specify which format it's in.
Encore is actively being developed and as such some of the things listed here may not apply but could be added or is planned to be added.

## Table of Contents

- [Track Names](#track-names)
- [Track Notes](#track-notes)
  - [Note Mechanics](#note-mechanics)
  - [Phrase Mechanics](#phrase-mechanics)

## Track Names
- `PART_GUITAR` - Pad Lead (.json)
- `PAD_GUITAR` - Pad Lead (.ini)
- `PART_BASS` - Pad Bass (.json)
- `PAD_BASS` - Pad Bass (.ini)
- `PART_DRUMS` - Pad Drums (.json)
- `PAD_DRUMS` - Pad Drums (.ini)
- `PART_VOCALS` - Pad Vocals (.json)
- `PAD_VOCALS` - Pad Vocals (.ini)
- `PART_KEYS` - Pad Keys (.json)
- `PAD_KEYS` - Pad Keys (.ini)

## Track Notes

| MIDI Note | Description                         |
| :-------: | :----------                         |
| Markers   |                                     |
| 127       | Trill lane marker                   |
| 126       | Tremolo lane marker                 |
| 124       | Big Rock Ending marker 1            |
| 123       | Big Rock Ending marker 2            |
| 122       | Big Rock Ending marker 3            |
| 121       | Big Rock Ending marker 4            |
| 120       | Big Rock Ending marker 5            |
| 116       | Overdrive marker                    |
| 108       | Solo marker                         |
| 101       | Alternative Solo marker             |
|           |                                     |
| Expert    |                                     |
| 106       | Expert Lane 5 Lift marker           |
| 105       | Expert Lane 4 Lift marker           |
| 104       | Expert Lane 3 Lift marker           |
| 103       | Expert Lane 2 Lift marker           |
| 102       | Expert Lane 1 Lift marker           |
|           |                                     |
| 100       | Expert Lane 5                       |
| 99        | Expert Lane 4                       |
| 98        | Expert Lane 3                       |
| 97        | Expert Lane 2                       |
| 96        | Expert Lane 1                       |
|           |                                     |
| Hard      |                                     |
| 94        | Hard Lane 5 Lift marker             |
| 93        | Hard Lane 4 Lift marker             |
| 92        | Hard Lane 3 Lift marker             |
| 91        | Hard Lane 2 Lift marker             |
| 90        | Hard Lane 1 Lift marker             |
|           |                                     |
| 88        | Hard Lane 5                         |
| 87        | Hard Lane 4                         |
| 86        | Hard Lane 3                         |
| 85        | Hard Lane 2                         |
| 84        | Hard Lane 1                         |
|           |                                     |
| Medium    |                                     |
| 82        | Medium Lane 5 Lift marker           |
| 81        | Medium Lane 4 Lift marker           |
| 80        | Medium Lane 3 Lift marker           |
| 79        | Medium Lane 2 Lift marker           |
| 78        | Medium Lane 1 Lift marker           |
|           |                                     |
| 76        | Medium Lane 5                       |
| 75        | Medium Lane 4                       |
| 74        | Medium Lane 3                       |
| 73        | Medium Lane 2                       |
| 72        | Medium Lane 1                       |
|           |                                     |
| Easy      |                                     |
| 70        | Easy Lane 5 Lift marker             |
| 69        | Easy Lane 4 Lift marker             |
| 68        | Easy Lane 3 Lift marker             |
| 67        | Easy Lane 2 Lift marker             |
| 66        | Easy Lane 1 Lift marker             |
|           |                                     |
| 64        | Easy Lane 5                         |
| 63        | Easy Lane 4                         |
| 62        | Easy Lane 3                         |
| 61        | Easy Lane 2                         |
| 60        | Easy Lane 1                         |

### Note Mechanics

A Lift marker will turn a note into a lift note, allowing the player to lift a input when the lift note crosses the strikeline to hit it. They are typically used for fast jacks or anything that would be extremely hard or uncomfortable on gamepad.

Sustains shorter than a 1/12th step are cut off and turned into a normal (non-sustain) note. This allows charters using a DAW to not have to make their notes 1 tick long in order for it to not be a sustain.

Chords may have individual notes with different lengths. These are referred to as extended sustains (start at different times) and disjoint chords (start at the same time, end at different times).

Lifts may occur at the same time as a regular note. These are often referend to as split chords, as they require the play to lift one note and hit the other.

Pressing the Overdrive button will hit any notes for the player, including chords and will also hold down sustains for their entire duration.

### Phrase Mechanics

Overdrive phrases mark sections of the chart where the player may gain Overdrive. When Overdrive is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

Solo markers designate sections of the song where a part in the song is playing a solo. These sections display a percentage of notes hit, and award bonus points at the end of the solo based on the percentage of notes hit.

Trill and tremolo lanes are used to make imprecise/indiscernible trills or jacks easier to play. They prevent striking and only require you to hit faster than a certain threshold to hit the charted notes.

- Tremolo lanes can be used on chords.
- Trill lanes should only be used with two-note trills.
- These only apply to Expert unless they are marked at a velocity between 50-41, in which case it applies to Hard as well.

Big Rock Endings (BREs) are freestyle phrases at the end of songs with so-called "big rock endings" where the band rocks out for a period of time before playing their final notes. These freestyle phrases let players gain bonus points by playing whatever they want during the phrase. Following the phrase is at least one note, which the player must hit in order to earn those bonus points. If they miss or strike, they lose the points.

- BREs are started by placing a `[coda]` text event on the `EVENTS` track. Then, the duration of the BRE markers determines how long the freestyle section will last. All 5 BRE notes must be used to mark the phrase.
- After the freestyle phrase, there must be at least one note for the player to play. If no notes are present, the player cannot get their score.
- In general, notes should still be charted regardless because not every game supports BREs.
