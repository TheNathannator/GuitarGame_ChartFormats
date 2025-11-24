# Rock Band 5-Fret Tracks

- `PART GUITAR` - Lead Guitar
- `PART BASS` - Bass Guitar
- `PART KEYS` - 5-Lane Keys

Only the new/unsupported events and other differences are listed here, to avoid duplicating information.

## Track Notes

New notes:

| MIDI Note | Description                           |
| :-------: | :----------                           |
| Animation |                                       |
| 59        | Left hand position 20                 |
| 58        | Left hand position 19                 |
| 57        | Left hand position 18                 |
| 56        | Left hand position 17                 |
| 55        | Left hand position 16                 |
| 54        | Left hand position 15                 |
| 53        | Left hand position 14                 |
| 52        | Left hand position 13                 |
| 51        | Left hand position 12                 |
| 50        | Left hand position 11                 |
| 49        | Left hand position 10                 |
| 48        | Left hand position 9                  |
| 47        | Left hand position 8                  |
| 46        | Left hand position 7                  |
| 45        | Left hand position 6                  |
| 44        | Left hand position 5                  |
| 43        | Left hand position 4                  |
| 42        | Left hand position 3                  |
| 41        | Left hand position 2                  |
| 40        | Left hand position 1 (near headstock) |
|           |                                       |
| Unknown   |                                       |
| 15        | ? ("h2h camera cuts and focus notes") | 
| 14        | ? ("h2h camera cuts and focus notes") | 
| 13        | ? ("h2h camera cuts and focus notes") | 
| 12        | ? ("h2h camera cuts and focus notes") | 

Not supported:

| MIDI Note | Description             |
| :-------: | :----------             |
| 104       | Tap note marker         |
| 103       | GH1-2 Star Power marker |
|           |                         |
| 95        | Expert Open             |
| 83        | Hard Open               |
| 71        | Medium Open             |
| 59        | Easy Open               |

- Note 103 is only supported for solo marking, to my knowledge Rock Band won't attempt to perform a fixup and interpret it as such.

## Note Mechanics

Notes with a position difference of 10 ticks or less will be snapped together as a chord, with the position to snap to being the position of the earliest note within the chord. This includes sustains as well, with the length of the new chord sustain being the length of the *shortest* note.

When playing with a Pro Keyboard, extended sustains/disjoint chords are allowed and HOPOs are not set. When playing using a guitar, extended sustains/disjoint chords are trimmed and HOPOs are set.

## Keys Track Notes

To my knowledge, the Keys track does not have forcing, Score Duel phrases (they were phased out in RB3), the left-hand positions (Keys animations are separate tracks), or the unknown notes (these were also phased out in RB3 as far as I can tell). I have not done any testing to confirm if it responds to forcing when playing on a guitar, however.

## Text Events

Character animations:

| Event Text        | Description                                                      |
| :---------        | :----------                                                      |
| `[idle]`          | Character idles during a part with no notes.                     |
| `[idle_realtime]` | Character idles in real-time (not synced to the beat).           |
| `[idle_intense]`  | Character idles intensely.                                       |
| `[play]`          | Character starts playing.                                        |
| `[play_solo]`     | Character plays fast while showing off. (Not available for Keys) |
| `[mellow]`        | Character plays in a mellow manner.                              |
| `[intense]`       | Character plays in an intense manner.                            |

Hand maps (Guitar/Bass only):

`[map <name>]` - Specifies a hand map to use from this point onwards.

- Left hand maps:

| Name                | Description                                                                         |
| :---------          | :----------                                                                         |
| `HandMap_Default`   | Single notes use single fingers, sustains use vibrato, chords use multiple fingers. |
| `HandMap_NoChords`  | No chord fingerings.                                                                |
| `HandMap_AllChords` | Only chord fingerings.                                                              |
| `HandMap_AllBend`   | "All ring finger hi vibrato" (sic, probably means all fingerings are vibrato)       |
| `HandMap_Solo`      | D major chord fingering for all chords, vibrato for all chord sustains.             |
| `HandMap_DropD`     | Open-fretting (no fingers down) for all green gems, all other gems are chords.      |
| `HandMap_DropD2`    | Open-fretting (no fingers down) for all green gems.                                 |
| `HandMap_Chord_C`   | C chord fingering for all notes.                                                    |
| `HandMap_Chord_D`   | D chord fingering for all notes.                                                    |
| `HandMap_Chord_A`   | A minor chord fingering for all notes.                                              |

- Right hand maps (Bass only):

| Name                | Description       |
| :---                | :----------       |
| `StrumMap_Default`  | Finger strumming. |
| `StrumMap_Pick`     | Pick strumming.   |
| `StrumMap_SlapBass` | Slap strumming.   |
