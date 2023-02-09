# Known song.ini Tags

There are *many* tags in existence, so listing them all in one place is rather difficult. Still, this should be a pretty extensive list of them.

In the descriptions, game names in parentheses indicate where the tag originates from if it is either specific to a game, is more specialized, or is a legacy tag. FoFiX stands for Frets on Fire X, PS stands for Phase Shift, and CH stands for Clone Hero.

## Table of Contents

- [Song/Chart Metadata](#songchart-metadata)
- [Track Difficulties](#track-difficulties)
- [Chart Properties](#chart-properties)
- [Images/Other Resources](#imagesother-resources)
- [Miscellaneous](#miscellaneous)
- [Resources](#resources)

## Song/Chart Metadata

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
| `frets`              | Same as `charter`.                                                                            | string      |
| `version`            | Version number for the song.                                                                  | number      |
|                      |                                                                                               |             |
| `album_track`        | Track number of the song within the album it's from.                                          | number      |
| `track`              | Same as `album_track`.                                                                        | number      |
| `playlist_track`     | Track number of the song within the playlist/setlist it's from.                               | number      |
|                      |                                                                                               |             |
| `song_length`        | Length of the song's audio in milliseconds.<br>Metadata only, don't rely on this.             | number      |
| `preview_start_time` | Timestamp in milliseconds where the song preview starts.                                      | number      |
| `preview_end_time`   | Timestamp in milliseconds that the preview should stop at.                                    | number      |
| `preview`            | (PS) Two timestamps in milliseconds for preview start and end time.<br>Example: `55000 85000` | two numbers |
|                      |                                                                                               |             |
| `modchart`           | Indicates if this song is a modchart.<br>Metadata only, don't rely on this.                   | boolean     |
| `lyrics`             | Indicates if the song has lyrics or not.<br>Metadata only, don't rely on this.                | boolean     |
|                      |                                                                                               |             |
| `loading_phrase`     | Flavor text for this song, usually shown after picking the song or during loading.            | string      |
| `playlist`           | (CH) Playlist that the song should show up in.                                                | string      |
| `sub_playlist`       | (CH) Sub-playlist that the song should show up in.                                            | string      |
| `cassettecolor`      | (FoFiX) Hex color to use in the song screen for the cassette.                                 | hex color   |
| `tags`               | (FoFiX) Miscellaneous tags for the chart.<br>Only known valid value is `cover`. | string |

## Track Difficulties

| Tag Name              | Description                     | Data type |
| :-------              | :----------                     | :-------: |
| `diff_band`           | Overall difficulty of the song. | number    |
|                       |                                 |           |
| `diff_guitar`         | Lead Guitar difficulty.         | number    |
| `diff_guitarghl`      | 6-Fret Lead difficulty.         | number    |
| `diff_guitar_coop`    | Guitar Co-op difficulty.        | number    |
| `diff_guitar_coop_ghl`| 6-Fret Guitar Co-op difficulty. | number    |
| `diff_guitar_real`    | Pro Guitar difficulty.          | number    |
| `diff_guitar_real_22` | Pro Guitar 22-fret difficulty.  | number    |
| `diff_rhythm`         | Rhythm Guitar difficulty.       | number    |
| `diff_rhythm_ghl`     | 6-Fret Rhythm Guitar difficulty.| number    |
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

## Chart Properties

These tags affect the parsing or configuration of a chart.

| Tag Name                     | Description                                                                              | Data type |
| :-------                     | :----------                                                                              | :-------: |
| `delay`                      | Chart offset time in milliseconds.<br>Higher = later notes. Can be negative.             | number    |
|                              |                                                                                          |           |
| `pro_drums`                  | Forces the Drums track to be Pro Drums.                                                  | boolean   |
| `pro_drum`                   | (FoFiX) Same as `pro_drums`?                                                             | boolean   |
| `five_lane_drums`            | Forces the Drums track to be 5-lane.                                                     | boolean   |
| `drum_fallback_blue`         | (PS) "Sets 5 to 4 Lane Drums Fallback Note" (No solid idea what this means lol)          | boolean   |
| `tutorial`                   | (FoF(iX)) Marks a song as a tutorial and hides it from Quickplay.                        | boolean   |
| `boss_battle`                | (FoF(iX)?) Marks a song as a boss battle.                                                | boolean?  |
|                              |                                                                                          |           |
| `sustain_cutoff_threshold`   | Overrides the default sustain cutoff threshold with a specified value in ticks.          | number    |
| `hopo_frequency`             | Overrides the default HOPO threshold with a specified value in ticks.<br>Overrides `hopofreq` and `eighthnote_hopo`.     | number    |
| `hopofreq`                   | (FoFiX) Overrides the natural HOPO threshold using [numbers from 0 to 5](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L1309).<br>No clue how it actually behaves, but here's my interpretation of the code based on what would be reasonable: 0 = 1/24th, 1 = 1/16th, 2 = 1/12th, 3 = 1/8th, 4 = 1/6th, 5 or higher = 1/4th.<br>Not recommended for new charts, use `hopo_frequency` instead. | number |
| `eighthnote_hopo`            | Sets the HOPO threshold to be a 1/8th step.<br>Not recommended for new charts, use `hopo_frequency` instead. | boolean |
| `multiplier_note`            | Overrides the note number for Star Power on 5-Fret Guitar in .mid charts.<br>Valid values are 103 and 116. | MIDI note |
| `star_power_note`            | Same as above.                                                                           | MIDI note |
|                              |                                                                                          |           |
| `end_events`                 | Sets whether or not end events in the chart will be respected.<br>Respecting this tag is only necessary if automatic detection of end events occuring before the last note in the song is not possible/feasible. | boolean   |
|                              |                                                                                          |           |
| `early_hit_window_size`      | (FoFiX) Sets the "early hit window" size.<br>Valid values are "none", "half", or "full". | string    |
|                              |                                                                                          |           |
|                              | Implementing the following tags is not recommended , as disabling these makes the chart play differently than intended and Phase Shift mainly added these as a way to avoid pre-processing. | |
| `sysex_slider`               | (PS) Enables .mid SysEx events for guitar sliders/tap notes.                             | boolean   |
| `sysex_high_hat_ctrl`        | (PS) Enables .mid SysEx events for Real Drums hi-hat pedal control.                      | boolean   |
| `sysex_rimshot`              | (PS) Enables .mid SysEx events for Real Drums rimshot hits.                              | boolean   |
| `sysex_open_bass`            | (PS) Enables .mid SysEx events for guitar open notes.                                    | boolean   |
| `sysex_pro_slide`            | (PS) Enables .mid SysEx events for Pro Guitar/Bass slide directions.                     | boolean   |
|                              |                                                                                          |           |
| `guitar_type`                | (PS) Sound sample set index for guitar.                                                  | number    |
| `bass_type`                  | (PS) Sound sample set index for bass.                                                    | number    |
| `kit_type`                   | (PS) Sound sample set index for drums.                                                   | number    |
| `keys_type`                  | (PS) Sound sample set index for keys.                                                    | number    |
| `dance_type`                 | (PS) Sound sample set index for dance.                                                   | number    |
| `vocal_gender`               | Specifies a voice type for the singer (either "male" or "female").                       | string    |
|                              |                                                                                          |           |
| `real_guitar_tuning`         | Specifies a tuning for 17-fret Pro Guitar.                                               | Pro Guitar tuning |
| `real_guitar_22_tuning`      | Specifies a tuning for 22-fret Pro Guitar.                                               | Pro Guitar tuning |
| `real_bass_tuning`           | Specifies a tuning for 17-fret Pro Bass.                                                 | Pro Guitar tuning |
| `real_bass_22_tuning`        | Specifies a tuning for 22-fret Pro Bass.                                                 | Pro Guitar tuning |
|                              |                                                                                          |           |
| `real_keys_lane_count_right` | Specifies the number of lanes for the right hand in Real Keys.                           | number    |
| `real_keys_lane_count_left`  | Specifies the number of lanes for the left hand in Real Keys.                            | number    |

## Images/Other Resources

| Tag Name        | Description                                | Data type |
| :-------        | :----------                                | :-------: |
| `icon`          | Name of an icon image to display for this song, not including the file extension.<br>Included in either the chart folder or the game the chart was made for, or sourced from [this repository of icons](https://gitlab.com/clonehero/sources). | string |
| `background`    | Name/path for a background image file.     | file name |
| `video`         | Name/path for a background video file.     | file name |
| `video_loop`    | Sets whether or not the video should loop. | boolean   |
| `video_start_time` | Timestamp in milliseconds where playback of an included video will start. Can be negative.<br>This tag controls the time relative to the video, not relative to the chart. Negative values will delay the video, positive values will make the video be at a further point in when the chart starts. | number      |
| `video_end_time`   | Timestamp in milliseconds where playback of an included video will end.<br>This is assumed to also be relative to the video, not the chart. | number      |
| `cover`         | Name/path for a cover image file.          | file name |
| `link_name_a`   | Name for banner A.                         | string    |
| `banner_link_a` | Link that clicking banner A will open.     | string    |
| `link_name_b`   | Name for banner B.                         | string    |
| `banner_link_b` | Link that clicking banner B will open.     | string    |

## Miscellaneous

| Tag Name           | Description                                                                             | Data type     |
| :-------           | :----------                                                                             | :-------:     |
| `rating`           | (PS) Player's rating of the song?                                                       | number        |
| `scores`           | FoF(iX) high score data.                                                                | string        |
| `scores_ext`       | FoF(iX) additional score data.                                                          | string        |
| `count`            | FoF(iX) play count.                                                                     | number        |
|                    |                                                                                         |               |
|                    |                                                                                         |               |
|                    | See [FoFiX_Careers.md](../Other/Frets%20on%20Fire%20X/FoFiX_Careers.md) for details on the following. | |
| `unlock_id`        | (FoF(iX)) Career ID of this song.                                                       | string        |
| `unlock_require`   | (FoF(iX)) The career ID that must be completed to unlock this song.                     | string        |
| `unlock_text`      | (FoF(iX)) Text to display if the song is locked.                                        | string        |
| `unlock_completed` | (FoF(iX)) Indicates if the song is unlocked.                                            | number        |
|                    |                                                                                         |               |
| `eof_midi_import_drum_accent_velocity` | (Editor on Fire) Sets a velocity number for drums accent notes.     | MIDI velocity |
| `eof_midi_import_drum_ghost_velocity`  | (Editor on Fire) Sets a velocity number for drums ghost notes.      | MIDI velocity |

## Resources

These tags are sourced from existing song.ini files and from these resources:

- [grishhung's song.ini guide](https://docs.google.com/document/d/1ped13di4LqDqhaxbCgZEMUoqnyc3gOy3Bw1FCg58FPI/edit#)
- [Phase Shift forums: Song Standard Advancements](https://dwsk.proboards.com/thread/404/song-standard-advancements)
- [FoFiX docs: Songs](https://fofix.readthedocs.io/en/latest/users/songs.html)
- [FoFiX source code: song.py](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py)
