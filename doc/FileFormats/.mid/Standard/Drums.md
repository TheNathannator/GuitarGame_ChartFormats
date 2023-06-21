# Drums in .mid

## Table of Contents

- [Drums Track Types](#drums-track-types)
  - [Determining Track Type](#determining-track-type)
  - [Track Type Conversions](#track-type-conversions)
- [Track Names](#track-names)
- [Track Notes](#track-notes)
  - [4-Lane Note Mechanics](#4-lane-note-mechanics)
  - [4-Lane Phrase Mechanics](#4-lane-phrase-mechanics)
  - [5-Lane Note Mechanics](#5-lane-note-mechanics)
- [Important Text Events](#important-text-events)
  - [Mix Event Details](#mix-event-details)

## Drums Track Types

There are three types of drum tracks:

- Standard 4-lane
  - Four lanes, drums and cymbals as the same note type
- 4-lane Pro
  - Four lanes, drums and cymbals as separate notes but still sharing lanes
  - Four drums, three cymbals
- 5-lane
  - Five lanes, drums and cymbals as distinct lanes
  - Three drums, two cymbals

These all unfortunately share the same track, and there are no specifications to allow explicitly distinguishing between different types of drum tracks by track name, so the track type must be determined through heuristics.

### Determining Track Type

The type of a Drums track can be determined using a process such as the following:

1. Check for the `pro_drums` and `five_lane_drums` tags in the song.ini.
   - If one is present, and set to true, force the drums track to be parsed as that type.
   - If neither are present, fall back to the note type detection.
   - Both being set to true is an invalid state, and either should not be accepted, or one is preferred over the other.
2. Check the chart for notes that indicate the type:
   - If cymbal markers are present, it's a 4-lane Pro track.
   - If the 5-lane green note is present, or if notes are sustained, it's a 5-lane track.
   - If neither 5-lane nor Pro are detected, fall back to standard 4-lane.
   - If both are detected, it may be preferable to prioritize Pro over 5-lane.

If both 5-lane and Pro Drums are detected, it may be preferable to prioritize Pro over 5-lane.

### Track Type Conversions

Here are some suggested conversions between different drum types:

- 5-lane to 4-lane Pro:

| 5-lane | 4-lane Pro    |
| :----- | :---------    |
| Red    | Red           |
| Yellow | Yellow cymbal |
| Blue   | Blue tom      |
| Orange | Green cymbal  |
| Green  | Green tom     |
| O + G  | G cym + B tom |

- 4-lane Pro to 5-lane:

| 4-lane Pro    | 5-lane |
| :---------    | :----- |
| Red           | Red    |
| Yellow cymbal | Yellow |
| Yellow tom    | Blue   |
| Blue cymbal   | Orange |
| Blue tom      | Blue   |
| Green cymbal  | Orange |
| Green tom     | Green  |
| Y tom + B tom | R + B  |
| B cym + G cym | Y + O  |

## Track Names

- `PART DRUMS` - Standard 4-Lane, 4-Lane Pro, and 5-Lane Drums
- `PART DRUM` - Alternate track to `PART DRUMS` that FoFiX supports
- `PART DRUMS_2X` - RBN 2x Kick drums chart
  - This shouldn't normally be present, it's intended for more easily creating 1x kick and 2x kick versions of a song since Rock Band doesn't have proper 2x kick support.
- `PART REAL_DRUMS_PS` - Phase Shift's Real Drums

## Track Notes

| MIDI Note | Description                                  |
| :-------: | :----------                                  |
| Markers   |                                              |
| 127       | 2-lane roll marker                           |
| 126       | 1-lane roll marker                           |
| 124       | Fill/Big Rock Ending marker 1                |
| 123       | Fill/Big Rock Ending marker 2                |
| 122       | Fill/Big Rock Ending marker 3                |
| 121       | Fill/Big Rock Ending marker 4                |
| 120       | Fill/Big Rock Ending marker 5                |
| 116       | Star Power/Overdrive marker                  |
| 112       | Green tom marker                             |
| 111       | Blue tom marker                              |
| 110       | Yellow tom marker                            |
| 109       | Flam marker                                  |
| 106       | Player 2 versus phrase marker                |
| 105       | Player 1 versus phrase marker                |
| 103       | Solo marker                                  |
|           |                                              |
| Expert    |                                              |
| 101       | Expert 5-lane Green (5th lane)               |
| 100       | Expert 4-lane Green/5-lane Orange (4th lane) |
| 99        | Expert Blue (3rd lane)                       |
| 98        | Expert Yellow (2nd lane)                     |
| 97        | Expert Red (1st lane)                        |
| 96        | Expert Kick                                  |
| 95        | Expert+ Kick                                 |
|           |                                              |
| Hard      |                                              |
| 89        | Hard 5-lane Green (5th lane)                 |
| 88        | Hard 4-lane Green/5-lane Orange (4th lane)   |
| 87        | Hard Blue (3rd lane)                         |
| 86        | Hard Yellow (2nd lane)                       |
| 85        | Hard Red (1st lane)                          |
| 84        | Hard Kick                                    |
|           |                                              |
| Medium    |                                              |
| 77        | Medium 5-lane Green (5th lane)               |
| 76        | Medium 4-lane Green/5-lane Orange (4th lane) |
| 75        | Medium Blue (3rd lane)                       |
| 74        | Medium Yellow (2nd lane)                     |
| 73        | Medium Red (1st lane)                        |
| 72        | Medium Kick                                  |
|           |                                              |
| Easy      |                                              |
| 65        | Easy 5-lane Green (5th lane)                 |
| 64        | Easy 4-lane Green/5-lane Orange (4th lane)   |
| 63        | Easy Blue (3rd lane)                         |
| 62        | Easy Yellow (2nd lane)                       |
| 61        | Easy Red (1st lane)                          |
| 60        | Easy Kick                                    |

### 4-Lane Note Mechanics

4-lane notes are cymbals by default in .mid. Toms are marked using the tom marker notes. It is impossible to have a cymbal and tom of the same color on the same tick, and red notes do not have tom markers and are always toms.

Expert+ / 2x kick notes are carried over from 5-lane. They are used for fast kicks that require a second pedal to play. 2x Kick notes should be opt-in, as they exist to allow someone with a single pedal to play a chart that would otherwise require two. When enabled, they are equivalent to normal kicks.

Accent and ghost notes are also carried over from 5-lane. They are notes that notate relatively louder or quieter notes within the section. They are marked by setting notes to a velocity of 127 (accent) or 1 (ghost), and they must be enabled through the `[ENABLE_CHART_DYNAMICS]` text event for legacy compatibilty reasons.

### 4-Lane Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

Roll lanes are used to make imprecise/indiscernible rhythms such as drum rolls or cymbal swells easier to play. They prevent overhitting and only require you to hit faster than a certain threshold to hit the charted notes. This threshold can be either a static time threshold, or it can be based off of the charted notes.

- Roll lanes are not inherently specific to any lanes, rather they are applied dynamically to a lane based on the notes that are present, excluding kicks.
- There are two variants of roll lanes:
  - Single-lane rolls are used for drum or cymbal rolls that only apply to a single lane at a time.
  - Double-lane rolls are used when alternating between two different lanes.
- If a single-lane roll starts on a chord (excluding kicks), the next non-chord note determines which lane the roll goes on.
- Roll lanes only apply to Expert unless they are marked at a velocity between 50-41, in which case they apply to Hard as well.

Fill markers are used to mark phrases in which a player can activate Star Power. These phrases can either replace the notes within the phrase with a freestyle phrase, or they can keep the existing notes and just mark the end of the phrase as an activation point (referred to as "static" fills).

- With the freestyle phrase option, a special activation note is placed at the end of the phrase. This note can require some hits in the freestyle phrase in order to appear.
- With the static phrase option, either one of the existing notes in the phrase can be marked as an activation note, or the notes at the end can be replaced with a specific activation note.
- Note that some charts may not have all 5 of the fill notes marked, notably charts exported to .mid with older versions of Moonscraper.

Flam markers are used to mark single notes as flams. Marked notes can either require two hits to hit (that is, require that they be played as a flam), or be converted into Rock Band-style flams, where one note is turned into two notes right next each other.

- The specialized note method has the advantage of being accurate to which drum is being flammed, but can be cheesed by double-stroking with one stick.
- The Rock Band-style method has the advantage of forcing the motion of a flam, but isn't particularly accurate to which drum is being flammed. It also requires either using the same conversion for two different colors, or making one of the conversions not have the original note as part of the chord.

The versus phrases designate a section of the chart to be played by a specific player in certain 2-player versus modes. During player 1 phrases, only player 1 plays the chart, and during player 2 phrases, only player 2 plays the chart. Both phrases can occur at the same time to make both players play at the same time.

Big Rock Endings (BREs) are freestyle phrases at the end of songs with so-called "big rock endings" where the band rocks out for a period of time before playing their final notes. These freestyle phrases let players gain bonus points by playing whatever they want during the phrase. Following the phrase is at least one note, which the player must hit in order to earn those bonus points. If they miss or overstrum, they lose the points.

- The distinction between fills and BREs is that BREs are started by placing a `[coda]` text event on the `EVENTS` track.
- The duration of the BRE markers determines how long the freestyle section will last. All 5 BRE notes must be used to mark the phrase.
- After the freestyle phrase, there must be at least one note for the player to play. If no notes are present, the player cannot get their score.
- Notes still need to be charted under Guitar and Bass BRE sections for the purpose of character animations. In general, notes should still be charted regardless because not every game supports BREs.
- [More than one freestyle phrase may be placed during a BRE](https://youtube.com/watch?v=2iegD-LR8RE&t=208) (warning: this video is loud and the chart is awful), though this was probably never intended as it was never documented, and it might not work in some games. This is *very* edge-case, and almost no charts will have it.

### 5-Lane Note Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

In 5-lane, red, blue, and green notes are toms, and yellow and orange notes are cymbals. While in Guitar Hero they were not distinguished visually, this is how they are charted, and on the drum kit peripheral this is how they are shaped.

Drum sustains are used for imprecise/indiscernible rhythms such as drum rolls or cymbal swells. They prevent overhitting and require you to hit faster than a certain threshold (around 4 hits per second in GH) to maintain the sustain. Unlike 4-lane roll lanes, kicks can be drum sustains.

Expert+ / 2x kick notes are used for fast kicks that require a second pedal to play. 2x Kick notes should be opt-in, as they exist to allow someone with a single pedal to play a chart that would otherwise require two. When enabled, they are equivalent to normal kicks.

Accent and ghost notes are notes that notate relatively louder or quieter notes within the section. Hitting them correctly is optional, and typically just grants bonus points.

## Important Text Events

| Event Text                         | Description                         |
| :---------                         | :----------                         |
| `[ENABLE_CHART_DYNAMICS]`          | Enables accent/ghost marking.       |
| `[mix <diff> drums<config><flag>]` | Sets a stem configuration for drums.<br>Can be found both with or without brackets, and with either spaces or underscores. |

### Mix Event Details

- `diff` is a number indicating the difficulty this event applies to:

  | Text | Description |
  | :--: | :---------- |
  | `0`  | Easy        |
  | `1`  | Medium      |
  | `2`  | Hard        |
  | `3`  | Expert      |

- `config` is a number corresponding to a specific stem configuration:

  | Text | Description                                                                  |
  | :--: | :----------                                                                  |
  | `0`  | One stereo stem for the entire kit.<br>Should be used for no stems, as well. |
  | `1`  | Mono kick, mono snare, stereo other.                                         |
  | `2`  | Mono kick, stereo snare, stereo other.                                       |
  | `3`  | Stereo kick, snare, and other.                                               |
  | `4`  | Mono kick, stereo other.                                                     |
  | `5`  | Stereo kick, snare, toms, and cymbals.<br>Not part of the RBN docs, rather, it is [defined as a community standard](https://strikeline.myjetbrains.com/youtrack/issue/CH-43) based on the GH drums stem layout. |

  - This number should be consistent throughout the chart.

- `flag` is an optional string added to the end of the event that applies a modification outside of setting up the mix:

  | Text         | Description |
  | :--:         | :---------- |
  | `d`          | On non-Pro, swaps the the snare and cymbal+tom/other stems, such that snare is activated by yellow, and cymbal+tom/other are activated by red.<br>On Pro Drums, flips yellow cymbal/yellow tom and red notes to restore proper playing of the part.<br>Used in sections that require alternating the left and right hands quickly on the hi-hat, which are typically charted swapped to allow non-Pro players to play more comfortably. |
  | `dnoflip`    | Swaps the snare and kit/cymbal and tom stems on both non-Pro and Pro Drums, without swapping red and yellow in Pro Drums.<br>Used in sections where notes should not be flipped in Pro Drums, but the snare and kit/cymbal stems should still be swapped.
  | `easy`       | Unmutes the tom and cymbal stems on Easy.<br>Used in sections where there are no tom or cymbal notes to unmute the kit stem or tom/cymbal stems. |
  | `easynokick` | Unmutes the cymbal+tom/other stem on Easy.<br>Used in sections where there are no kick notes to unmute the kick stem. |

  - Modifications reset upon reaching a new mix event. If there is no flag supplied, all modifications are reset and no new modifications are applied.
