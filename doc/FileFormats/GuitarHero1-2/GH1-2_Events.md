# Documented Guitar Hero 1 and 2 Text Events

This documents all known text events available for the Rock Band series.

Please note that this list is not 100% comprehensive and may not be fully accurate.

These are sourced from [this ScoreHero post](https://www.scorehero.com/forum/viewtopic.php?t=20179).

This document pertains to .mid, though these events may als be found in some .chart files, with or without square brackets.

## Table of Contents

## Guitar Hero 1

### EVENTS Track Text Events

| Event Text       | Description                                      |
| :---------       | :----------                                      |
| `[verse]`        | Marks a verse in the song for stage animations.  |
| `[chorus]`       | Marks a chorus in the song for stage animations. |
| `[solo]`         | Marks a solo in the song for stage animations.   |
| `[end]`          | Marks the end point of the song.                 |
|                  |                                                  |
| `[gtr_off]`      | Guitarist is idle.                               |
| `[gtr_on]`       | Guitarist moves around.                          |
| `[solo_on]`      | Guitarist shows off during a solo.               |
| `[solo_off]`     | Guitarist transitions back to normal.            |
| `[wail_on]`      | Guitarist wails on their guitar.                 |
| `[wail_off]`     | Guitarist transitions back to normal.            |
|                  |                                                  |
| `[bass_off]`     | Bassist is idle.                                 |
| `[bass_on]`      | Bassist is active.                               |
|                  |                                                  |
| `[drum_off]`     | Drummer is idle.                                 |
| `[drum_on]`      | Drummer is active.                               |
| `[drum_half]`    | Drummer plays in half time.                      |
| `[drum_double]`  | Drummer plays in double time.                    |
| `[drum_normal]`  | Drummer plays a normal rock pattern.             |
| `[drum_allbeat]` | Drummer hits the snare drum on all beats.        |
|                  |                                                  |
| `[sing_off]`     | Singer is idle.                                  |
| `[sing_on]`      | Singer is active.                                |
|                  |                                                  |
| `[keys_off]`     | Keyboardist is idle.                             |
| `[keys_on]`      | Keyboardist is active.                           |

## Guitar Hero 2

### Guitar and Bass Tracks

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

### BAND_DRUM Track

| Event Text       | Description                                       |
| :---------       | :----------                                       |
| `[idle]`         | Drummer idles during a part with no notes.        |
| `[play]`         | Drummer starts playing.                           |
| `[normal_tempo]` | Drummer plays kick drum on first and third beats. |
| `[half_time]`    | Drummer plays kick drum on first beat only.       |
| `[double_time]`  | Drummer plays kick drum in eighth note pattern.   |
| `[allbeat]`      | Drummer plays kick drum in quarter note pattern.  |

### BAND_BASS Track

| Event Text | Description                                |
| :--------- | :----------                                |
| `[idle]`   | Bassist idles during a part with no notes. |
| `[play]`   | Bassist starts playing.                    |

### BAND_SINGER Text Events

| Event Text | Description                                 |
| :--------- | :----------                                 |
| `[idle]`   | Vocalist idles during a part with no notes. |
| `[play]`   | Vocalist starts playing.                    |

### EVENTS Track

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

`[lighting]` event `type`s:

| Text         | Description                                                          |
| :---------   | :----------                                                          |
| `()`         | Default lighting.                                                    |
| `(flare)`    | Sets off flares that come up out of the stage.                       |
| `(blackout)` | Turns off the stage lights.                                          |
| `(chase)`    | (Speculated) A spotlight will chase the guitarist.                   |
| `(strobe)`   | Enables a strobe-light effect.                                       |
| `(color1)`   | Changes the lighting color to the first color specified in a venue.  |
| `(color2)`   | Changes the lighting color to the second color specified in a venue. |
| `(color3)`   | Changes the lighting color to the third color specified in a venue.  |
| `(sweep)`    | Makes the lights sweep the stage.                                    |
