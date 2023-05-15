# Rock Band Venue Track

This track controls camera placement/effects and venue lighting/effects in Rock Band.

Rock Band Network 1 and Rock Band Network 2 have different sets of venue notes and events, so categories are split into RB1/RB2/RBN1 and RB3/RBN2.

## Table of Contents

- [Track Names](#track-names)
- [Track Notes](#track-notes)
  - [RB1, RB2, RBN1](#rb1-rb2-rbn1)
  - [RB3, RBN2](#rb3-rbn2)
- [Text Events](#text-events)
  - [RB1, RB2, RBN1](#rb1-rb2-rbn1-1)
    - [Directed Camera Cuts](#directed-camera-cuts)
    - [Venue Lighting](#venue-lighting)
    - [Other](#other)
  - [RB3, RBN2](#rb3-rbn2-1)
    - [Camera Cuts](#camera-cuts)
    - [Directed Camera Cuts](#directed-camera-cuts-1)
    - [Post-Processing Effects](#post-processing-effects)
    - [Venue Lighting](#venue-lighting-1)
    - [Other](#other-1)

## Track Names

- `VENUE` - Standard venue effects
- `MV VENUE` - (RB2 only) Venue effects for music video venues

## Track Notes

### RB1, RB2, RBN1

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

### RB3, RBN2

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

## Text Events

### RB1, RB2, RBN1

#### Directed Camera Cuts

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

#### Venue Lighting

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

#### Other

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

### RB3, RBN2

#### Camera Cuts

These text events specify a camera shot to be used. Only 4 band members can be on-stage at a time out of the 5 possible instrument types, so camera cuts are often stacked on the same point to ensure a proper shot is used. The camera system has a priority list it uses to pick a shot that most closely matches the characters on-stage. If none of the authored cuts match the available parts, it will pick from a list of generic parts.

Available cuts:

(These don't all have specific descriptions for what exactly they do, so descriptions may be extrapolated from the name.)

- Four-character cuts:

| Text                | Description                |
| :---------          | :----------                |
| `[coop_all_behind]` | Behind shot of the band.   |
| `[coop_all_far]`    | Far away shot of the band. |
| `[coop_all_near]`   | Closeup shot of the band.  |

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

#### Directed Camera Cuts

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

#### Post-Processing Effects

These apply post-processing effects to the camera. A video demonstrating these effects may be found [here](https://www.youtube.com/watch?v=qUjv-dCQ2gs).

| Event Text                          | Description                                                                      |
| :---------                          | :----------                                                                      |
| `[bloom.pp]`                        | Slightly brightens picture and adds a low-FPS effect.                            |
| `[bright.pp]`                       | Drastically brightens everything.                                                |
| `[clean_trails.pp]`                 | Small video feed delay (a visual "echo").                                        |
| `[contrast_a.pp]`                   | Gritty, polarized black & white.                                                 |
| `[desat_blue.pp]`                   | Black & white with a slight blue tinge.                                          |
| `[desat_posterize_trails.pp]`       | Video feed delay with desaturated and posterized colors.                         |
| `[film_16mm.pp]`                    | Grainy, slightly darker colors.                                                  |
| `[film_b+w.pp]`                     | Desaturates colors to black & white.                                             |
| `[film_blue_filter.pp]`             | Blue filter with visible scan lines.                                             |
| `[film_contrast.pp]`                | Darkens dark colors, lightens light colors.                                      |
| `[film_contrast_blue.pp]`           | Contrasts the blue channel of the image.                                         |
| `[film_contrast_green.pp]`          | Contrasts the green channel of the image.                                        |
| `[film_contrast_red.pp]`            | Contrasts the red channel of the image.                                          |
| `[film_sepia_ink.pp]`               | Hue-shifts colors to yellowish-gray shades.                                      |
| `[film_silvertone.pp]`              | Hue-shifts colors to gray shades.                                                |
| `[flicker_trails.pp]`               | Black & white video feed delay with wavering brightness.                         |
| `[horror_movie_special.pp]`         | Polarizes colors to inverted red & black.                                        |
| `[photo_negative.pp]`               | Inverts colors and brightness.                                                   |
| `[photocopy.pp]`                    | Black & white with a choppy, low-FPS effect.                                     |
| `[posterize.pp]`                    | Posterizes colors.                                                               |
| `[ProFilm_a.pp]`                    | Default, no notable effects.                                                     |
| `[ProFilm_b.pp]`                    | Slight red effect, slightly muted colors.                                        |
| `[ProFilm_mirror_a.pp]`             | Left side mirrors right side, posterizes colors to oranges, greens, and yellows. |
| `[ProFilm_psychedelic_blue_red.pp]` | Polarizes colors to either red or blue.                                          |
| `[shitty_tv.pp]`                    | Very grainy video with lightened colors and slightly offset color channels.      |
| `[space_woosh.pp]`                  | Heavy video feed delay with drastically lightened and offset colors.             |
| `[video_a.pp]`                      | Slightly grainy video with visible scanlines.                                    |
| `[video_bw.pp]`                     | Black & white with visible scanlines.                                            |
| `[video_security.pp]`               | Hue-shifts colors to night-vision green with visible scanlines.                  |
| `[video_trails.pp]`                 | Longer video feed delay.                                                         |

#### Venue Lighting

| Event Text                  | Description                                                                             |
| :---------                  | :----------                                                                             |
| `[first]`                   | Goes to the first keyframe of a keyframed lighting cue.                                 |
| `[prev]`                    | Goes to the previous keyframe of a keyframed lighting cue.                              |
| `[next]`                    | Goes to the next keyframe of a keyframed lighting cue.                                  |
| `[lighting (<descriptor>)]` | Sets a lighting cue. `descriptor` is an identifier for the cue to be set, listed below. |
  
Lighting types:

- Keyframed cues:
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

#### Other

These events trigger an explosion or flamethrowers on the venue. These only work in arena venues.

| Event Text           | Description                                               |
| :---------           | :----------                                               |
| `[bonusfx]`          | Triggers an explosion effect.                             |
| `[bonusfx_optional]` | Triggers an explosion effect if the player is doing well. |

These should not be used during a BRE, as it does this automatically.
