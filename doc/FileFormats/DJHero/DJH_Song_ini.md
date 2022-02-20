# song.ini DJ Hero Metadata Specifications (Proposal)

This document details a proposal for metadata to be used in DJ Hero chart song.ini files.

## Table of Contents

- [Song/Chart Metadata](#songchart-metadata)
- [Deck Speed](#deck-speed)
- [Chart Difficulty](#chart-difficulty)
- [Miscellaneous](#miscellaneous)
  - [Megamixes](#megamixes)
  - [Other Optional](#other-optional)
- [Optional misc toggles](#optional-misc-toggles)

## Song/Chart Metadata

| Tag Name             | Description                                    | Data type |
| :-------             | :----------                                    | :-------: |
| `name`               | First song's name.                             | string    |
| `artist`             | First song's artist.                           | string    |
| `album`              | First song's album.                            | string    |
| `genre`              | First song's genre.                            | string    |
| `year`               | First song's release year.                     | number    |
|                      |                                                |           |
| `name2`              | Second song's name.                            | string    |
| `artist2`            | Second song's artist.                          | string    |
| `album2`             | Second song's album.                           | string    |
| `genre2`             | Second song's genre.                           | string    |
| `year2`              | Second song's release year.                    | number    |
|                      |                                                |           |
| `dj`                 | Person who made the mix.                       | string    |
| `charter`            | Person who made the chart.                     | string    |
|                      |                                                |           |
| `megamix_track`      | Index of the chart within a megamix.           | number    |
| `megamix_name`       | Name of the megamix that the chart is part of. | string    |
|                      |                                                |           |
| `song_length`        | Song length, in milliseconds.                  | number    |
| `preview_start_time` | Menu preview start time, in milliseconds.      | number    |
| `preview_end_time`   | Menu preview start time, in milliseconds.      | number    |

## Deck Speed

In DJH, deck speed is tied to BPM, so BPM is mandatory.
A multiplier is used to tune the deckspeed per difficulty. It's usually an decimal number between 1 and 4.

| Tag Name             | Description                                    | Data type |
| :-------             | :----------                                    | :-------: |
| `bpm`                | The chart's BPM, used for deck speed purposes. | float     |
| `deckspeed_beginner` | Deckspeed multiplier for Beginner.             | float     |
| `deckspeed_easy`     | Deckspeed multiplier for Easy.                 | float     |
| `deckspeed_medium`   | Deckspeed multiplier for Medium.               | float     |
| `deckspeed_hard`     | Deckspeed multiplier for Hard.                 | float     |
| `deckspeed_expert`   | Deckspeed multiplier for Expert.               | float     |

## Chart Difficulty

| Tag Name               | Description                                                   | Data type |
| :-------               | :----------                                                   | :-------: |
| `track_complexity`     | Overall difficulty<br>Typically 0 to 100 but can be higher.   | float?    |
| `tap_complexity`       | Tap difficulty<br>Typically 0 to 100 but can be higher.       | float?    |
| `crossfade_complexity` | Crossfade difficulty<br>Typically 0 to 100 but can be higher. | float?    |
| `scratch_complexity`   | Scratch difficulty<br>Typically 0 to 100 but can be higher.   | float?    |

## Miscellaneous

### Megamixes

Megamixes can have extended intros that include notes that aren't part of the regular chart.

| Tag Name                | Description                                                    | Data type |
| :-------                | :----------                                                    | :-------: |
| `megamix_hasintro`      | Indicates whether or not the megamix has an intro.             | boolean   |
| `megamix_highwayoffset` | Milliseconds before the chart's beginning to start the megamix | number    |

Megamixes can contain "megamix transition" charts that don't appear in the main setlist

| Tag Name             | Description                                                   | Data type |
| :-------             | :----------                                                   | :-------: |
| `megamix_transition` | Indicates whether or not this chart has a megamix transition. | boolean   |

### Other Optional

In DJH, when loading the chart and venue, a random mix is played in the background.
If the game uses this mix for the background, the mix will play at this timestamp instead of from the beginning.

| Tag Name         | Description                                 | Data type |
| :-------         | :----------                                 | :-------: |
| `env_start_time` | Background mix start time, in milliseconds. | number    |

## Optional misc toggles

| Tag Name     | Description                                                                           | Data type |
| :-------     | :----------                                                                           | :-------: |
| `battle_mix` | Marks if this is a DJH 2 battle mix.                                                  | boolean   |
| `guitar_mix` | Marks if this is a DJH 1 DJ + guitar mix.                                             | boolean   |
| `menu_music` | Marks if the song plays in the main menu.<br>Only 33 menu tracks are allowed in DJH2. | boolean   |
