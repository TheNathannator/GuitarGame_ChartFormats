# Documented Rock Band Text Events

This documents all known text events available for the Rock Band series.

Please note that this list is not 100% comprehensive and may not be fully accurate.

## Table of Contents

## All of RB

### Guitar and Bass Tracks

| Event Text         | Description                                                                                             |
| :---------         | :----------                                                                                             |
| `[idle]`           | Character idles during a part with no notes.                                                            |
| `[idle_realtime]`  | Character idles in real-time (not synced to the beat).                                                  |
| `[idle_intense]`   | Character idles intensely.                                                                              |
| `[play]`           | Character starts playing.                                                                               |
| `[play_solo]`      | Character plays fast while showing off.                                                                 |
| `[mellow]`         | Character plays in a mellow manner.                                                                     |
| `[intense]`        | Character plays in an intense manner.                                                                   |
| `[map <name>]`     | Specifies a hand map to use from this point onwards.<br>See the following 2 tables for known hand maps. |

Left-hand maps:

| Name                | Description                                                                         |
| :---------          | :----------                                                                         |
| `HandMap_Default`   | Single notes use single fingers, sustains use vibrato, chords use multiple fingers. |
| `HandMap_NoChords`  | No chord fingerings.                                                                |
| `HandMap_AllChords` | Only chord fingerings.                                                              |
| `HandMap_AllBend`   | "All ring finger hi vibrato" (sic)                                                  |
| `HandMap_Solo`      | `Dmaj` chord fingering for all chords, vibrato for all chord sustains.              |
| `HandMap_DropD`     | Open-fretting (no fingers down) for all green gems, all other gems are chords.      |
| `HandMap_DropD2`    | Open-fretting (no fingers down) for all green gems.                                 |
| `HandMap_Chord_C`   | `C` chord fingering for all notes.                                                  |
| `HandMap_Chord_D`   | `D` chord fingering for all notes.                                                  |
| `HandMap_Chord_A`   | `A` chord fingering for all notes.                                                  |

Right-hand hand maps (Bass only):

| Name                | Description       |
| :---                | :----------       |
| `StrumMap_Default`  | Finger strumming. |
| `StrumMap_Pick`     | Pick strumming.   |
| `StrumMap_SlapBass` | Slap strumming.   |

### Drums Track

| Event Text             | Description                                                                        |
| :---------             | :----------                                                                        |
| `[idle]`               | Character idles during a part with no notes                                        |
| `[idle_realtime]`      | Character idles in real-time (not synced to the beat).                             |
| `[idle_intense]`       | Character idles intensely.                                                         |
| `[play]`               | Character starts playing.                                                          |
| `[mellow]`             | Character plays in a mellow manner.                                                |
| `[intense]`            | Character plays in an intense manner.                                              |
| `[ride_side_<enable>]` | Character uses a side-swipe to hit the ride. `enable` is either `true` or `false`. |

#### Vocals Track

Animation:

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

#### Pro Guitar/Bass Text Events

Trainers:

- Not usable in RBN2/custom charts, only in custom Pro upgrades

| Event Text                            | Description                                           |
| :---------                            | :----------                                           |
| `[begin_pg song_trainer_pg_<number>]` | Marks the start of a Pro Guitar trainer section.<br>`number` is an index for the trainer, starting at 1. |
| `[end_pg song_trainer_pg_<number>]`   | Marks the end of a Pro Guitar trainer section.        |
| `[pg_norm song_trainer_pg_<number>]`  | Marks the loop point of a Pro Guitar trainer section. |
|                                       |                                                       |
| `[begin_pb song_trainer_pb_<number>]` | Marks the start of a Pro Bass trainer section.        |
| `[end_pb song_trainer_pb_<number>]`   | Marks the end of a Pro Bass trainer section.          |
| `[pb_norm song_trainer_pb_<number>]`  | Marks the loop point of a Pro Bass trainer section.   |

Chord names:

| Event Text                  | Description |
| :---------                  | :---------- |
| `[chrd<difficulty> <name>]` | Overrides the chord name shown for the current chord.<br>`difficulty` is a number representing the difficulty: 0 = Easy, 3 = Expert, etc.<br>`name` is the name to use for the chord. A `<gtr></gtr>` tag pair can be used for superscript in RB3.

### EVENTS Track

| Event Text                        | Description                                                                              |
| :---------                        | :----------                                                                              |
| `[prc_<name>]`/`[section <name>]` | Marks a practice mode section. `<name>` is the name of the practice mode section.        |
|                                   |                                                                                          |
| `[music_start]`                   | Marks the start of the song audio.                                                       |
| `[music_end]`                     | Marks the end of the song audio.                                                         |
| `[end]`                           | Marks the end point of the chart. For Rock Band, it must be the last event in the chart. |
| `[coda]`                          | Starts a Big Rock Ending.                                                                |
|                                   |                                                                                          |
| `[crowd_normal]`                  | Crowd will act in a typical manner.                                                      |
| `[crowd_mellow]`                  | Crowd will act in a mellow manner.                                                       |
| `[crowd_intense]`                 | Crowd will act intensely.                                                                |
| `[crowd_realtime]`                | Crowd will act independently of the beat.                                                |
|                                   |                                                                                          |
| `[crowd_clap]`                    | Crowd will clap along to the beat if the player is doing well.                           |
| `[crowd_noclap]`                  | Disables the crowd clap.                                                                 |

## RB1/2, RBN1

### Venue Text Events (RB1/RB2/RBN1)

#### Directed Cuts (RB1/RB2/RBN1)

Directed cuts are special camera cuts which use animations that don't appear in the standard looping animations for basic camera cuts. These have a variable amount of pre-roll, so the event is placed where the "hit" of the animation goes: for example, if the guitarist should jump or kick the camera, the event should be placed at the time the guitarist should land, or when the kick hits the camera.

There are two ways to call a directed cut:

| Event Text                | Description                                    |
| :---------                | :----------                                    |
| `[do_directed_cut <cut>]` | Always play the cut.                           |
| `[do_optional_cut <cut>]` | Only play the cut if the player is doing well. |

`cut` is the cut to be called:

Full band:

| Text                  | Description                                         |
| :---------            | :----------                                         |
| `directed_all`        | (Not documented in C3 docs)                         |
| `directed_all_yeah`   | (Not documented in C3 docs)                         |
| `directed_bre`        | Band is intense for a BRE.                          |
| `directed_brej`       | Band does an impactful action on the last BRE note. |
| `directed_all_lt`     | Long shot heading away from the stage.              |
| `directed_all_cam`    | Entire band interacts with camera.                  |

Individual characters:

| Text                  | Description                 |
| :---------            | :----------                 |
| `directed_guitar`     | (Not documented in C3 docs) |
| `directed_bass`       | (Not documented in C3 docs) |
| `directed_drums`      | (Not documented in C3 docs) |
| `directed_vocals`     | (Not documented in C3 docs) |

Individual idle characters:

| Text                  | Description                 |
| :---------            | :----------                 |
| `directed_guitar_np`  | (Not documented in C3 docs) |
| `directed_bass_np`    | (Not documented in C3 docs) |
| `directed_drums_np`   | (Not documented in C3 docs) |
| `directed_vocals_np`  | (Not documented in C3 docs) |

Individual character camera interactions:

| Text                  | Description                 |
| :---------            | :----------                 |
| `directed_guitar_cam` | (Not documented in C3 docs) |
| `directed_bass_cam`   | (Not documented in C3 docs) |
| `directed_drums_cam`  | (Not documented in C3 docs) |
| `directed_vocals_cam` | (Not documented in C3 docs) |

Individual character closeups:

| Text                  | Description                 |
| :---------            | :----------                 |
| `directed_guitar_cls` | Guitar fretboard closeup.   |
| `directed_bass_cls`   | Bass fretboard closeup.     |
| `directed_drums_kd`   | Drums kick drum closeup.    |
| `directed_vocals_cls` | (Not documented in C3 docs) |

Character singalongs (used in conjunction with the singalong notes):

| Text                 | Description                                        |
| :---------           | :----------                                        |
| `directed_duo_guitar`| Guitarist sings along and interacts with vocalist. |
| `directed_duo_bass`  | Bassist sings along and interacts with vocalist.   |
| `directed_duo_drums` | Drummer sings along, no vocalist in shot.          |

Miscellaneous cuts:

| Text                 | Description                            |
| :---------           | :----------                            |
| `directed_duo_gb`    | Guitar and bass interactions.          |
| `directed_drums_lt`  | Long shot rotating around the drummer. |
| `directed_drums_pnt` | Drummer points at camera.              |
| `directed_stagedive` | Vocalist jumps off stage.              |
| `directed_crowdsurf` | Vocalist goes crowdsurfing.            |
| `directed_crowd_g`   | Guitarist interacts with crowd.        |
| `directed_crowd_b`   | Bassist interacts with crowd.          |

#### Venue Lighting (RB1/RB2/RBN1)

| Event Text                  | Description               |
| :---------                  | :----------               |
| `[lighting (<descriptor>)]` | Sets a venue lighting cue. A `[verse]` event must be placed before any other lighting call events to initialize the lighting system. |

Lighting types:

Keyframed calls:

| Text          | Description                |
| :---------    | :----------                |
| `()`          | Default lighting.          |
| `manual_cool` | Cool-temperature lighting. |
| `manual_warm` | Warm-temperature lighting. |
| `dischord`    | Harsh, dissonant lighting. |
| `stomp`       | All lights on or off.      |

Automatic calls:

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

#### Other (RB1/RB2/RBN1)

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

## RB3, RBN2

### Venue Text Events (RB3/RBN2)

#### Camera Cuts (RB3/RBN2)

These text events specify a camera shot to be used. Only 4 band members can be on-stage at a time out of the 5 possible instrument types, so camera cuts are often stacked on the same point to ensure a proper shot is used. The camera system has a priority list it uses to pick a shot that most closely matches the characters on-stage. If none of the authored cuts match the available parts, it will pick from a list of generic parts.

These cuts are listed roughly in order from most generic (least priority) to most specific (highest priority).

Four-character cuts:

- `[coop_all_behind]`
- `[coop_all_far]`
- `[coop_all_near]`

Three-character cuts (no drums):

- `[coop_front_behind]`
- `[coop_front_near]`

One-character standard cuts:

- `[coop_d_behind]`
- `[coop_d_near]`
- `[coop_v_behind]`
- `[coop_v_near]`
- `[coop_b_behind]`
- `[coop_b_near]`
- `[coop_g_behind]`
- `[coop_g_near]`
- `[coop_k_behind]`
- `[coop_k_near]`

One-character closeups:

- `[coop_d_closeup_hand]`
- `[coop_d_closeup_head]`
- `[coop_v_closeup]`
- `[coop_b_closeup_hand]`
- `[coop_b_closeup_head]`
- `[coop_g_closeup_hand]`
- `[coop_g_closeup_head]`
- `[coop_k_closeup_hand]`
- `[coop_k_closeup_head]`

Two-character cuts:

- `[coop_dv_near]`
- `[coop_bd_near]`
- `[coop_dg_near]`
- `[coop_bv_behind]`
- `[coop_bv_near]`
- `[coop_gv_behind]`
- `[coop_gv_near]`
- `[coop_kv_behind]`
- `[coop_kv_near]`
- `[coop_bg_behind]`
- `[coop_bg_near]`
- `[coop_bk_behind]`
- `[coop_bk_near]`
- `[coop_gk_behind]`
- `[coop_gk_near]`

#### Directed Camera Cuts (RB3/RBN2)

Directed cuts are special camera cuts which use animations that don't appear in the standard looping animations for basic camera cuts. These are placed where the "hit" of the animation goes: for example, if the guitarist should jump or kick the camera, the event should be placed at the time the guitarist should land, or when the kick hits the camera.

Directed cuts have higher priority than standard camera cuts, and the events listed here are also roughly in priority order.

Full band:

- `[directed_all]`
- `[directed_all_cam]`
- `[directed_all_yeah]`
- `[directed_all_lt]`*
- `[directed_bre]`
- `[directed_brej]`

- `[directed_crowd]`

Single character:

- `[directed_drums]`
- `[directed_drums_pnt]`
- `[directed_drums_np]`
- `[directed_drums_lt]`*
- `[directed_drums_kd]`*
- `[directed_vocals]`
- `[directed_vocals_np]`
- `[directed_vocals_cls]`
- `[directed_vocals_cam_pr]`
- `[directed_vocals_cam_pt]`
- `[directed_stagedive]`
- `[directed_crowdsurf]`
- `[directed_bass]`
- `[directed_crowd_b]`
- `[directed_bass_np]`
- `[directed_bass_cam]`
- `[directed_bass_cls]`*
- `[directed_guitar]`
- `[directed_crowd_g]`
- `[directed_guitar_np]`
- `[directed_guitar_cls]`*
- `[directed_guitar_cam_pr]`
- `[directed_guitar_cam_pt]`
- `[directed_keys]`
- `[directed_keys_cam]`
- `[directed_keys_np]`

Two characters:

- `[directed_duo_drums]`
- `[directed_duo_guitar]`
- `[directed_duo_bass]`
- `[directed_duo_kv]`
- `[directed_duo_gb]`
- `[directed_duo_kb]`
- `[directed_duo_kg]`

*These directed cut events only apply unique camera angles, they still use the standard character animations.

#### Post-Processing Effects (RB3/RBN2)

These text events apply post-processing effects to the camera.

Basic post-processing:

| Event Text              | Description                                                       |
| :---------              | :----------                                                       |
| `[ProFilm_a.pp]`        | Default, no notable effects                                       |
| `[ProFilm_b.pp]`        | Slightly mutes colors                                             |
| `[video_a.pp]`          | Slightly grainy video                                             |
| `[film_16mm.pp]`        | Grainy video                                                      |
| `[shitty_tv.pp]`        | Very grainy video, dramatically lightens colors                   |
| `[bloom.pp]`            | Slightly brightens picture and adds a low-FPS effect              |
| `[film_sepia_ink.pp]`   | Reduces colors to yellowish-gray shades                           |
| `[film_silvertone.pp]`  | Reduces colors to gray shades                                     |
| `[film_b+w.pp]`         | Reduces colors to a smaller range of gray shades                  |
| `[video_bw.pp]`         | Reduces colors to a smaller range of gray shades, slightly gritty |
| `[contrast_a.pp]`       | Very gritty, somewhat polarized black and white                   |
| `[photocopy.pp]`        | Choppy, low frame-per-second effect                               |
| `[film_blue_filter.pp]` | Reduces colors to blue shades                                     |
| `[desat_blue.pp]`       | Produces slightly grainy images with blue tinge                   |
| `[video_security.pp]`   | Grainy, reduces colors to green shades                            |

Special post-processing:

| Event Text                          | Description                                                                            |
| :---------                          | :----------                                                                            |
| `[bright.pp]`                       | Brightens everything                                                                   |
| `[posterize.pp]`                    | Flattens colors                                                                        |
| `[clean_trails.pp]`                 | Creates a small video feed delay (a visual "echo")                                     |
| `[video_trails.pp]`                 | Creates a longer video feed delay,                                                     |
| `[flicker_trails.pp]`               | Creates a video feed delay, slightly darkens images and mutes colors                   |
| `[desat_posterize_trails.pp]`       | Creates a long video feed delay, flattens colors                                       |
| `[film_contrast.pp]`                | Darkens dark colors, lightens light colors                                             |
| `[film_contrast_blue.pp]`           | Darkens dark colors, lightens light colors, slightly blue hues                         |
| `[film_contrast_green.pp]`          | Darkens dark colors, lightens light colors, slightly green hues                        |
| `[film_contrast_red.pp]`            | Darkens dark colors, lightens light colors, slightly red hues                          |
| `[horror_movie_special.pp]`         | Polarizes colors to either red or black                                                |
| `[photo_negative.pp]`               | Inverts colors and brightness                                                          |
| `[ProFilm_mirror_a.pp]`             | Left side of screen mirrors right side, changes colors to oranges, greens, and yellows |
| `[ProFilm_psychedelic_blue_red.pp]` | Polarizes colors to either red or blue                                                 |
| `[space_woosh.pp]`                  | Lightens colors dramatically, creates red/green/blue video feed delay                  |

#### Venue Lighting (RB3/RBN2)

| Event Text                  | Description                                                                                  |
| :---------                  | :----------                                                                                  |
| `[first]`                   | Goes to the first keyframe of a keyframed venue lighting effect.                             |
| `[prev]`                    | Goes to the previous keyframe of a keyframed venue lighting effect.                          |
| `[next]`                    | Goes to the next keyframe of a keyframed venue lighting effect.                              |
| `[lighting (<descriptor>)]` | Sets a venue lighting cue. `descriptor` is an identifier for which cue to set, listed below. |
  
Lighting types:

Keyframed cues:

| Text          | Description                                                  |
| :---------    | :----------                                                  |
| `verse`       | Soft but full blends; varies per venue.                      |
| `chorus`      | Stark, dramatic colors; varies per venue.                    |
| `manual_cool` | Cool-temperature lighting.                                   |
| `manual_warm` | Warm-temperature lighting.                                   |
| `dischord`    | Harsh blend of dissonant colors.                             |
| `stomp`       | All lights on or off; only responds to the `[next]` trigger. |

Automatic cues:

| Text               | Description                                                                                               |
| :---------         | :----------                                                                                               |
| `intro`            | Transitions lighting from starting state to authored lighting.<br>Doesn't seem to be necessary as of RB3. |
| `loop_cool`        | Blend of cool temperature colors.                                                                         |
| `loop_warm`        | Blend of warm temperature colors.                                                                         |
| `harmony`          | Blend of a harmonious color palette.                                                                      |
| `frenzy`           | Frenetic, dissonant colored lighting that alternates quickly.                                             |
| `silhouettes`      | Dark, atmospheric lighting, shows darkened silhouettes of characters.                                     |
| `silhouettes_spot` | Dark, atmospheric lighting, with characters visible.                                                      |
| `searchlights`     | Lights that sweep individually.                                                                           |
| `sweep`            | Lights that sweep together in banks.                                                                      |
| `strobe_slow`      | Strobe light that blinks every 16th note/120 ticks.                                                       |
| `strobe_fast`      | Strobe light that blinks every 32nd note/60 ticks.                                                        |
| `blackout_slow`    | Darken the stage slowly.                                                                                  |
| `blackout_fast`    | Darken the stage quickly.                                                                                 |
| `blackout_spot`    | Darkened stage with added underlighting.                                                                  |
| `flare_slow`       | Bright white flare; fades slowly into the next lighting preset.                                           |
| `flare_fast`       | Bright white flare; fades quickly into the next lighting preset.                                          |
| `bre`              | Frenetic lighting used during a Big Rock Ending.                                                          |

`[lighting ()]`, `[verse]`, and `[chorus]` from RBN1 are not valid for RBN2.

#### Pyrotechnics (RB3/RBN2)

These events trigger an explosion or flamethrowers on the venue. These only work in arena venues.

| Event Text           | Description                                                                         |
| :---------           | :----------                                                                         |
| `[bonusfx]`          | Triggers an explosion effect.                                                       |
| `[bonusfx_optional]` | Same as above, but the effect will only be triggered when the player is doing well. |

These should not be used during a BRE, as it does this automatically.
