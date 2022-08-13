# Guitar Hero 1 and 2 Venue Tracks

These tracks contain events used for venue rendering.

## Table of Contents

- [Guitar Hero 1](#guitar-hero-1)
  - [GH1 Triggers Track](#gh1-triggers-track)
    - [GH1 Triggers Notes](#gh1-triggers-notes)
- [Guitar Hero 2](#guitar-hero-2)
  - [GH2 BAND_BASS Track](#gh2-band_bass-track)
    - [GH2 BAND_BASS Notes](#gh2-band_bass-notes)
    - [GH2 BAND_BASS Text Events](#gh2-band_bass-text-events)
  - [GH2 BAND_DRUM Track](#gh2-band_drum-track)
    - [GH2 BAND_DRUM Notes](#gh2-band_drum-notes)
    - [GH2 BAND_DRUM Text Events](#gh2-band_drum-text-events)
  - [GH2 BAND_SINGER Track](#gh2-band_singer-track)
    - [GH2 BAND_SINGER Notes](#gh2-band_singer-notes)
    - [GH2 BAND_SINGER Text Events](#gh2-band_singer-text-events)
  - [GH2 Triggers Track](#gh2-triggers-track)
    - [GH2 Triggers Notes](#gh2-triggers-notes)

## Guitar Hero 1

### GH1 Triggers Track

The exact purpose of this track is unknown.

#### GH1 Triggers Notes

| MIDI Note | Description |
| :-------: | :---------- |
| 61        | Unknown     | 
| 60        | Unknown     | 

## Guitar Hero 2

### GH2 BAND_BASS Track

This track is not present if the co-op track is Bass.

#### GH2 BAND_BASS Notes

| MIDI Note | Description             |
| :-------: | :----------             |
| 36        | Strum animation trigger |

#### GH2 BAND_BASS Text Events

| Event Text | Description                                |
| :--------- | :----------                                |
| `[idle]`   | Bassist idles during a part with no notes. |
| `[play]`   | Bassist starts playing.                    |

### GH2 BAND_DRUM Track

#### GH2 BAND_DRUM Notes

| MIDI Note | Description  |
| :-------: | :----------  |
| 37        | Crash cymbal |
| 36        | Kick drum    |

#### GH2 BAND_DRUM Text Events

| Event Text       | Description                                       |
| :---------       | :----------                                       |
| `[idle]`         | Drummer idles during a part with no notes.        |
| `[play]`         | Drummer starts playing.                           |
| `[normal_tempo]` | Drummer plays kick drum on first and third beats. |
| `[half_time]`    | Drummer plays kick drum on first beat only.       |
| `[double_time]`  | Drummer plays kick drum in eighth note pattern.   |
| `[allbeat]`      | Drummer plays kick drum in quarter note pattern.  |

### GH2 BAND_SINGER Track

#### GH2 BAND_SINGER Notes

| MIDI Note | Description                                            |
| :-------: | :----------                                            |
| 108       | Mouth open/close<br>Note on = open, note off = closed. |

#### GH2 BAND_SINGER Text Events

| Event Text | Description                                 |
| :--------- | :----------                                 |
| `[idle]`   | Vocalist idles during a part with no notes. |
| `[play]`   | Vocalist starts playing.                    |

### GH2 Triggers Track

This track contains triggers for venue effects and practice mode drum samples.

#### GH2 Triggers Notes

This list is incomplete.

| MIDI Note | Description                                 |
| :-------: | :----------                                 |
| 52        | Venue-specific effect                       |
| 50        | Venue lighting: First keyframe of effect    |
| 49        | Venue lighting: Previous keyframe of effect |
| 48        | Venue lighting: Next keyframe of effect     |
| 26        | Hi-hat practice mode assist sample          |
| 25        | Snare practice mode assist sample           |
| 24        | Kick practice mode assist sample            |
