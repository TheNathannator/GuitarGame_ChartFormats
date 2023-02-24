# Pro Guitar/Bass in .mid

Pro Guitar/Bass is a mode designed to be played with a controller that much more closely imitates a real guitar.

This mode has some rather complicated mechanics, so some things may have been missed. There are also some observed unknowns in some chart files documented here.

## Table of Contents

- [Track Names](#track-names)
- [Track Notes](#track-notes)
- [Track Channels](#track-channels)
- [Track SysEx Events](#track-sysex-events)
- [Note Mechanics](#note-mechanics)
- [Phrase Mechanics](#phrase-mechanics)
- [Marker Mechanics](#marker-mechanics)
- [Track Text Events](#track-text-events)

## Track Names

- `PART REAL_GUITAR` - Pro Guitar (17-Fret)
- `PART REAL_GUITAR_22` - Pro Guitar (22-Fret)
- `PART REAL_GUITAR_BONUS` - Unknown Pro Guitar track that EoF has available
- `PART REAL_BASS` - Pro Bass (17-Fret)
- `PART REAL_BASS_22` - Pro Bass (22-Fret)

There are separate tracks for 17-fret and 22-fret due to different Pro Guitar controller models having a different amount of available frets.

## Track Notes

| MIDI Note  | Description                             |
| :-------:  | :----------                             |
| Markers    |                                         |
| 127        | Trill lane marker                       |
| 126        | Tremolo lane marker                     |
| 125        | Big Rock Ending marker 1                |
| 124        | Big Rock Ending marker 2                |
| 123        | Big Rock Ending marker 3                |
| 122        | Big Rock Ending marker 4                |
| 121        | Big Rock Ending marker 5                |
| 120        | Big Rock Ending marker 6                |
| 116        | Star Power marker                       |
| 115        | Solo marker                             |
| 108        | Left hand position marker               |
| 107        | Force full chord numbering              |
|            |                                         |
| Expert     |                                         |
| 106        | Expert Unknown                          | 
| 105        | Expert string emphasis marker           |
| 104        | Expert arpeggio phrase marker           |
| 103        | Expert slide marker                     |
| 102        | Expert force HOPO marker                |
| 101        | Expert Purple (6th lane, high E string) |
| 100        | Expert Yellow (5th lane, B string)      |
| 99         | Expert Blue (4th lane, G string)        |
| 98         | Expert Orange (3rd lane, D string)      |
| 97         | Expert Green (2nd lane, A string)       |
| 96         | Expert Red (1st lane, low E string)     |
|            |                                         |
| Hard       |                                         |
| 82         | Hard Unknown                            | 
| 81         | Hard string emphasis marker             |
| 80         | Hard arpeggio phrase marker             |
| 79         | Hard slide marker                       |
| 78         | Hard force HOPO marker                  |
| 77         | Hard Purple (6th lane, high E string)   |
| 76         | Hard Yellow (5th lane, B string)        |
| 75         | Hard Blue (4th lane, G string)          |
| 74         | Hard Orange (3rd lane, D string)        |
| 73         | Hard Green (2nd lane, A string)         |
| 72         | Hard Red (1st lane, low E string)       |
|            |                                         |
| Medium     |                                         |
| 58         | Medium Unknown                          | 
| 56         | Medium arpeggio phrase marker           |
| 55         | Medium slide marker                     |
| 54         | Medium force HOPO marker                |
| 53         | Medium Purple (6th lane, high E string) |
| 52         | Medium Yellow (5th lane, B string)      |
| 51         | Medium Blue (4th lane, G string)        |
| 50         | Medium Orange (3rd lane, D string)      |
| 49         | Medium Green (2nd lane, A string)       |
| 48         | Medium Red (1st lane, low E string)     |
|            |                                         |
| Easy       |                                         |
| 34         | (Unobserved) Easy Unknown               | 
| 32         | Easy arpeggio phrase marker             |
| 31         | Easy slide marker                       |
| 30         | Easy force HOPO marker                  |
| 29         | Easy Purple (6th lane, high E string)   |
| 28         | Easy Yellow (5th lane, B string)        |
| 27         | Easy Blue (4th lane, G string)          |
| 26         | Easy Orange (3rd lane, D string)        |
| 25         | Easy Green (2nd lane, A string)         |
| 24         | Easy Red (1st lane, low E string)       |
|            |                                         |
| Root Notes |                                         |
| 18         | Sharp-flat switch                       |
| 17         | Hide chord name                         |
| 16         | Slash chord marker                      |
| 15         | Root note marker: D#/Eb                 |
| 14         | Root note marker: D                     |
| 13         | Root note marker: C#/Db                 |
| 12         | Root note marker: C                     |
| 11         | Root note marker: B                     |
| 10         | Root note marker: A#/Bb                 |
| 9          | Root note marker: A                     |
| 8          | Root note marker: G#/Ab                 |
| 7          | Root note marker: G                     |
| 6          | Root note marker: F#/Gb                 |
| 5          | Root note marker: F                     |
| 4          | Root note marker: E                     |

## Track Channels

Notes:

| MIDI Channel | Description     |
| :----------: | :----------     |
| 0            | Normal notes    |
| 1            | Ghost notes     |
| 2            | Bend notes      |
| 3            | Muted notes     |
| 4            | Tapped notes    |
| 5            | Harmonics       |
| 6            | Pinch harmonics |
<!-- | 14           | Slap notes      | TODO: where did I find this??? -->

Markers/phrases:

| MIDI Channel | Description             |
| :----------: | :----------             |
| 1            | Arpeggio chord shapes   |
| 11           | Reverse slide direction |
| 13           | String emphasis: EAD    |
| 14           | String emphasis: ADGB   |
| 15           | String emphasis: GBe    |

## Track SysEx Events

| Modifier | Description    |
| :------: | :----------    |
| `0x02`   | Slide up       |
| `0x03`   | Slide down     |
| `0x09`   | Palm mute      |
| `0x0A`   | Vibrato        |
| `0x0B`   | Harmonic       |
| `0x0C`   | Pinch harmonic |
| `0x0D`   | Bend           |
| `0x0E`   | Accent         |
| `0x0F`   | Pop            |
| `0x10`   | Slap           |

## Note Mechanics

The fret number of notes and markers that use fret numbers is determined by the MIDI note velocity, starting from velocity 100 and going to velocity 117 for 17-fret or 122 for 22-fret.

Different channels in the MIDI track are used to mark notes as different types of notes. The exact differences between the different notes aren't well-documented, this will require research.<!--TODO-->

If any note in a chord is marked as muted, the whole chord will become muted. Chords that contain single muted strings must omit the muted strings in charting.

## Phrase Mechanics

Trill and tremolo lanes are used to make imprecise/indiscernible trills or strumming easier to play. They prevent overstrumming and only require you to hit faster than a certain threshold to hit the charted notes.

- The exact mechanics for these on Pro Guitar are not fully known, but it should be safe to assume it works the same as regular guitar.
- Tremolo lanes can be used on chords.
- Trill lanes should only be used with two-note trills.
- These only apply to Expert unless they are marked at a velocity between 50-41, in which case it applies to Hard as well.

Arpeggio phrases use a corresponding note/chord on track channel 2 as a "ghost" note/chord to be displayed for the duration of the marker, indicating to the player that they can hold that note/chord for the upcoming notes.

## Marker Mechanics

The left hand position marker marks the fret position where the player's left hand should be from this point forward, and determines how fret numbers get positioned on top of the note in RB3. Fret 0 and the last 3 frets of the fret range are invalid for this marker (i.e. ranges from 101 to 114 for 17-fret, and 101 to 119 for 22-fret).

The force full chord numbering marker makes the game display all of the fret numbers for all of the notes in a chord.

The string emphasis markers use track channels to visually emphasize certain strings:

- 13: High string emphasis (GBe)
- 14: Middle string emphasis (DG)
- 15: Low string emphasis (EAD)

| MIDI Channel | Description                 |
| :----------: | :----------                 |
| 13           | High string emphasis (GBe)  |
| 14           | Middle string emphasis (DG) |
| 15           | Low string emphasis (EAD)   |

The slide marker turns sustain notes into slide notes. The direction of the slide is determined through these rules:

- If the sustain ends within a 1/16th of a following note, the slide will follow the direction of the next note's fret number: lower to higher will slide up, higher to lower will slide down. If both are the same, it will slide down.
- Otherwise, if the fret number for the corresponding note is 0-7, the slide will go up, else if it's 8 or higher it will go down.
- For chords, the lowest non-open (fret number of 0) string's fret number determines the direction.
- The direction of the slide can be reversed by placing the slide marker on channel 11.

The root note markers mark the root note of chords. They are required for chords, but have a running status: a new root note marker only needs to be placed when the root note changes.

The sharp-flat switch marker switches whether the chord displays as sharp or flat.

The hide chord name marker hides the chord name that would normally be displayed for a chord.

The slash chord marker is used for displaying slash chords such as `Fm7/E` or `Am/G`. It excludes the lowest string when calculating the name for before the slash, and uses it only for the note after the slash.

## Track Text Events

Chord names:

| Event Text                  | Description |
| :---------                  | :---------- |
| `[chrd<difficulty> <name>]` | Overrides the chord name shown for the current chord.<br>`difficulty` is a number representing the difficulty: 0 = Easy, 3 = Expert, etc.<br>`name` is the name to use for the chord. A `<gtr></gtr>` tag pair is used for superscript, so you can use, for example, `C<gtr>7</gtr>` to display C⁷.
