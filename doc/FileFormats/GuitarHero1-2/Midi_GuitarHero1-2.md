# Guitar Hero 1/2 MIDI Files

This document details the format of Guitar Hero 1/2 .mid charts. See [Midi.md](../_General/Midi.md) for a more general overview of .mid charts.

I do not currently have access to the original files of the game to research them myself, instead I'm referencing some old ScoreHero threads. Take the information here with a grain of salt, and maybe consider adding your own corrections if you see something you know is wrong or missing.

## Table of Contents

- [Guitar Hero 1](#guitar-hero-1)
  - [GH1 Track Names](#gh1-track-names)
  - [T1 GEMS Track](#t1-gems-track)
    - [T1 GEMS Notes](#t1-gems-notes)
  - [ANIM Track](#anim-track)
    - [ANIM Notes](#anim-notes)
  - [TRIGGERS Track](#triggers-track)
    - [TRIGGERS Notes](#triggers-notes)
  - [EVENTS Track](#events-track)
    - [EVENTS Text Events](#events-text-events)
- [Guitar Hero 2](#guitar-hero-2)
  - [GH2 Track Names](#gh2-track-names)
  - [Instrument Tracks](#instrument-tracks)
  - [Instrument Track Notes](#instrument-track-notes)
    - [Instrument Track Text Events](#instrument-track-text-events)
  - [BAND_BASS Track](#band_bass-track)
    - [BAND_BASS Notes](#band_bass-notes)
    - [BAND_BASS Text Events](#band_bass-text-events)
  - [BAND_DRUM Track](#band_drum-track)
    - [BAND_DRUM Notes](#band_drum-notes)
    - [BAND_DRUM Text Events](#band_drum-text-events)
  - [BAND_SINGER Track](#band_singer-track)
    - [BAND_SINGER Notes](#band_singer-notes)
    - [BAND_SINGER Text Events](#band_singer-text-events)
  - [TRIGGERS Track](#triggers-track-1)
    - [TRIGGERS Notes](#triggers-notes-1)
  - [EVENTS Track](#events-track-1)
    - [EVENTS Text Events](#events-text-events-1)
- [References](#references)

## Guitar Hero 1

### GH1 Track Names

| Track Name | Track Description                                      |
| :--------- | :----------------                                      |
| `T1 GEMS`  | Lead Guitar                                            |
| `ANIM`     | Guitarist animations track                             |
| `TRIGGERS` | Lighting, venue, and practice mode drum sample track   |
| `EVENTS`   | Global events                                          |

### T1 GEMS Track

This track contains both the guitar track notes/markers/phrases, and the vocalist's mouth open/close notes.

#### T1 GEMS Notes

| MIDI Note | Description                                                                     |
| :-------: | :----------                                                                     |
| 108       | Vocalist mouth open/close<br>Note on = open, note off = closed.                 |
|           |                                                                                 |
| Expert    |                                                                                 |
| 106       | Expert player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker. |
| 105       | Expert player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker. |
| 103       | Expert Star Power marker                                                        |
| 100       | Expert Orange (5th lane)                                                        |
| 99        | Expert Blue (4th lane)                                                          |
| 98        | Expert Yellow (3rd lane)                                                        |
| 97        | Expert Red (2nd lane)                                                           |
| 96        | Expert Green (1st lane)                                                         |
|           |                                                                                 |
| Hard      |                                                                                 |
| 94        | Hard player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker.   |
| 93        | Hard player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker.   |
| 91        | Hard Star Power marker                                                          |
| 88        | Hard Orange (5th lane)                                                          |
| 87        | Hard Blue (4th lane)                                                            |
| 86        | Hard Yellow (3rd lane)                                                          |
| 85        | Hard Red (2nd lane)                                                             |
| 84        | Hard Green (1st lane)                                                           |
|           |                                                                                 |
| Medium    |                                                                                 |
| 82        | Medium player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker. |
| 81        | Medium player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker. |
| 79        | Medium Star Power marker                                                        |
| 76        | Medium Orange (5th lane)                                                        |
| 75        | Medium Blue (4th lane)                                                          |
| 74        | Medium Yellow (3rd lane)                                                        |
| 73        | Medium Red (2nd lane)                                                           |
| 72        | Medium Green (1st lane)                                                         |
|           |                                                                                 |
| Easy      |                                                                                 |
| 70        | Easy player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker.   |
| 69        | Easy player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker.   |
| 67        | Easy Star Power marker                                                          |
| 64        | Easy Orange (5th lane)                                                          |
| 63        | Easy Blue (4th lane)                                                            |
| 62        | Easy Yellow (3rd lane)                                                          |
| 61        | Easy Red (2nd lane)                                                             |
| 60        | Easy Green (1st lane)                                                           |

- Notes get set as HOPOs (hammer-ons/pull-offs) automatically if they are within 160 ticks of the previous note, unless they are the same lane as the previous note, or are a chord.
  - Notes can *not* be forced as strums or HOPOs in GH1/2.
- Notes shorter than 160 ticks step will be cut off and turned into a normal note.
- Notes with a position difference of 10 ticks or less will be snapped together as a chord, with the position to snap to being the position of the earliest note within the chord. This includes sustains as well.

### ANIM Track

#### ANIM Notes

| MIDI Note | Description                           |
| :-------: | :----------                           |
| 59        | Left hand position 20<br>May not actually be part of the range? But it would make sense to go to 20 and not a more arbitrary 19 |
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

### TRIGGERS Track

#### TRIGGERS Notes

This list is incomplete.

| MIDI Note | Description |
| :-------: | :---------- |
| 61        | Unknown     | 
| 60        | Unknown     | 

### EVENTS Track

#### EVENTS Text Events

| Event Text       | Description                                      |
| :---------       | :----------                                      |
| `[verse]`        | Marks a verse in the song for stage animations.  |
| `[chorus]`       | Marks a chorus in the song for stage animations. |
| `[solo]`         | Marks a solo in the song for stage animations.   |
| `[end]`          | Marks the end point of the song.                 |
|                  |                                                  |
| `[gtr_on]`       | Guitarist moves around.                          |
| `[gtr_off]`      | Guitarist is idle.                               |
| `[solo_on]`      | Guitarist shows off during a solo.               |
| `[solo_off]`     | Guitarist transitions back to normal.            |
| `[wail_on]`      | Guitarist wails on their guitar.                 |
| `[wail_off]`     | Guitarist transitions back to normal.            |
|                  |                                                  |
| `[bass_on]`      | Bassist is active.                               |
| `[bass_off]`     | Bassist is idle.                                 |
|                  |                                                  |
| `[drum_on]`      | Drummer is active.                               |
| `[drum_off]`     | Drummer is idle.                                 |
| `[drum_half]`    | Drummer plays in half time.                      |
| `[drum_double]`  | Drummer plays in double time.                    |
| `[drum_normal]`  | Drummer plays a normal rock pattern.             |
| `[drum_allbeat]` | Drummer hits the snare drum on all beats.        |
|                  |                                                  |
| `[sing_on]`      | Singer is active.                                |
| `[sing_off]`     | Singer is idle.                                  |
|                  |                                                  |
| `[keys_on]`      | Keyboard player is active.                       |
| `[keys_off]`     | Keyboard player is idle.                         |

## Guitar Hero 2

### GH2 Track Names

| Track Name         | Track Description                                    |
| :---------         | :----------------                                    |
| `PART GUITAR`      | Guitar                                               |
| `PART GUITAR COOP` | Co-op Guitar                                         |
| `PART RHYTHM`      | Rhythm Guitar                                        |
| `PART BASS`        | Bass Guitar                                          |
| `BAND_BASS`        | Bassist animations if `PART BASS` not present        |
| `BAND_DRUMS`       | Drummer animations                                   |
| `BAND_SINGER`      | Vocalist animations                                  |
| `TRIGGERS`         | Lighting, venue, and practice mode drum sample track |
| `EVENTS`           | Global events                                        |

### Instrument Tracks

Track names:

- `PART GUITAR` - Guitar
- `PART GUITAR COOP` - Co-op Guitar
- `PART RHYTHM` - Rhythm Guitar
- `PART BASS` - Bass Guitar

Rhythm and Bass are mutually exclusive: only one or the other may be present in a single chart.

### Instrument Track Notes

| MIDI Note | Description                                                                     |
| :-------: | :----------                                                                     |
| Markers   |                                                                                 |
| 110       | "Big note" marker<br>The crowd will react negatively to missing notes marked with this. Notes should be marked individually, not as a phrase. |
|           |                                                                                 |
| Expert    |                                                                                 |
| 106       | Expert player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker. |
| 105       | Expert player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker. |
| 103       | Expert Star Power marker                                                        |
| 100       | Expert Orange (5th lane)                                                        |
| 99        | Expert Blue (4th lane)                                                          |
| 98        | Expert Yellow (3rd lane)                                                        |
| 97        | Expert Red (2nd lane)                                                           |
| 96        | Expert Green (1st lane)                                                         |
|           |                                                                                 |
| Hard      |                                                                                 |
| 94        | Hard player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker.   |
| 93        | Hard player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker.   |
| 91        | Hard Star Power marker                                                          |
| 88        | Hard Orange (5th lane)                                                          |
| 87        | Hard Blue (4th lane)                                                            |
| 86        | Hard Yellow (3rd lane)                                                          |
| 85        | Hard Red (2nd lane)                                                             |
| 84        | Hard Green (1st lane)                                                           |
|           |                                                                                 |
| Medium    |                                                                                 |
| 82        | Medium player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker. |
| 81        | Medium player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker. |
| 79        | Medium Star Power marker                                                        |
| 76        | Medium Orange (5th lane)                                                        |
| 75        | Medium Blue (4th lane)                                                          |
| 74        | Medium Yellow (3rd lane)                                                        |
| 73        | Medium Red (2nd lane)                                                           |
| 72        | Medium Green (1st lane)                                                         |
|           |                                                                                 |
| Easy      |                                                                                 |
| 70        | Easy player 2 Face-Off phrase marker<br>Can overlap with the Player 1 marker.   |
| 69        | Easy player 1 Face-Off phrase marker<br>Can overlap with the Player 2 marker.   |
| 67        | Easy Star Power marker                                                          |
| 64        | Easy Orange (5th lane)                                                          |
| 63        | Easy Blue (4th lane)                                                            |
| 62        | Easy Yellow (3rd lane)                                                          |
| 61        | Easy Red (2nd lane)                                                             |
| 60        | Easy Green (1st lane)                                                           |
|           |                                                                                 |
| Animation |                                                                                 |
| 59        | Left hand position 20<br>May not actually be part of the range? But it would make sense to go to 20 and not a more arbitrary 19 |
| 58        | Left hand position 19                                                           |
| 57        | Left hand position 18                                                           |
| 56        | Left hand position 17                                                           |
| 55        | Left hand position 16                                                           |
| 54        | Left hand position 15                                                           |
| 53        | Left hand position 14                                                           |
| 52        | Left hand position 13                                                           |
| 51        | Left hand position 12                                                           |
| 50        | Left hand position 11                                                           |
| 49        | Left hand position 10                                                           |
| 48        | Left hand position 9                                                            |
| 47        | Left hand position 8                                                            |
| 46        | Left hand position 7                                                            |
| 45        | Left hand position 6                                                            |
| 44        | Left hand position 5                                                            |
| 43        | Left hand position 4                                                            |
| 42        | Left hand position 3                                                            |
| 41        | Left hand position 2                                                            |
| 40        | Left hand position 1 (near headstock)                                           |

HOPO forcing, sustain cutoffs, and chord snapping work the same as in GH1.

#### Instrument Track Text Events

| Event Text       | Description                                                                                          |
| :---------       | :----------                                                                                          |
| `[idle]`         | Character idles during a part with no notes.                                                         |
| `[play]`         | Character starts playing.                                                                            |
| `[solo_on]`      | Character shows off during a solo.                                                                   |
| `[solo_off]`     | Character transitions back to normal.                                                                |
| `[wail_on]`      | Character wails on their instrument.                                                                 |
| `[wail_off]`     | Character transitions back to normal.                                                                |
| `[half_tempo]`   | Makes the character move at half the song's tempo.                                                   |
| `[normal_tempo]` | Returns the character's movements to normal tempo.                                                   |
| `[map <name>]`   | Specifies a hand map to use from this point onwards.<br>See the following table for known hand maps. |

Hand maps:

| Name               | Description          |
| :---               | :----------          |
| `HandMap_Default`  | Default hand map.    |
| `HandMap_NoChords` | No chord fingerings. |
| `HandMap_DropD2`   | Drop-D2 fingerings.  |

### BAND_BASS Track

This track is not present if the co-op track is Bass.

#### BAND_BASS Notes

| MIDI Note | Description             |
| :-------: | :----------             |
| 36        | Strum animation trigger |

#### BAND_BASS Text Events

| Event Text | Description                                |
| :--------- | :----------                                |
| `[idle]`   | Bassist idles during a part with no notes. |
| `[play]`   | Bassist starts playing.                    |

### BAND_DRUM Track

#### BAND_DRUM Notes

| MIDI Note | Description  |
| :-------: | :----------  |
| 37        | Crash cymbal |
| 36        | Kick drum    |

#### BAND_DRUM Text Events

| Event Text       | Description                                       |
| :---------       | :----------                                       |
| `[idle]`         | Drummer idles during a part with no notes.        |
| `[play]`         | Drummer starts playing.                           |
| `[normal_tempo]` | Drummer plays kick drum on first and third beats. |
| `[half_time]`    | Drummer plays kick drum on first beat only.       |
| `[double_time]`  | Drummer plays kick drum in eighth note pattern.   |
| `[allbeat]`      | Drummer plays kick drum in quarter note pattern.  |

### BAND_SINGER Track

#### BAND_SINGER Notes

| MIDI Note | Description                                            |
| :-------: | :----------                                            |
| 108       | Mouth open/close<br>Note on = open, note off = closed. |

#### BAND_SINGER Text Events

| Event Text | Description                                 |
| :--------- | :----------                                 |
| `[idle]`   | Vocalist idles during a part with no notes. |
| `[play]`   | Vocalist starts playing.                    |

### TRIGGERS Track

#### TRIGGERS Notes

This list is incomplete.

| MIDI Note | Description                                 |
| :-------: | :----------                                 |
| 61        | Unknown                                     | 
| 60        | Unknown                                     | 
| 52        | Venue-specific effect                       |
| 50        | Venue lighting: First keyframe of effect    |
| 49        | Venue lighting: Previous keyframe of effect |
| 48        | Venue lighting: Next keyframe of effect     |
| 26        | Hi-hat practice mode assist sample          |
| 25        | Snare practice mode assist sample           |
| 24        | Kick practice mode assist sample            |

### EVENTS Track

#### EVENTS Text Events

| Event Text              | Description                                         |
| :---------              | :----------                                         |
| `[music_start]`         | Marks the start of the song for stage animations.   |
| `[verse]`               | Marks a verse in the song for stage animations.     |
| `[chorus]`              | Marks a chorus in the song for stage animations.    |
| `[solo]`                | Marks a solo in the song for stage animations.      |
| `[end]`                 | Marks the end point of the song.                    |
|                         |                                                     |
| `[crowd_lighters_fast]` | Makes the crowd wave lighters quickly.              |
| `[crowd_lighters_off]`  | Makes the crowd no longer have lighters.            |
| `[crowd_lighters_slow]` | Makes the crowd wave lighters slowly.               |
| `[crowd_half_tempo]`    | Crowd moves at half of the set tempo.               |
| `[crowd_normal_tempo]`  | Crowd moves at the set tempo.                       |
| `[crowd_double_tempo]`  | Crowd moves at double the set tempo.                |
|                         |                                                     |
| `[band_jump]`           | Starts a camera cut where the band jumps in unison. |
| `[sync_head_bang]`      | Makes the band head-bang in sync with each other.   |
| `[sync_wag]`            | Makes the band sway left and right to the beat.     |
|                         |                                                     |
| `[section <name>]`      | Marks a practice mode section.                      |
|                         |                                                     |
| `[lighting (<type>)]`   | Triggers a lighting change or event.<br>The `[music_start]` event must be present beforehand along with either `[verse]`, `[chorus]`, or `[solo]` in order for these to work. |

`[lighting]` event types:

| Text              | Description                                                          |
| :---------        | :----------                                                          |
| Empty (i.e. `()`) | Default lighting.                                                    |
| `(flare)`         | Sets off flares that come up out of the stage.                       |
| `(blackout)`      | Turns off the stage lights.                                          |
| `(chase)`         | (Speculated) A spotlight will chase the guitarist.                   |
| `(strobe)`        | Enables a strobe-light effect.                                       |
| `(color1)`        | Changes the lighting color to the first color specified in a venue.  |
| `(color2)`        | Changes the lighting color to the second color specified in a venue. |
| `(color3)`        | Changes the lighting color to the third color specified in a venue.  |
| `(sweep)`         | Makes the lights sweep the stage.                                    |

## References

Most of this info comes from these Score Hero threads:

- ["Guitar Hero MIDI and VGS File Details"](https://www.scorehero.com/forum/viewtopic.php?t=1179)
- ["Lighting/Band/Crowd Events & Notes (New lighting ideas.)"](https://www.scorehero.com/forum/viewtopic.php?t=20179)
- [ScoreHero forums: "GH2 Midi Encyclopedia / Dictionary"](https://www.scorehero.com/forum/viewtopic.php?t=5523)
