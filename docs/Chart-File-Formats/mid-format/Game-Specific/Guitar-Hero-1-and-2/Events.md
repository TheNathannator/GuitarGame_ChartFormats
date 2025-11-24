# Guitar Hero 1 and 2 Events Track

These are the text events found in Guitar Hero 1 and 2 songs.

## Guitar Hero 1 Text Events

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

## Guitar Hero 2 Text Events

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

| Text        | Description                                                          |
| :---------  | :----------                                                          |
| None (`()`) | Default lighting.                                                    |
| `flare`     | Sets off flares that come up out of the stage.                       |
| `blackout`  | Turns off the stage lights.                                          |
| `chase`     | (Speculated) A spotlight will chase the guitarist.                   |
| `strobe`    | Enables a strobe-light effect.                                       |
| `color1`    | Changes the lighting color to the first color specified in a venue.  |
| `color2`    | Changes the lighting color to the second color specified in a venue. |
| `color3`    | Changes the lighting color to the third color specified in a venue.  |
| `sweep`     | Makes the lights sweep the stage.                                    |
