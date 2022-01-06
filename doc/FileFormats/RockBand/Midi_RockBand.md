# Rock Band MIDI Files

This document details the format of Rock Band .mid charts. See [Midi.md](../_General/Midi.md) for a more general overview of .mid charts.

I do not currently have access to the original files of the game to research them myself, instead I'm referencing the RBN/C3 docs and a couple C3/RGW posts. Take the information here with a grain of salt, and maybe consider adding your own corrections if you see something you know is wrong or missing.

## Table of Contents

- [Track Names](#track-names)
  - [5-Fret Tracks](#5-fret-tracks)
    - [5-Fret Notes](#5-fret-notes)
    - [5-Lane Keys Notes](#5-lane-keys-notes)
    - [5-Fret Text Events](#5-fret-text-events)
  - [Drums Track](#drums-track)
    - [Drums Notes](#drums-notes)
    - [Drums Text Events](#drums-text-events)
  - [Vocals Tracks](#vocals-tracks)
    - [Vocals Notes](#vocals-notes)
    - [Vocals Text Events](#vocals-text-events)
    - [Vocals Lyrics](#vocals-lyrics)
  - [Pro Keys Tracks](#pro-keys-tracks)
    - [Pro Keys Notes](#pro-keys-notes)
    - [Pro Keys Animation Track Notes](#pro-keys-animation-track-notes)
  - [Pro Guitar/Bass Tracks](#pro-guitarbass-tracks)
    - [Pro Guitar/Bass Notes and Channels](#pro-guitarbass-notes-and-channels)
    - [Pro Guitar/Bass Text Events](#pro-guitarbass-text-events)
  - [Events Track](#events-track)
    - [Events Notes](#events-notes)
    - [Events Common Text Events](#events-common-text-events)
  - [Venue Track](#venue-track)
    - [Venue Notes (RB1/RB2/RBN1)](#venue-notes-rb1rb2rbn1)
    - [Venue Notes (RB3/RBN2)](#venue-notes-rb3rbn2)
    - [Venue Text Events (RB1/RB2/RBN1)](#venue-text-events-rb1rb2rbn1)
      - [Directed Cuts (RB1/RB2/RBN1)](#directed-cuts-rb1rb2rbn1)
      - [Venue Lighting (RB1/RB2/RBN1)](#venue-lighting-rb1rb2rbn1)
      - [Other (RB1/RB2/RBN1)](#other-rb1rb2rbn1)
    - [Venue Text Events (RB3/RBN2)](#venue-text-events-rb3rbn2)
    - [Camera Cuts (RB3/RBN2)](#camera-cuts-rb3rbn2)
    - [Directed Camera Cuts (RB3/RBN2)](#directed-camera-cuts-rb3rbn2)
    - [Post-Processing Effects (RB3/RBN2)](#post-processing-effects-rb3rbn2)
    - [Venue Lighting (RB3/RBN2)](#venue-lighting-rb3rbn2)
    - [Other (RB3/RBN2)](#other-rb3rbn2)
  - [Beat Track](#beat-track)
    - [Beat Notes](#beat-notes)
- [References](#references)

## Track Names

| Track Name                                             | Track Description              |
| :---------                                             | :----------------              |
| [`PART GUITAR`](#5-fret-tracks)                        | Lead Guitar                    |
| [`PART BASS`](#5-fret-tracks)                          | Bass Guitar                    |
| [`PART KEYS`](#5-lane-keys-notes)                      | 5-Lane Keys                    |
| [`PART DRUMS`](#drums-track)                           | Drums/Pro Drums                |
| [`PART DRUMS_2X`](#drums-track)                        | RBN 2x Kick Drums chart<br>Meant for generating separate 1x and 2x kick files for Rock Band. Probably won't ever be seen in a publicly-available chart, but worth noting. |
| [`PART VOCALS`](#vocals-tracks)                        | Vocals                         |
| [`HARM1`](#vocals-tracks)                              | Harmonies part 1               |
| [`HARM2`](#vocals-tracks)                              | Harmonies part 2               |
| [`HARM3`](#vocals-tracks)                              | Harmonies part 3               |
| [`PART REAL_GUITAR`](#pro-guitarbass-tracks)           | Pro Guitar (17-fret)           |
| [`PART REAL_GUITAR_22`](#pro-guitarbass-tracks)        | Pro Guitar (22-fret)           |
| [`PART REAL_BASS`](#pro-guitarbass-tracks)             | Pro Bass (17-fret)             |
| [`PART REAL_BASS_22`](#pro-guitarbass-tracks)          | Pro Bass (22-fret)             |
| [`PART REAL_KEYS_X`](#pro-keys-tracks)                 | Pro Keys Expert                |
| [`PART REAL_KEYS_H`](#pro-keys-tracks)                 | Pro Keys Hard                  |
| [`PART REAL_KEYS_M`](#pro-keys-tracks)                 | Pro Keys Medium                |
| [`PART REAL_KEYS_E`](#pro-keys-tracks)                 | Pro Keys Easy                  |
| [`PART KEYS_ANIM_LH`](#pro-keys-animation-track-notes) | Pro Keys left-hand animations  |
| [`PART KEYS_ANIM_RH`](#pro-keys-animation-track-notes) | Pro Keys right-hand animations |
| [`EVENTS`](#events-track)                              | Global events                  |
| [`VENUE`](#venue-track)                                | Venue track                    |
| [`BEAT`](#beat-track)                                  | Upbeat/downbeat track          |

### 5-Fret Tracks

- `PART GUITAR` - Lead Guitar
- `PART BASS` - Bass Guitar
- `PART KEYS` - 5-Lane Keys

#### 5-Fret Notes

| MIDI Note | Description                                                                                                   |
| :-------: | :----------                                                                                                   |
| Markers   |                                                                                                               |
| 127       | Trill lane marker<br>Only applies to Expert unless velocity is 50-41, then it will show up on Hard as well.   |
| 126       | Tremolo lane marker<br>Only applies to Expert unless velocity is 50-41, then it will show up on Hard as well. |
| 124       | Big Rock Ending marker 1                                                                                      |
| 123       | Big Rock Ending marker 2                                                                                      |
| 122       | Big Rock Ending marker 3                                                                                      |
| 121       | Big Rock Ending marker 4                                                                                      |
| 120       | Big Rock Ending marker 5<br>All 5 must be used as a chord along with a `[coda]` event on the EVENTS track at the start of the chord to initiate a Big Rock Ending. |
| 116       | Star Power/Overdrive marker                                                                                   |
| 106       | Score Duel(?) player 2 phrase<br>Can overlap with the Player 1 marker.                                        |
| 105       | Score Duel(?) player 1 phrase<br>Can overlap with the Player 2 marker.                                        |
| 103       | Solo marker                                                                                                   |
|           |                                                                                                               |
| Expert    |                                                                                                               |
| 102       | Expert force strum                                                                                            |
| 101       | Expert force HOPO                                                                                             |
| 100       | Expert Orange (5th lane)                                                                                      |
| 99        | Expert Blue (4th lane)                                                                                        |
| 98        | Expert Yellow (3rd lane)                                                                                      |
| 97        | Expert Red (2nd lane)                                                                                         |
| 96        | Expert Green (1st lane)                                                                                       |
|           |                                                                                                               |
| Hard      |                                                                                                               |
| 90        | Hard force strum                                                                                              |
| 89        | Hard force HOPO                                                                                               |
| 88        | Hard Orange (5th lane)                                                                                        |
| 87        | Hard Blue (4th lane)                                                                                          |
| 86        | Hard Yellow (3rd lane)                                                                                        |
| 85        | Hard Red (2nd lane)                                                                                           |
| 84        | Hard Green (1st lane)                                                                                         |
|           |                                                                                                               |
| Medium    |                                                                                                               |
| 78        | Medium force strum                                                                                            |
| 77        | Medium force HOPO                                                                                             |
| 76        | Medium Orange (5th lane)                                                                                      |
| 75        | Medium Blue (4th lane)                                                                                        |
| 74        | Medium Yellow (3rd lane)                                                                                      |
| 73        | Medium Red (2nd lane)                                                                                         |
| 72        | Medium Green (1st lane)                                                                                       |
|           |                                                                                                               |
| Easy      |                                                                                                               |
| 66        | Easy force strum                                                                                              |
| 65        | Easy force HOPO                                                                                               |
| 64        | Easy Orange (5th lane)                                                                                        |
| 63        | Easy Blue (4th lane)                                                                                          |
| 62        | Easy Yellow (3rd lane)                                                                                        |
| 61        | Easy Red (2nd lane)                                                                                           |
| 60        | Easy Green (1st lane)                                                                                         |
|           |                                                                                                               |
| Animation |                                                                                                               |
| 59        | Left hand position 20                                                                                         |
| 58        | Left hand position 19                                                                                         |
| 57        | Left hand position 18                                                                                         |
| 56        | Left hand position 17                                                                                         |
| 55        | Left hand position 16                                                                                         |
| 54        | Left hand position 15                                                                                         |
| 53        | Left hand position 14                                                                                         |
| 52        | Left hand position 13                                                                                         |
| 51        | Left hand position 12                                                                                         |
| 50        | Left hand position 11                                                                                         |
| 49        | Left hand position 10                                                                                         |
| 48        | Left hand position 9                                                                                          |
| 47        | Left hand position 8                                                                                          |
| 46        | Left hand position 7                                                                                          |
| 45        | Left hand position 6                                                                                          |
| 44        | Left hand position 5                                                                                          |
| 43        | Left hand position 4                                                                                          |
| 42        | Left hand position 3                                                                                          |
| 41        | Left hand position 2                                                                                          |
| 40        | Left hand position 1 (near headstock)                                                                         |
|           |                                                                                                               |
| Unknown   |                                                                                                               |
| 15        |                                                                                                               |
| 14        |                                                                                                               |
| 13        |                                                                                                               |
| 12        |                                                                                                               |

- Notes get set as HOPOs (hammer-ons/pull-offs) automatically if they are within 160 ticks of the previous note, unless they are the same lane as the previous note, or are a chord.
  - Notes can be forced as either strums or HOPOs using the Force Strum and Force HOPO markers. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.
- Notes shorter than 160 ticks step will be cut off and turned into a normal note.
- Trill and tremolo lanes are used to make imprecise/indiscernible trills or fast strumming easier to play. They prevent overstrumming and only require you to hit faster than a certain threshold to hit the charted notes.
- Notes with a position difference of 10 ticks or less will be snapped together as a chord, with the position to snap to being the position of the earliest note within the chord. This includes sustains as well.
- Extended sustains/disjoint chords are not allowed except for on Keys.
- Keys track:
  - The Keys track does not have tremolo lanes, forcing, Score Duel phrases (they were phased out in RB3), the left-hand positions (Keys animations are separate tracks), or the unknown notes at the bottom.
  - When playing with a Pro Keyboard, extended sustains/disjoint chords are allowed and HOPOs are not set. When playing using a guitar, extended sustains/disjoint chords are trimmed and HOPOs are set.

#### 5-Fret Text Events

Character animations:

| Event Text         | Description                                                         |
| :---------         | :----------                                                         |
| `[idle]`           | Character idles during a part with no notes.                        |
| `[idle_realtime]`  | Character idles in real-time (not synced to the beat).              |
| `[idle_intense]`   | Character idles intensely.                                          |
| `[play]`           | Character starts playing.                                           |
| `[play_solo]`      | Character plays fast while showing off.<br>(Not available for Keys) |
| `[mellow]`         | Character plays in a mellow manner.                                 |
| `[intense]`        | Character plays in an intense manner.                               |

Hand maps (Guitar/Bass only):

`[map <name>]` - Specifies a hand map to use from this point onwards. See the following 2 tables for known hand maps.

- Left-hand maps:

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

- Right-hand maps (Bass only):

| Name                | Description       |
| :---                | :----------       |
| `StrumMap_Default`  | Finger strumming. |
| `StrumMap_Pick`     | Pick strumming.   |
| `StrumMap_SlapBass` | Slap strumming.   |

### Drums Track

- `PART DRUMS` - Standard 4-Lane, 4-Lane Pro, and 5-Lane Drums
- `PART DRUMS_2X` - RBN 2x Kick drums chart

#### Drums Notes

| MIDI Note | Description                                                                                             |
| :-------: | :----------                                                                                             |
| Markers   |                                                                                                         |
| 127       | 2-lane roll marker<br>Only applies to Expert unless velocity is 50-41, then it will be on Hard as well. |
| 126       | 1-lane roll marker<br>Only applies to Expert unless velocity is 50-41, then it will be on Hard as well. |
| 124       | Big Rock Ending/Fill marker 1                                                                           |
| 123       | Big Rock Ending/Fill marker 2                                                                           |
| 122       | Big Rock Ending/Fill marker 3                                                                           |
| 121       | Big Rock Ending/Fill marker 4                                                                           |
| 120       | Big Rock Ending/Fill marker 5<br>All 5 must be used as a chord to mark an SP activation fill; for a Big Rock Ending, there should be a `[coda]` event on the EVENTS track at the start of this chord. |
| 116       | Star Power/Overdrive marker                                                                             |
| 112       | Green tom marker                                                                                        |
| 111       | Blue tom marker                                                                                         |
| 110       | Yellow tom marker                                                                                       |
| 106       | Score Duel(?) player 2 phrase<br>Can overlap with the Player 1 marker.                                  |
| 105       | Score Duel(?) player 1 phrase<br>Can overlap with the Player 2 marker.                                  |
| 103       | Solo marker                                                                                             |
|           |                                                                                                         |
| Expert    |                                                                                                         |
| 100       | Expert Green (4th lane)                                                                                 |
| 99        | Expert Blue (3rd lane)                                                                                  |
| 98        | Expert Yellow (2nd lane)                                                                                |
| 97        | Expert Red (1st lane)                                                                                   |
| 96        | Expert Kick                                                                                             |
|           |                                                                                                         |
| Hard      |                                                                                                         |
| 88        | Hard Green (4th lane)                                                                                   |
| 87        | Hard Blue (3rd lane)                                                                                    |
| 86        | Hard Yellow (2nd lane)                                                                                  |
| 85        | Hard Red (1st lane)                                                                                     |
| 84        | Hard Kick                                                                                               |
|           |                                                                                                         |
| Medium    |                                                                                                         |
| 76        | Medium Green (4th lane)                                                                                 |
| 75        | Medium Blue (3rd lane)                                                                                  |
| 74        | Medium Yellow (2nd lane)                                                                                |
| 73        | Medium Red (1st lane)                                                                                   |
| 72        | Medium Kick                                                                                             |
|           |                                                                                                         |
| Easy      |                                                                                                         |
| 64        | Easy Green (4th lane)                                                                                   |
| 63        | Easy Blue (3rd lane)                                                                                    |
| 62        | Easy Yellow (2nd lane)                                                                                  |
| 61        | Easy Red (1st lane)                                                                                     |
| 60        | Easy Kick                                                                                               |
|           |                                                                                                         |
| Animation |                                                                                                         |
| 51        | Floor tom, right hand                                                                                   |
| 50        | Floor tom, left hand                                                                                    |
| 49        | Tom 2, right hand                                                                                       |
| 48        | Tom 2, left hand                                                                                        |
| 47        | Tom 1, right hand                                                                                       |
| 46        | Tom 1, left hand                                                                                        |
| 45        | Crash 2, soft, left hand                                                                                |
| 44        | Crash 2, hard, left hand                                                                                |
| 43        | Ride cymbal, left hand                                                                                  |
| 42        | Ride cymbal, right hand                                                                                 |
| 41        | Crash 2, choke                                                                                          |
| 40        | Crash 1, choke                                                                                          |
| 39        | Crash 2, soft, right hand                                                                               |
| 38        | Crash 2, hard, right hand                                                                               |
| 37        | Crash 1, soft, right hand                                                                               |
| 36        | Crash 1, hard, right hand                                                                               |
| 35        | Crash 1, soft, left hand                                                                                |
| 34        | Crash 1, hard, left hand                                                                                |
| 32        | Other percussion, right hand                                                                            |
| 31        | Hi-hat, right hand                                                                                      |
| 30        | Hi-hat, left hand                                                                                       |
| 29        | Snare, soft, right hand                                                                                 |
| 28        | Snare, soft, left hand                                                                                  |
| 27        | Snare, hard, right hand                                                                                 |
| 26        | Snare, hard, left hand                                                                                  |
| 25        | Hi-hat, open                                                                                            |
| 24        | Kick, right foot                                                                                        |

Additional info:

- Notes are cymbals by default. Toms are marked using notes 110-112, excluding red which is always a tom note.
  - This makes it impossible to have a tom and cymbal of the same color at the same position.
- Roll lanes are used to make imprecise/indiscernible fast rhythms such as drum rolls or cymbal swells easier to play. They prevent overhitting and only require you to hit faster than a certain threshold to hit the charted notes.
  - Rock Band's chart compiler known as Magma does not allow roll lanes to be started on chords (not including kicks).
  - Roll lanes cannot be applied to kicks.

#### Drums Text Events

Mix events:

`[mix <difficulty> drums<configuration><flag>]` - Sets a stem configuration for drums.

- `difficulty` is a number indicating the difficulty this event applies to:

| Text | Description |
| :--: | :---------- |
| `0`  | Easy        |
| `1`  | Medium      |
| `2`  | Hard        |
| `3`  | Expert      |

- `configuration` is a number corresponding to a specific stem configuration:

| Text | Description                            |
| :--: | :----------                            |
| `0`  | One stereo stem for the entire kit.    |
| `1`  | Mono kick, mono snare, stereo other.   |
| `2`  | Mono kick, stereo snare, stereo other. |
| `3`  | Stereo kick, snare, and other.         |
| `4`  | Mono kick, stereo other.               |

- `flag` is an optional string added to the end of the event that applies a modification outside of setting up the mix:

| Text         | Description |
| :--:         | :---------- |
| `d`          | Known as Disco Flip.<br>- On non-Pro, swaps the the snare and other/cymbal+tom stems, such that snare is activated by yellow, and other/cymbal+tom are activated by red.<br>- On Pro Drums, swaps yellow cymbal/yellow tom and red notes to restore proper playing of the part with cymbals instead of swapping the stems.<br>- This flag is used in sections that require alternating the left and right hands quickly on the hi-hat (commonly a disco beat, hence the name), which are typically charted swapped, since playing it un-swapped in non-Pro on stock Rock Band kits tends to be uncomfortable. |
| `dnoflip`    | Swaps the snare and kit/cymbal and tom stems on both non-Pro and Pro Drums, without swapping red and yellow in Pro Drums.<br>- Used in sections where notes should not be flipped in Pro Drums, but the snare and kit/cymbal stems should still be swapped.
| `easy`       | Unmutes the tom and cymbal gems on Easy.<br>- Used in sections where there are no tom or cymbal gems to unmute the kit stem or tom/cymbal stems. Not supported in RB3, it handles this automatically. |
| `easynokick` | Unmutes the other/cymbal+tom stem on Easy.<br>- Used in sections where there are no kick gems to unmute the kick stem. Not supported in RB3, it handles this automatically. |

Character animations:

| Event Text             | Description                                                                        |
| :---------             | :----------                                                                        |
| `[idle]`               | Character idles during a part with no notes                                        |
| `[idle_realtime]`      | Character idles in real-time (not synced to the beat).                             |
| `[idle_intense]`       | Character idles intensely.                                                         |
| `[play]`               | Character starts playing.                                                          |
| `[mellow]`             | Character plays in a mellow manner.                                                |
| `[intense]`            | Character plays in an intense manner.                                              |
| `[ride_side_<enable>]` | Character uses a side-swipe to hit the ride. `enable` is either `true` or `false`. |

### Vocals Tracks

- `PART VOCALS` - Standard vocals track
- `HARM1` - Harmonies track 1
- `HARM2` - Harmonies track 2
- `HARM3` - Harmonies track 3

#### Vocals Notes

| MIDI Note  | Description                                                                                                         |
| :-------:  | :----------                                                                                                         |
| Markers    |                                                                                                                     |
| 116        | Star Power/Overdrive marker<br>Standard vocals and Harmonies can have independent overdrive. `HARM2` and `HARM3` get their overdrive from `HARM1`. |
| 106        | Lyrics phrase marker 2/Score Duel player 2 phrase<br>Can appear with the other phrase marker at the same time (same start and end). |
| 105        | Lyrics phrase marker 1/Score Duel player 1 phrase<br>Can appear with the other phrase marker at the same time (same start and end).<br>For harmonies, the `HARM1` phrase is used for all 3 harmony tracks. The `HARM2` phrase is only used for when harmony 2/3 lyrics shift in static vocals. `HARM3` typically has no phrases marked. |
|            |                                                                                                                     |
| Percussion |                                                                                                                     |
| 97         | Not displayed percussion                                                                                            |
| 96         | Displayed percussion                                                                                                |
|            |                                                                                                                     |
| Notes      |                                                                                                                     |
| 84         | C6 (Highest)<br>Doesn't work properly in Rock Band Blitz.                                                           |
| 83         | B5                                                                                                                  |
| 82         | Bb5                                                                                                                 |
| 81         | A5                                                                                                                  |
| 80         | G#5                                                                                                                 |
| 79         | G5                                                                                                                  |
| 78         | F#5                                                                                                                 |
| 77         | F5                                                                                                                  |
| 76         | E5                                                                                                                  |
| 75         | Eb5                                                                                                                 |
| 74         | D5                                                                                                                  |
| 73         | C#5                                                                                                                 |
| 72         | C5                                                                                                                  |
| 71         | B4                                                                                                                  |
| 70         | Bb4                                                                                                                 |
| 69         | A4                                                                                                                  |
| 68         | G#4                                                                                                                 |
| 67         | G4                                                                                                                  |
| 66         | F#4                                                                                                                 |
| 65         | F4                                                                                                                  |
| 64         | E4                                                                                                                  |
| 63         | Eb4                                                                                                                 |
| 62         | D4                                                                                                                  |
| 61         | C#4                                                                                                                 |
| 60         | C4                                                                                                                  |
| 59         | B3                                                                                                                  |
| 58         | Bb3                                                                                                                 |
| 57         | A3                                                                                                                  |
| 56         | G#3                                                                                                                 |
| 55         | G3                                                                                                                  |
| 54         | F#3                                                                                                                 |
| 53         | F3                                                                                                                  |
| 52         | E3                                                                                                                  |
| 51         | Eb3                                                                                                                 |
| 50         | D3                                                                                                                  |
| 49         | C#3                                                                                                                 |
| 48         | C3                                                                                                                  |
| 47         | B2                                                                                                                  |
| 46         | Bb2                                                                                                                 |
| 45         | A2                                                                                                                  |
| 44         | G#2                                                                                                                 |
| 43         | G2                                                                                                                  |
| 42         | F#2                                                                                                                 |
| 41         | F2                                                                                                                  |
| 40         | E2                                                                                                                  |
| 39         | Eb2                                                                                                                 |
| 38         | D2                                                                                                                  |
| 37         | C#2                                                                                                                 |
| 36         | C2 (Lowest)                                                                                                         |
|            |                                                                                                                     |
| Shifts     |                                                                                                                     |
| 1          | Lyric Shift<br>Sets additional shift points for static lyrics.                                                      |
| 0          | Range Shift<br>Allows the note display range to shift if a vocal range changes drastically. Length determines the speed of the shift, not the duration that it should be shifted for. |

#### Vocals Text Events

Character animation:

| Event Text           | Description                                            |
| :---------           | :----------                                            |
| `[idle]`             | Character idles during a part with no notes            |
| `[idle_realtime]`    | Character idles in real-time (not synced to the beat). |
| `[play]`             | Character starts playing.                              |
| `[mellow]`           | Character plays in a mellow manner.                    |
| `[intense]`          | Character plays in an intense manner.                  |
| `[tambourine_start]` | Character plays a tambourine.                          |
| `[tambourine_end]`   | Character returns from tambourine to vocals.           |
| `[cowbell_start]`    | Character plays a cowbell.                             |
| `[cowbell_end]`      | Character returns from cowbell to vocals.              |
| `[clap_start]`       | Character does claps to the beat.                      |
| `[clap_end]`         | Character returns from claps to vocals.                |

#### Vocals Lyrics

Lyrics are stored as meta events (usually text or lyric), paired up with notes in the 36 to 84 range. Lyric phrases are marked using either note 105 or note 106, or both at the same time (same start and end). (Rock Band 3 phased out the Score Duel mode and only uses note 105 in the chart files.)

There are various symbols used for specific things in lyrics:

| Name           | Symbol | Description                                                                             |
| :---           | :----: | :----------                                                                             |
| Hyphens        | `-`    | Indicates that this syllable should be combined with the next.                          |
| Pluses         | `+`    | Connects the previous note and the current note into a slide. Usually found standalone. |
| Equals         | `=`    | Indicates that a syllable should be joined with the next using a literal hyphen.        |
| Pounds         | `#`    | Marks a note as non-pitched.                                                            |
| Carets         | `^`    | Marks a note as non-pitched with a more lenient scoring.<br>Typically used on short syllables or syllables without sharp attacks. |
| Asterisks      | `*`    | Marks a note as non-pitched, but their differentiation is unknown.                      |
| Percents       | `%`    | Marks a range division for vocals parts with large octave ranges.                       |
| Sections       | `§`    | Indicates that two syllables are sung as a single syllable, used in some Spanish lyrics. Both syllables are in the same event, with the `§` used in place of a space between the two. These are displayed with a tie character `‿` in RB. |
| Dollars        | `$`    | Marks syllables in harmony parts as hidden.<br>In one case these are also on the standard Vocals track for some reason, its purpose is not known (may be an authoring error). |
| Slashes        | `/`    | These appear in some RB charts for an unknown reason, mainly The Beatles: Rock Band.    |

Syllables not joined together through any symbols will be separated by a space.

### Pro Keys Tracks

- `PART REAL_KEYS_X` - Pro Keys Expert
- `PART REAL_KEYS_H` - Pro Keys Hard
- `PART REAL_KEYS_M` - Pro Keys Medium
- `PART REAL_KEYS_E` - Pro Keys Easy

#### Pro Keys Notes

| MIDI Note | Description                                                                                               |
| :-------: | :----------                                                                                               |
| Markers   |                                                                                                           |
| 127       | Trill marker                                                                                              |
| 126       | Glissando marker<br>Only works on Expert                                                                  |
| 120       | Big Rock Ending marker                                                                                    |
| 116       | Overdrive marker<br>For RB3, only OD placed in the Expert track will work, and it must match `PART KEYS`. |
| 115       | Solo marker                                                                                               |
|           |                                                                                                           |
| Notes     |                                                                                                           |
| 72        | C3 (Highest note)                                                                                         |
| 71        | B2                                                                                                        |
| 70        | A#2/Bb2                                                                                                   |
| 69        | A2                                                                                                        |
| 68        | G#2/Ab2                                                                                                   |
| 67        | G2                                                                                                        |
| 66        | F#2/Gb2                                                                                                   |
| 65        | F2                                                                                                        |
| 64        | E2                                                                                                        |
| 63        | D#2/Eb2                                                                                                   |
| 62        | D2                                                                                                        |
| 61        | C#2/Db2                                                                                                   |
| 60        | C2                                                                                                        |
| 59        | B1                                                                                                        |
| 58        | A#1/Bb1                                                                                                   |
| 57        | A1                                                                                                        |
| 56        | G#1/Ab1                                                                                                   |
| 55        | G1                                                                                                        |
| 54        | F#1/Gb1                                                                                                   |
| 53        | F1                                                                                                        |
| 52        | E1                                                                                                        |
| 51        | D#1/Eb1                                                                                                   |
| 50        | D1                                                                                                        |
| 49        | C#1/Db1                                                                                                   |
| 48        | C1 (Lowest note)                                                                                          |
|           |                                                                                                           |
| Ranges    |                                                                                                           |
| 9         | Range shift: A1-C3                                                                                        |
| 7         | Range shift: G1-B2                                                                                        |
| 5         | Range shift: F1-A2                                                                                        |
| 4         | Range shift: E1-G2                                                                                        |
| 2         | Range shift: D1-F2                                                                                        |
| 0         | Range shift: C1-E2                                                                                        |

Additional info:

- Up to 4 notes are allowed at a time, and extended sustains/disjoint chords are allowed.
- Trill and glissando lanes are used to make imprecise/indiscernible trills and glissandi easier to play.
  - Trill markers prevent overhitting and only require the player to alternate faster than a certain threshold to hit the charted notes.
  - Glissando markers disable the scoring system for the notes between the start and end of the glissando.
- The range shift notes will shift the range of the keys shown on the track. 17 keys out of the 25 keys are shown at a time, so if there are notes that go beyond the current range, the range must be shifted to show those notes.
  - These notes do not last the duration of the shift, they simply mark when the shift happens.
  - One of these shifts should be marked at the very beginning as the starting range.

#### Pro Keys Animation Track Notes

- `PART KEYS_ANIM_LH` - RB3 Pro Keys left-hand animations
- `PART KEYS_ANIM_RH` - RB3 Pro Keys right-hand animations

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

### Pro Guitar/Bass Tracks

- `PART REAL_GUITAR` - Pro Guitar (17-Fret)
- `PART REAL_GUITAR_22` - Pro Guitar (22-Fret)
- `PART REAL_GUITAR_BONUS` - Unknown Pro Guitar track
- `PART REAL_BASS` - Pro Bass (17-Fret)
- `PART REAL_BASS_22` - Pro Bass (22-Fret)

Pro Guitar/Bass has some rather complicated mechanics, so some things may have been skimmed over. See [this Rhythm Gaming World post](https://rhythmgamingworld.com/forums/topic/general-thread-for-pro-guitarbass-questions/) for the source of this info.

#### Pro Guitar/Bass Notes and Channels

Gems/markers:

| MIDI Note  | Description                                               |
| :-------:  | :----------                                               |
| Markers    |                                                           |
| 127        | Trill marker                                              |
| 126        | Tremolo marker                                            |
| 125        | Big Rock Ending marker 1                                  |
| 124        | Big Rock Ending marker 2                                  |
| 123        | Big Rock Ending marker 3                                  |
| 122        | Big Rock Ending marker 4                                  |
| 121        | Big Rock Ending marker 5                                  |
| 120        | Big Rock Ending marker 6                                  |
| 116        | Star Power/Overdrive marker                               |
| 115        | Solo marker                                               |
| 108        | Player's hand position marker<br>Marks where the player's left hand should be from this point forward and determines how fret numbers get positioned on top of the note. Fret number determined by note velocity, from 101 to 114 for 17-fret, 119 for 22-fret. |
| 107        | Force chord numbering to display all fret numbers         |
|            |                                                           |
| Expert     |                                                           |
| 106        | Expert Unknown                                            | 
| 105        | Expert string emphasis marker<br>Uses track channels to emphasize certain strings: 13 = EAD, 14 = ADGB, 15 = GBe. |
| 104        | Expert arpeggio marker<br>Requires a corresponding note/chord on track channel 2 to use as a ghost note/chord shape in-game.
| 103        | Expert slide marker<br>Slide direction determined by various rules detailed below. |
| 102        | Expert force HOPO                                         |
| 101        | Expert Purple (e string)                                  |
| 100        | Expert Yellow (B string)                                  |
| 99         | Expert Blue (G string)                                    |
| 98         | Expert Orange (D string)                                  |
| 97         | Expert Green (A string)                                   |
| 96         | Expert Red (E string)                                     |
|            |                                                           |
| Hard       |                                                           |
| 82         | Hard Unknown                                              | 
| 81         | Hard string emphasis marker                               |
| 80         | Hard Arpeggio marker                                      |
| 79         | Hard slide marker                                         |
| 78         | Hard force HOPO                                           |
| 77         | Hard Purple (e string)                                    |
| 76         | Hard Yellow (B string)                                    |
| 75         | Hard Blue (G string)                                      |
| 74         | Hard Orange (D string)                                    |
| 73         | Hard Green (A string)                                     |
| 72         | Hard Red (E string)                                       |
|            |                                                           |
| Medium     |                                                           |
| 58         | Medium Unknown                                            | 
| 56         | Medium Arpeggio marker                                    |
| 55         | Medium slide marker                                       |
| 54         | Medium force HOPO<br>Likely not supported in RB3.         |
| 53         | Medium Purple (e string)                                  |
| 52         | Medium Yellow (B string)                                  |
| 51         | Medium Blue (G string)                                    |
| 50         | Medium Orange (D string)                                  |
| 49         | Medium Green (A string)                                   |
| 48         | Medium Red (E string)                                     |
|            |                                                           |
| Easy       |                                                           |
| 34         | Easy Unknown                                              | 
| 32         | Easy Arpeggio marker                                      |
| 31         | Easy slide marker                                         |
| 30         | Easy force HOPO<br>Likely not supported in RB3.           |
| 29         | Easy Purple (e string)                                    |
| 28         | Easy Yellow (B string)                                    |
| 27         | Easy Blue (G string)                                      |
| 26         | Easy Orange (D string)                                    |
| 25         | Easy Green (A string)                                     |
| 24         | Easy Red (E string)                                       |
|            |                                                           |
| Root Notes |                                                           |
| 18         | Switch the chord name from sharps to flats, or vice versa |
| 17         | Hide the chord name                                       |
| 16         | Marks slash chords, such as `Fm7/E` or `Am/G`<br>Uses the lowest string's fret number for determining what note to use after the slash. |
| 15         | Root note marker: D#/Eb<br>Root notes apply to further notes until a new root marker is placed, at which point that root will continue until the next, and so on. |
| 14         | Root note marker: D                                       |
| 13         | Root note marker: C#/Db                                   |
| 12         | Root note marker: C                                       |
| 11         | Root note marker: B                                       |
| 10         | Root note marker: A#/Bb                                   |
| 9          | Root note marker: A                                       |
| 8          | Root note marker: G#/Ab                                   |
| 7          | Root note marker: G                                       |
| 6          | Root note marker: F#/Gb                                   |
| 5          | Root note marker: F                                       |
| 4          | Root note marker: E                                       |

Channels:

| MIDI Channel | Description                       |
| :----------: | :----------                       |
| 0            | Normal notes and markers          |
| 1            | Ghost notes/arpeggio chord shapes |
| 2            | Bend notes                        |
| 3            | Muted notes<br>In RB3, if any note in a chord is marked as muted, the whole chord will become muted. Chords that contain single muted strings must omit the muted strings in charting. |
| 4            | Tapped notes                      |
| 5            | Harmonics                         |
| 6            | Pinch harmonics                   |
| 13           | String emphasis: EAD              |
| 14           | Bass slap; string emphasis: ADGB  |
| 15           | String emphasis: GBe              |

Additional info:

- The fret number of notes and markers that use fret numbers is determined by the MIDI note velocity, starting from velocity 100 and going to velocity 117 for 17-fret or 122 for 22-fret.
- There are separate tracks for 17-fret and 22-fret due to different controller models having a different amount of available frets.
- Root notes are required for chords, but they don't have to be placed on every chord. It's only necessary to place one when the root note changes.
- RB3 slide direction rules:
  - If the sustain ends within a 1/16th of a following note, the slide will follow the direction of the next note's fret number: lower to higher will slide up, higher to lower will slide down. If both are the same, it will slide down.
  - Otherwise, if the fret number for the corresponding note is 0-7, the slide will go up, otherwise if it's 8 or higher it will go down.
  - For chords, the lowest non-open (fret number of 0) string's fret number determines the direction.
  - At least for muted notes or muted chords, the direction can be reversed by placing the slide marker on channel 12.
- For RB3, overdrive must match that of standard Guitar/Bass.

#### Pro Guitar/Bass Text Events

Song trainers:

- Not usable in RBN2/custom charts, only in custom Pro upgrades

| Event Text                            | Description                                                    |
| :---------                            | :----------                                                    |
| `[begin_pg song_trainer_pg_<number>]` | Marks the start of a Pro Guitar trainer section.<br>`number` is an index for the trainer, starting at 1. |
| `[end_pg song_trainer_pg_<number>]`   | Marks the end of a Pro Guitar trainer section.                 |
| `[pg_norm song_trainer_pg_<number>]`  | Marks the seamless loop point of a Pro Guitar trainer section. |
|                                       |                                                                |
| `[begin_pb song_trainer_pb_<number>]` | Marks the start of a Pro Bass trainer section.                 |
| `[end_pb song_trainer_pb_<number>]`   | Marks the end of a Pro Bass trainer section.                   |
| `[pb_norm song_trainer_pb_<number>]`  | Marks the seamless loop point of a Pro Bass trainer section.   |

Chord names:

| Event Text                  | Description |
| :---------                  | :---------- |
| `[chrd<difficulty> <name>]` | Overrides the chord name shown for the current chord.<br>`difficulty` is a number representing the difficulty: 0 = Easy, 3 = Expert, etc.<br>`name` is the name to use for the chord. A `<gtr></gtr>` tag pair can be used for superscript in RB3.

### Events Track

The Events track contains text events that do various things, along with a few notes for practice mode drum samples.

#### Events Notes

| MIDI Note | Description                        |
| :-------: | :----------                        |
| 26        | Hi-hat practice mode assist sample |
| 25        | Snare practice mode assist sample  |
| 24        | Kick practice mode assist sample   |

These samples presumably play in Practice Mode in RB3 alongside an isolated stem when the speed is set below 100%. Needs checking. 

#### Events Common Text Events

| Event Text                        | Description                                                                              |
| :---------                        | :----------                                                                              |
| `[prc_<name>]`/`[section <name>]` | Marks a practice mode section. `<name>` is the name of the practice mode section.        |
| `[music_start]`                   | Marks the start of the song audio.                                                       |
| `[music_end]`                     | Marks the end of the song audio.                                                         |
| `[end]`                           | Marks the end point of the chart. For Rock Band, it must be the last event in the chart. |
| `[coda]`                          | Starts a Big Rock Ending.                                                                |
|                                   |                                                                                          |
| `[crowd_normal]`                  | Crowd will act in a typical manner.                                                      |
| `[crowd_mellow]`                  | Crowd will act in a mellow manner.                                                       |
| `[crowd_intense]`                 | Crowd will act intensely.                                                                |
| `[crowd_realtime]`                | Crowd will act independently of the beat.                                                |
| `[crowd_clap]`                    | Crowd will clap along to the beat if the player is doing well.                           |
| `[crowd_noclap]`                  | Disables the crowd clap.                                                                 |

### Venue Track

Controls camera placement/effects and venue lighting/effects in Rock Band.

Rock Band Network 1 and Rock Band Network 2 have different sets of venue notes and events, so categories are split into RB1/RB2/RBN1 and RB3/RBN2.

Text events related to this track are detailed in [RB_Events.md](../RockBand/RB_Events.md), as there are a *lot* of them and they don't pertain to other games. The note lists here are mainly just a reference, they take up much less space than the sheer amount of text events do.

#### Venue Notes (RB1/RB2/RBN1)

| MIDI Note | Description                                 |
| :-------: | :----------                                 |
| 110       | Post-processing: Trails                     |
| 109       | Post-processing: Security camera            |
| 108       | Post-processing: Black and white            |
| 107       | Post-processing: Lines                      |
| 106       | Post-processing: Blue tint                  |
| 105       | Post-processing: Mirror                     |
| 104       | Post-processing: Bloom B                    |
| 103       | Post-processing: Bloom A                    |
| 102       | Post-processing: Photocopy                  |
| 101       | Post-processing: Negative                   |
| 100       | Post-processing: Silvertone                 |
| 99        | Post-processing: Sepia                      |
| 98        | Post-processing: 16mm                       |
| 97        | Post-processing: Contrast A                 |
| 96        | Post-processing: Default                    |
|           |                                             |
| 87        | Guitarist sing-along with vocalist          |
| 86        | Drummer sing-along with vocalist            |
| 85        | Bassist sing-along with vocalist            |
|           |                                             |
| 73        | Camera cut: No close-ups                    |
| 72        | Camera cut: Only close-ups                  |
| 71        | Camera cut: Only far cuts                   |
| 70        | Camera cut: No behind cuts                  |
| 64        | Camera cut: Focus on vocalist               |
| 63        | Camera cut: Focus on guitarist              |
| 62        | Camera cut: Focus on drummer                |
| 61        | Camera cut: Focus on bassist                |
| 60        | Camera cut: Random new shot<br>Works in tandem with the 8 notes above. (Speculation) Not used for directed camera cuts. |
|           |                                             |
| 50        | Venue lighting: First keyframe of effect    |
| 49        | Venue lighting: Previous keyframe of effect |
| 48        | Venue lighting: Next keyframe of effect     |
|           |                                             |
| 40        | Spotlight on vocals                         |
| 39        | Spotlight on guitar                         |
| 38        | Spotlight on drums                          |
| 37        | Spotlight on bass                           |

#### Venue Notes (RB3/RBN2)

| MIDI Note | Description                                                                              |
| :-------: | :----------                                                                              |
| 87        | Guitarist sing-along with vocalist<br>Replaced with keyboard player if guitar is absent. |
| 86        | Drummer sing-along with vocalist                                                         |
| 85        | Bassist sing-along with vocalist<br>Replaced with keyboard player if bass is absent.     |
|           |                                                                                          |
| 41        | Spotlight on keys                                                                        |
| 40        | Spotlight on vocals                                                                      |
| 39        | Spotlight on guitar                                                                      |
| 38        | Spotlight on drums                                                                       |
| 37        | Spotlight on bass                                                                        |

#### Venue Text Events (RB1/RB2/RBN1)

##### Directed Cuts (RB1/RB2/RBN1)

Directed cuts are special camera cuts which use animations that don't appear in the standard looping animations for basic camera cuts. These have a variable amount of pre-roll, so the event is placed where the "hit" of the animation goes: for example, if the guitarist should jump or kick the camera, the event should be placed at the time the guitarist should land, or when the kick hits the camera.

There are two ways to call a directed cut:

| Event Text                | Description                                    |
| :---------                | :----------                                    |
| `[do_directed_cut <cut>]` | Always play the cut.                           |
| `[do_optional_cut <cut>]` | Only play the cut if the player is doing well. |

`cut` is the cut to be called:

- Full band cuts:

| Text                  | Description                                        |
| :---------            | :----------                                        |
| `directed_all`        | Band interacts with each other.                    |
| `directed_all_yeah`   | Band impact moment.                                |
| `directed_all_lt`     | Long shot heading away from the stage.             |
| `directed_all_cam`    | Band interacts with camera.                        |
| `directed_bre`        | Band is intense during a big rock ending.          |
| `directed_brej`       | Band impact on the last note of a big rock ending. |

- Single-character cuts:

| Text                  | Description                                                |
| :---------            | :----------                                                |
| `directed_guitar`     | Guitarist impact moment.                                   |
| `directed_guitar_np`  | Guitarist in an idle state.                                |
| `directed_guitar_cam` | Guitarist interacts with camera; not tied to the downbeat. |
| `directed_guitar_cls` | Guitar fretboard closeup.                                  |
| `directed_crowd_g`    | Guitarist interacts with crowd.                            |
|                       |                                                            |
| `directed_bass`       | Bassist impact moment.                                     |
| `directed_bass_np`    | Bassist in an idle state.                                  |
| `directed_bass_cam`   | Bassist interacts with camera; not tied to the downbeat.   |
| `directed_bass_cls`   | Bass fretboard closeup.                                    |
| `directed_crowd_b`    | Bassist interacts with crowd.                              |
|                       |                                                            |
| `directed_drums`      | Drummer impact moment.                                     |
| `directed_drums_np`   | Drummer in an idle state.                                  |
| `directed_drums_kd`   | Drums kick drum closeup.                                   |
| `directed_drums_lt`   | Long shot rotating around the drummer.                     |
| `directed_drums_pnt`  | Drummer points at camera.                                  |
| `directed_duo_drums`  | Drummer sings along, no vocalist in shot.                  |
|                       |                                                            |
| `directed_vocals`     | Vocalist impact moment.                                    |
| `directed_vocals_np`  | Vocalist in an idle state.                                 |
| `directed_vocals_cam` | Vocalist interacts with camera; not tied to the downbeat.  |
| `directed_vocals_cls` | Vocalist closeup.                                          |
| `directed_stagedive`  | Vocalist jumps off stage.                                  |
| `directed_crowdsurf`  | Vocalist goes crowdsurfing.                                |

- Two-character cuts:

| Text                 | Description                                                                                         |
| :---------           | :----------                                                                                         |
| `directed_duo_gb`    | Guitarist and bassist interactions.                                                                 |
| `directed_duo_guitar`| Guitarist sings along and interacts with vocalist.<br>Used in conjunction with the singalong notes. |
| `directed_duo_bass`  | Bassist sings along and interacts with vocalist.<br>Used in conjunction with the singalong notes.   |

##### Venue Lighting (RB1/RB2/RBN1)

| Event Text                  | Description |
| :---------                  | :---------- |
| `[lighting (<descriptor>)]` | Sets a venue lighting cue.<br>A `[verse]` event must be placed before any other lighting call events to initialize the lighting system. |

Lighting types:

- Keyframed calls:

| Text              | Description                |
| :---------        | :----------                |
| Empty (i.e. `()`) | Default lighting.          |
| `manual_cool`     | Cool-temperature lighting. |
| `manual_warm`     | Warm-temperature lighting. |
| `dischord`        | Harsh, dissonant lighting. |
| `stomp`           | All lights on or off.      |

- Automatic calls:

| Text               | Description                                                          |
| :---------         | :----------                                                          |
| `loop_cool`        | Cool-temperature lighting.                                           |
| `loop_warm`        | Warm-temperature lighting.                                           |
| `harmony`          | Harmonious lighting.                                                 |
| `frenzy`           | Frenetic, dissonant lighting.                                        |
| `silhouettes`      | Dark, atmospheric lighting with darkened character silhouettes.      |
| `silhouettes_spot` | Dark, atmospheric lighting with slightly visible characters.         |
| `searchlights`     | Lights that sweep individually.                                      |
| `sweep`            | Lights that sweep together in banks.                                 |
| `strobe_slow`      | Strobe light that blinks every 16th note/120 ticks.                  |
| `strobe_fast`      | Strobe light that blinks every 32nd note/60 ticks.                   |
| `blackout_fast`    | Darken the stage quickly (0.2 seconds).                              |
| `blackout_slow`    | Darken the stage slowly (2 seconds).                                 |
| `flare_slow`       | Bright white flare that fades slowly into the next lighting preset.  |
| `flare_fast`       | Bright white flare that fades quickly into the next lighting preset. |
| `bre`              | Frenetic lighting used during a Big Rock Ending.                     |

##### Other (RB1/RB2/RBN1)

These events trigger an explosion effect or flamethrowers on the venue. These only work in arena venues, and should not be used during a BRE, as they trigger this automatically.

| Event Text           | Description                                               |
| :---------           | :----------                                               |
| `[bonusfx]`          | Triggers an explosion effect.                             |
| `[bonusfx_optional]` | Triggers an explosion effect if the player is doing well. |

These toggle fog on stage:

| Event Text | Description               |
| :--------- | :----------               |
| `[FogOn]`  | Fills the stage with fog. |
| `[FogOff]` | Removes the fog.          |

#### Venue Text Events (RB3/RBN2)

#### Camera Cuts (RB3/RBN2)

These text events specify a camera shot to be used. Only 4 band members can be on-stage at a time out of the 5 possible instrument types, so camera cuts are often stacked on the same point to ensure a proper shot is used. The camera system has a priority list it uses to pick a shot that most closely matches the characters on-stage. If none of the authored cuts match the available parts, it will pick from a list of generic parts.

Available cuts:

(These don't all have specific descriptions for what exactly they do, so descriptions may be extrapolated from the name.)

- Four-character cuts:

| Text                | Description                |
| :---------          | :----------                |
| `[coop_all_behind]` | Behind shot of the band.   |
| `[coop_all_far]`    | Far away shot of the band. |
| `[coop_all_near]`   | Closeup shot of the band.    |

- Three-character cuts (no drums):

| Text                  | Description                                       |
| :---------            | :----------                                       |
| `[coop_front_behind]` | Behind shot of guitarist, bassist, and vocalist.  |
| `[coop_front_near]`   | Closeup shot of guitarist, bassist, and vocalist. |

- One-character standard cuts:

| Text              | Description                          |
| :---------        | :----------                          |
| `[coop_g_behind]` | Behind shot of the guitarist.        |
| `[coop_g_near]`   | Closeup shot of the guitarist.       |
| `[coop_b_behind]` | Behind shot of the bassist.          |
| `[coop_b_near]`   | Closeup shot of the bassist.         |
| `[coop_d_behind]` | Behind shot of the drummer.          |
| `[coop_d_near]`   | Closeup shot of the drummer.         |
| `[coop_v_behind]` | Behind shot of the vocalist.         |
| `[coop_v_near]`   | Closeup shot of the vocalist.        |
| `[coop_k_behind]` | Behind shot of the keyboard player.  |
| `[coop_k_near]`   | Closeup shot of the keyboard player. |

- One-character closeups:

| Text                    | Description                               |
| :---------              | :----------                               |
| `[coop_g_closeup_hand]` | Closeup of the guitarist's hand(s).       |
| `[coop_g_closeup_head]` | Closeup of the guitarist's head.          |
|                         |                                           |
| `[coop_b_closeup_hand]` | Closeup of the bassist's hand(s).         |
| `[coop_b_closeup_head]` | Closeup of the bassist's head.            |
|                         |                                           |
| `[coop_d_closeup_hand]` | Closeup of the drummer's hand(s).         |
| `[coop_d_closeup_head]` | Closeup of the drummer's head.            |
|                         |                                           |
| `[coop_v_closeup]`      | Closeup of the vocalist.                  |
|                         |                                           |
| `[coop_k_closeup_hand]` | Closeup of the keyboard player's hand(s). |
| `[coop_k_closeup_head]` | Closeup of the keyboard player's head.    |

- Two-character cuts:

| Text               | Description                                        |
| :---------         | :----------                                        |
| `[coop_gv_behind]` | Behind shot of the guitarist and vocalist.         |
| `[coop_gv_near]`   | Closeup shot of the guitarist and vocalist.        |
| `[coop_gk_behind]` | Behind shot of the guitarist and keyboard player.  |
| `[coop_gk_near]`   | Closeup shot of the guitarist and keyboard player. |
|                    |                                                    |
| `[coop_bg_behind]` | Behind shot of the bassist and guitarist.          |
| `[coop_bg_near]`   | Closeup shot of the bassist and guitarist.         |
| `[coop_bd_near]`   | Closeup shot of the bassist and drummer.           |
| `[coop_bv_behind]` | Behind shot of the bassist and vocalist.           |
| `[coop_bv_near]`   | Closeup shot of the bassist and vocalist.          |
| `[coop_bk_behind]` | Behind shot of the bassist and keyboard player.    |
| `[coop_bk_near]`   | Closeup shot of the bassist and keyboard player.   |
|                    |                                                    |
| `[coop_dg_near]`   | Closeup shot of the drummer and guitarist.         |
| `[coop_dv_near]`   | Closeup shot of the drummer and vocalist.          |
|                    |                                                    |
| `[coop_kv_behind]` | Behind shot of the keyboard player and vocalist.   |
| `[coop_kv_near]`   | Closeup shot of the keyboard player and vocalist.  |

#### Directed Camera Cuts (RB3/RBN2)

Directed cuts are special camera cuts which have a higher priority over standard cuts and use animations that don't appear in the standard looping animations for basic camera cuts. These are placed where the "hit" of the animation goes: for example, if the guitarist should jump or kick the camera, the event should be placed at the time the guitarist should land, or when the kick hits the camera.

Available cuts:

(As with standard cuts, these don't all have specific descriptions for what exactly they do, so descriptions may be extrapolated from the name.)

- Full band:

| Text                  | Description                                                          |
| :---------            | :----------                                                          |
| `[directed_all]`      | Band interacts with each other.                                      |
| `[directed_all_cam]`  | Band interacts with the camera.                                      |
| `[directed_all_yeah]` | Band impact moment.                                                  |
| `[directed_all_lt]`*  | Long shot heading away from the stage.                               |
| `[directed_bre]`      | Band is intense for a big rock ending.                               |
| `[directed_brej]`     | Band does an impactful action on the last note of a big rock ending. |
| `[directed_crowd]`    | Crowd shot.                                                          |

- Single character:

| Text                       | Description                                |
| :---------                 | :----------                                |
| `[directed_guitar]`        | Guitarist impact moment.                   |
| `[directed_guitar_np]`     | Guitarist in an idle state.                |
| `[directed_guitar_cls]`*   | Guitar fretboard closeup.                  |
| `[directed_guitar_cam_pr]` | Guitarist interacts with camera pre-roll.  |
| `[directed_guitar_cam_pt]` | Guitarist interacts with camera post-roll. |
| `[directed_crowd_g]`       | Guitarist interacts with crowd.            |
|                            |                                            |
| `[directed_bass]`          | Bassist impact moment.                     |
| `[directed_bass_np]`       | Bassist in an idle state.                  |
| `[directed_bass_cam]`      | Bassist interacts with camera,             |
| `[directed_bass_cls]`*     | Bass fretboard closeup.                    |
| `[directed_crowd_b]`       | Bassist interacts with crowd.              |
|                            |                                            |
| `[directed_drums]`         | Drummer impact moment.                     |
| `[directed_drums_lt]`*     | Longer drummer impact moment.              |
| `[directed_drums_np]`      | Drummer in an idle state.                  |
| `[directed_drums_pnt]`     | Drummer points at camera.                  |
| `[directed_drums_kd]`*     | Kick drum closeup.                         |
|                            |                                            |
| `[directed_vocals]`        | Vocalist impact moment.                    |
| `[directed_vocals_np]`     | Vocalist in an idle state.                 |
| `[directed_vocals_cls]`    | Vocalist closeup.                          |
| `[directed_vocals_cam_pr]` | Vocalist interacts with camera pre-roll.   |
| `[directed_vocals_cam_pt]` | Vocalist interacts with camera post-roll.  |
| `[directed_stagedive]`     | Vocalist jumps off stage.                  |
| `[directed_crowdsurf]`     | Vocalist goes crowdsurfing.                |
|                            |                                            |
| `[directed_keys]`          | Keyboard player impact moment.             |
| `[directed_keys_np]`       | Keyboard player in an idle state.          |
| `[directed_keys_cam]`      | Keyboard player interacts with camera.     |

- Two characters:

| Text                    | Description                                                          |
| :---------              | :----------                                                          |
| `[directed_duo_guitar]` | Guitarist sings along and interacts with vocalist.                   |
| `[directed_duo_bass]`   | Bassist sings along and interacts with vocalist.                     |
| `[directed_duo_drums]`  | Drummer sings along, vocalist not visible.                           |
| `[directed_duo_kv]`     | Keyboard player sings along and interacts with vocalist.             |
|                         |                                                                      |
| `[directed_duo_gb]`     | Guitarist and bassist sing along and interact with vocalist.         |
| `[directed_duo_kg]`     | Keyboard player and guitarist sing along and interact with vocalist. |
| `[directed_duo_kb]`     | Keyboard player and bassist sing along and interact with vocalist.   |

*These cuts only apply unique camera angles, they still use standard character animations.

#### Post-Processing Effects (RB3/RBN2)

These apply post-processing effects to the camera. A video demonstrating these effects may be found [here](https://www.youtube.com/watch?v=qUjv-dCQ2gs).

| Event Text                          | Description                                                                     |
| :---------                          | :----------                                                                     |
| `[bloom.pp]`                        | Slightly brightens picture and adds a low-FPS effect                            |
| `[bright.pp]`                       | Drastically brightens everything                                                |
| `[clean_trails.pp]`                 | Small video feed delay (a visual "echo")                                        |
| `[contrast_a.pp]`                   | Gritty, polarized black & white                                                 |
| `[desat_blue.pp]`                   | Black & white with a slight blue tinge                                          |
| `[desat_posterize_trails.pp]`       | Video feed delay with desaturated and posterized colors                         |
| `[film_16mm.pp]`                    | Grainy, slightly darker colors                                                  |
| `[film_b+w.pp]`                     | Desaturates colors to black & white                                             |
| `[film_blue_filter.pp]`             | Blue filter with visible scan lines                                             |
| `[film_contrast.pp]`                | Darkens dark colors, lightens light colors                                      |
| `[film_contrast_blue.pp]`           | Contrasts the blue channel of the image                                         |
| `[film_contrast_green.pp]`          | Contrasts the green channel of the image                                        |
| `[film_contrast_red.pp]`            | Contrasts the red channel of the image                                          |
| `[film_sepia_ink.pp]`               | Hue-shifts colors to yellowish-gray shades                                      |
| `[film_silvertone.pp]`              | Hue-shifts colors to gray shades                                                |
| `[flicker_trails.pp]`               | Black & white video feed delay with wavering brightness                         |
| `[horror_movie_special.pp]`         | Polarizes colors to inverted red & black                                        |
| `[photo_negative.pp]`               | Inverts colors and brightness                                                   |
| `[photocopy.pp]`                    | Black & white with a choppy, low-FPS effect                                     |
| `[posterize.pp]`                    | Posterizes colors                                                               |
| `[ProFilm_a.pp]`                    | Default, no notable effects                                                     |
| `[ProFilm_b.pp]`                    | Slight red effect, slightly muted colors                                        |
| `[ProFilm_mirror_a.pp]`             | Left side mirrors right side, posterizes colors to oranges, greens, and yellows |
| `[ProFilm_psychedelic_blue_red.pp]` | Polarizes colors to either red or blue                                          |
| `[shitty_tv.pp]`                    | Very grainy video with lightened colors and slightly offset color channels      |
| `[space_woosh.pp]`                  | Heavy video feed delay with drastically lightened and offset colors             |
| `[video_a.pp]`                      | Slightly grainy video with visible scanlines                                    |
| `[video_bw.pp]`                     | Black & white with visible scanlines                                            |
| `[video_security.pp]`               | Hue-shifts colors to night-vision green with visible scanlines                  |
| `[video_trails.pp]`                 | Longer video feed delay                                                         |

#### Venue Lighting (RB3/RBN2)

| Event Text                  | Description                                                                             |
| :---------                  | :----------                                                                             |
| `[first]`                   | Goes to the first keyframe of a keyframed lighting cue.                                 |
| `[prev]`                    | Goes to the previous keyframe of a keyframed lighting cue.                              |
| `[next]`                    | Goes to the next keyframe of a keyframed lighting cue.                                  |
| `[lighting (<descriptor>)]` | Sets a lighting cue. `descriptor` is an identifier for the cue to be set, listed below. |
  
Lighting types:

- Keyframed cues
  - These cues have multiple keyframes that can be selected using the `[first]`, `[prev]`, and `[next]` events.

| Text          | Description                                                   |
| :---------    | :----------                                                   |
| `verse`       | Soft but full blends.<br>Varies per venue.                    |
| `chorus`      | Stark, dramatic colors.<br>Varies per venue.                  |
| `manual_cool` | Cool-temperature lighting.                                    |
| `manual_warm` | Warm-temperature lighting.                                    |
| `dischord`    | Harsh blend of dissonant colors.                              |
| `stomp`       | All lights on or off.<br>only responds to the `[next]` event. |

- Automatic cues:

| Text               | Description                                                                              |
| :---------         | :----------                                                                              |
| `intro`            | Transitions lighting from starting state to authored lighting.<br>Not necessary for RB3. |
| `loop_cool`        | Blend of cool-temperature colors.                                                        |
| `loop_warm`        | Blend of warm-temperature colors.                                                        |
| `harmony`          | Blend of harmonious colors.                                                              |
| `frenzy`           | Frenetic, dissonant lighting that alternates quickly.                                    |
| `silhouettes`      | Dark, atmospheric lighting; shows darkened silhouettes of characters.                    |
| `silhouettes_spot` | Dark, atmospheric lighting; characters visible.                                          |
| `searchlights`     | Spotlights that sweep individually.                                                      |
| `sweep`            | Spotlights that sweep together in banks.                                                 |
| `strobe_slow`      | Strobe light that blinks every 16th note.                                                |
| `strobe_fast`      | Strobe light that blinks every 32nd note.                                                |
| `blackout_slow`    | Darkens the stage slowly.                                                                |
| `blackout_fast`    | Darkens the stage quickly.                                                               |
| `blackout_spot`    | Darkened stage with added underlighting.                                                 |
| `flare_slow`       | Bright white flare that fades slowly into the next lighting preset.                      |
| `flare_fast`       | Bright white flare that fades quickly into the next lighting preset.                     |
| `bre`              | Frenetic lighting used during a Big Rock Ending.                                         |

`[lighting ()]`, `[verse]`, and `[chorus]` from RBN1 are not valid for RBN2.

#### Other (RB3/RBN2)

These events trigger an explosion or flamethrowers on the venue. These only work in arena venues.

| Event Text           | Description                                               |
| :---------           | :----------                                               |
| `[bonusfx]`          | Triggers an explosion effect.                             |
| `[bonusfx_optional]` | Triggers an explosion effect if the player is doing well. |

These should not be used during a BRE, as it does this automatically.

### Beat Track

The `BEAT` track marks where upbeats and downbeats in the song are, separately from the tempo and time signature. This drives character animations, lighting, the crowd, and how quickly Overdrive depletes.

Rock Band requires that the last note in this track must occur one beat before the `[end]` event.

#### Beat Notes

| MIDI Note | Description             |
| :-------: | :----------             |
| 13        | Other beats             |
| 12        | First beat of a measure |

## References

- [C3/Rock Band Network docs](http://docs.c3universe.com/rbndocs/index.php?title=Authoring).
- [Rhythm Gaming World forums: "General Thread for PRO Guitar/Bass Questions"](https://rhythmgamingworld.com/forums/topic/general-thread-for-pro-guitarbass-questions/).
  - Since old C3 links redirect to RGW incorrectly: [Rhythm Gaming World forums: "Track Requirements: Pro Guitar/Bass"](https://rhythmgamingworld.com/forums/topic/track-requirements-pro-guitarbass/)
