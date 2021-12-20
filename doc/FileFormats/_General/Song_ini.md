# song.ini

song.ini files are used for storing song metadata. Originally they were used for .mid, but Clone Hero also extends this to .chart files.

## Table of Contents

- [Basic Structure](#basic-structure)
  - [Sections](#sections)
  - [Tags](#tags)
  - [Comments](#comments)
- [Known Tags](#known-tags)
  - [Song/Chart Metadata](#songchart-metadata)
  - [Track Difficulties](#track-difficulties)
  - [Chart Properties](#chart-properties)
  - [Images/Other Resources](#imagesother-resources)
  - [Miscellaneous](#miscellaneous)
- [Resources](#resources)

## Basic Structure

### Sections

Sections are a header line containing only a string encapsulated in square brackets:

`[SectionName]`

Section names are not necessarily case-sensitive.

A song.ini file should contain a `[song]`/`[Song]` section header.

### Tags

Tags are a key-value pair that store each metadata entry, stored in this general format:

`<Key> = <Value>`

- `Key` is the name of the value.
- `Value` is the stored value. The data type depends on the key. Some tags may contain multiple values.

The equals sign may, but is not required to be, padded by spaces, but it is recommended to pad it as not all games/programs support song.ini files without the padding (namely Phase Shift).

Tags can be listed in any order, and may have inconsistent capitalization to the tag names listed below.

Value type info:

- `number` fields are whole numbers typically written in base 10.
  - A value of -1 may be used in some number fields to indicate that there is no value set.
- `float` fields allow decimal places and are also written in base 10.
- `string` fields typically, but don't always, use quotes. Strings that don't use quotes will be marked as such.
- `boolean` fields can be either `True`/`False` (or `true`/`false`), or `0`/`1` (only a couple booelan tags do this, but simplest to specify it for all).
- `file name` fields

Specialized value types:

- `Pro Guitar tuning` - 4 to 6 numbers representing semitone differences from standard tuning, and an optional string for the tuning name
  - Examples:
  - `0 0 0 0 0 0 "Standard tuning"`
  - `0 2 5 7 10 10`
  - `-2 0 0 0 "Drop D tuning"`
  - `0 0 0 0 0`
- `MIDI note`/`MIDI velocity` - Any MIDI note/velocity number. Notes go from 0 to 127, velocities go from 1 to 127.

### Comments

Comments are not fully standardized, but may appear in some files.

Comments are typically delimited using either a semicolon `;` or a hash `#`. Some .ini file implementations only support one or the other. Additionally, some implementations only support comments that are a line of their own, while others may allow trailing comments.

## Known Tags

There are *many* tags in existence, so listing them all in one place is rather difficult. Still, this should be a pretty extensive list of them.

Note that for string tags, there may be [formatting tags](http://digitalnativestudios.com/textmeshpro/docs/rich-text/) present, such as `<color=#00FF00>`. Make sure to trim these out after parsing if you don't want to use them.

In the descriptions, game names in parentheses indicate where the tag originates from if it is either specific to a game, is more specialized, or is a legacy tag. FoFiX stands for Frets on Fire X, PS stands for Phase Shift, and CH stands for Clone Hero.

### Song/Chart Metadata

| Tag Name             | Description                                                                                   | Data type   |
| :-------             | :----------                                                                                   | :-------:   |
| `name`               | Title of the song.                                                                            | string      |
| `artist`             | Artist(s) or band(s) behind the song.                                                         | string      |
| `album`              | Title of the album the song is featured in.                                                   | string      |
| `genre`              | Genre of the song.                                                                            | string      |
| `sub_genre`          | Sub-genre for the song.                                                                       | string      |
| `year`               | Year of the song’s release.                                                                   | string      |
|                      |                                                                                               |             |
| `charter`            | Community member responsible for charting the song.                                           | string      |
| `frets`              | (FoFiX) Same as `charter`.                                                                    | string      |
|                      |                                                                                               |             |
| `album_track`        | Track number of the song within the album it's from.                                          | number      |
| `track`              | (PS) Same as `album_track`.                                                                   | number      |
| `playlist_track`     | Track number of the song within the playlist/setlist it's from.                               | number      |
|                      |                                                                                               |             |
| `song_length`        | Length of the song's audio in milliseconds.<br>Only meant as metadata, don't rely on this.    | number      |
| `preview_start_time` | Timestamp in milliseconds where the song preview starts.                                      | number      |
| `preview_end_time`   | (May not actually exist) Timestamp in milliseconds that the preview should stop at.           | number      |
| `preview`            | (PS) Two timestamps in milliseconds for preview start and end time.<br>Example: `55000 85000` | two numbers |
| `video_start_time`   | Timestamp in milliseconds where playback of an included video will start. Can be negative.    | number      |
| `video_end_time`     | Timestamp in milliseconds where playback of an included video background will end.            | number      |
|                      |                                                                                               |             |
| `tags`               | (FoFiX) Name is unclear, known use is to mark a song as a cover and have `As made famous by` display next to the artist name.<br>Only known valid value is `cover`. | string |
| `cassettecolor`      | (FoFiX) Hex color to use in the song screen for the cassette.                                 | hex color   |
| `modchart`           | (CH) Indicates if this song is a modchart. For sorting purposes only.                         | boolean     |
| `lyrics`             | (FoFiX) Indicates if the song has lyrics or not.                                              | boolean     |
| `playlist`           | (CH) Playlist that the song should show up in.                                                | string      |
| `sub_playlist`       | (CH) Sub-playlist that the song should show up in.                                            | string      |
|                      |                                                                                               |             |
| `loading_phrase`     | Flavor text for this song, usually shown after picking the song or during loading.            | string      |

### Track Difficulties

| Tag Name              | Description                     | Data type |
| :-------              | :----------                     | :-------: |
| `diff_band`           | Overall difficulty of the song. | number    |
|                       |                                 |           |
| `diff_guitar`         | Lead Guitar difficulty.         | number    |
| `diff_guitarghl`      | 6-Fret Lead difficulty.         | number    |
| `diff_guitar_coop`    | Guitar Co-op difficulty.        | number    |
| `diff_guitar_real`    | Pro Guitar difficulty.          | number    |
| `diff_guitar_real_22` | Pro Guitar 22-fret difficulty.  | number    |
| `diff_rhythm`         | Rhythm Guitar difficulty.       | number    |
|                       |                                 |           |
| `diff_bass`           | Bass Guitar difficulty.         | number    |
| `diff_bassghl`        | 6-Fret Bass difficulty.         | number    |
| `diff_bass_real`      | Pro Bass difficulty.            | number    |
| `diff_bass_real_22`   | Pro Bass 22-fret difficulty.    | number    |
|                       |                                 |           |
| `diff_drums`          | Drums difficulty.               | number    |
| `diff_drums_real`     | Pro Drums difficulty.           | number    |
| `diff_drums_real_ps`  | Drums Real difficulty.          | number    |
|                       |                                 |           |
| `diff_keys`           | Keys difficulty.                | number    |
| `diff_keys_real`      | Pro Keys difficulty.            | number    |
| `diff_keys_real_ps`   | Keys Real difficulty.           | number    |
|                       |                                 |           |
| `diff_vocals`         | Vocals difficulty.              | number    |
| `diff_vocals_harm`    | Harmonies difficulty.           | number    |
|                       |                                 |           |
| `diff_dance`          | Dance difficulty.               | number    |

### Chart Properties

These tags affect the parsing or configuration of a chart.

| Tag Name                     | Description                                                                              | Data type |
| :-------                     | :----------                                                                              | :-------: |
| `delay`                      | Chart offset time in milliseconds.<br>Higher = later notes. Can be negative.             | number    |
|                              |                                                                                          |           |
| `pro_drums`                  | Forces the Drums track to be Pro Drums.                                                  | boolean   |
| `pro_drum`                   | (FoFiX) Same as `pro_drums`?                                                             | boolean   |
| `five_lane_drums`            | Forces the Drums track to be 5-lane.                                                     | boolean   |
| `drum_fallback_blue`         | (PS) "Sets 5 to 4 Lane Drums Fallback Note" (No solid idea what this means lol)          | boolean   |
|                              |                                                                                          |           |
| `sustain_cutoff_threshold`   | Overrides the default sustain cutoff threshold with the specified number of ticks.       | number    |
| `hopo_frequency`             | Overrides the natural HOPO threshold with the specified number of ticks.                 | number    |
| `hopofreq`                   | (FoFiX) Overrides the natural HOPO threshold using [numbers from 0 to 5](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L1309).<br>0 = 1/24th, 1 = 1/16th, 2 = 1/12th, 3 = calculates to 1/9th but 1/8th is more reasonable, 4 = 1/6th, 5 = 1/4th<br>Not recommended for new charts, use `hopo_frequency` instead. | number |
| `eighthnote_hopo`            | (FoFiX) Overrides the natural HOPO threshold to be a 1/8th step.<br>Not recommended for new charts, use `hopo_frequency` instead. | boolean |
| `multiplier_note`            | Overrides the Star Power phrase MIDI note for .mid charts.                               | MIDI note |
| `star_power_note`            | (PS) Same as above.                                                                      | MIDI note |
|                              |                                                                                          |           |
| `end_events`                 | (CH) Overrides whether or not end events in the chart will be respected.                 | boolean   |
|                              |                                                                                          |           |
| `early_hit_window_size`      | (FoFiX) Sets the "early hit window" size.<br>Valid values are "none", "half", or "full". | string    |
|                              |                                                                                          |           |
| `sysex_slider`               | (PS) Indicates if the chart uses SysEx events for sliders/tap notes.                     | boolean   |
| `sysex_high_hat_ctrl`        | (PS) Indicates if the chart uses SysEx events for Drums Real hi-hat pedal control.       | boolean   |
| `sysex_rimshot`              | (PS) Indicates if the chart uses SysEx events for Drums Real rimshot hits.               | boolean   |
| `sysex_open_bass`            | (PS) Indicates if the chart uses SysEx events for open notes.                            | boolean   |
| `sysex_pro_slide`            | (PS) Indicates if the chart uses SysEx events for Pro Guitar/Bass slide directions.      | boolean   |
|                              |                                                                                          |           |
| `guitar_type`                | (PS) Sample sound set for Guitar.                                                        | number    |
| `bass_type`                  | (PS) Sample sound set for Bass.                                                          | number    |
| `kit_type`                   | (PS) Sample sound set for Drums.                                                         | number    |
| `keys_type`                  | (PS) Sample sound set for Keyboard.                                                      | number    |
| `dance_type`                 | (PS) Unsure (sets the type of pad for Part Dance?)                                       | number    |
| `vocal_gender`               | (PS) Specifies a voice type for the singer (either "male" or "female").                  | string    |
|                              |                                                                                          |           |
| `real_guitar_tuning`         | Specifies a tuning for 17-fret Pro Guitar.                                               | Pro Guitar tuning |
| `real_guitar_22_tuning`      | Specifies a tuning for 22-fret Pro Guitar.                                               | Pro Guitar tuning |
| `real_bass_tuning`           | Specifies a tuning for 17-fret Pro Bass.                                                 | Pro Guitar tuning |
| `real_bass_22_tuning`        | Specifies a tuning for 22-fret Pro Bass.                                                 | Pro Guitar tuning |
|                              |                                                                                          |           |
| `real_keys_lane_count_right` | Specifies the number of lanes for the right hand in Real Keys.                           | number    |
| `real_keys_lane_count_left`  | Specifies the number of lanes for the left hand in Real Keys.                            | number    |

### Images/Other Resources

| Tag Name        | Description                                | Data type |
| :-------        | :----------                                | :-------: |
| `icon`          | Name of an icon to display for this song.<br>Included in either the chart folder or the game the chart was made for, or sourced from [this repository of icons](https://gitlab.com/clonehero/sources). | string |
|                 |                                            |           |
| `background`    | Name/path for a background image file.     | file path |
| `video`         | Name/path for a background video file.     | file path |
| `video_loop`    | Sets whether or not the video should loop. | boolean   |
| `cover`         | Name/path for a cover image file.          | file path |
| `link_name_a`   | Name for banner A.                         | string    |
| `banner_link_a` | Link that clicking banner A will open.     | string    |
| `link_name_b`   | Name for banner B.                         | string    |
| `banner_link_b` | Link that clicking banner B will open.     | string    |

### Miscellaneous

| Tag Name           | Description                                                                             | Data type     |
| :-------           | :----------                                                                             | :-------:     |
| `rating`           | (PS) Player's rating of the song?                                                       | number        |
| `scores`           | FoF(iX) high score data.                                                                | string        |
| `scores_ext`       | FoF(iX) additional score data.                                                          | string        |
| `count`            | FoF(iX) play count.                                                                     | number        |
|                    |                                                                                         |               |
| `version`          | (FoF(iX)) Version number for the song.                                                  | number        |
| `tutorial`         | (FoF(iX)) Indicates if is a tutorial song and should be hidden from quickplay.          | boolean       |
| `boss_battle`      | (FoF(iX)?) Indicates if this song is a boss battle.                                     | boolean?      |
|                    |                                                                                         |               |
| `unlock_id`        | (FoF(iX)) Career ID of this song. See [FoFiX_Careers.md](FoFiX_Careers.md) for details. | string        |
| `unlock_require`   | (FoF(iX)) The career ID that must be completed to unlock this song.                     | string        |
| `unlock_text`      | (FoF(iX)) Text to display if the song is locked.                                        | string        |
| `unlock_completed` | (FoF(iX)) Indicates if the song is unlocked. (*not* a boolean value)                    | number        |
|                    |                                                                                         |               |
| `eof_midi_import_drum_accent_velocity` | (Editor on Fire) Sets a velocity number for drums accent notes.     | MIDI velocity |
| `eof_midi_import_drum_ghost_velocity`  | (Editor on Fire) Sets a velocity number for drums ghost notes.      | MIDI velocity |

## Resources

These tags are sourced from existing song.ini files and from these resources:

- [grishhung's song.ini guide](https://docs.google.com/document/d/1ped13di4LqDqhaxbCgZEMUoqnyc3gOy3Bw1FCg58FPI/edit#)
- [Phase Shift forums: Song Standard Advancements](https://dwsk.proboards.com/thread/404/song-standard-advancements)
- [FoFiX docs: Songs](https://fofix.readthedocs.io/en/latest/users/songs.html)
- [FoFiX source code: song.py](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py)
