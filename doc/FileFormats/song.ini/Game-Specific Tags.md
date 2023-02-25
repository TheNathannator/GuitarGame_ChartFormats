# Game-Specific Tags

The tags listed here are all generally specific to certain games. Supporting them is entirely optional, and they are listed mainly for posterity.

In the descriptions, game names in parentheses indicate where the tag originates from. FoFiX stands for Frets on Fire/Frets on Fire X, PS stands for Phase Shift, and CH stands for Clone Hero.

## Song/Chart Metadata

| Tag Name        | Description                                                                                   | Data type   |
| :-------        | :----------                                                                                   | :-------:   |
| `cassettecolor` | (FoFiX) Hex color to use in the song screen for the cassette.                                 | hex color   |
| `tags`          | (FoFiX) Miscellaneous tags for the chart.<br>Only known valid value is `cover`.               | string      |
| `preview`       | (PS) Two timestamps in milliseconds for preview start and end time.<br>Example: `55000 85000` | two numbers |
| `playlist`      | (CH) Playlist that the song should show up in.                                                | string      |
| `sub_playlist`  | (CH) Sub-playlist that the song should show up in.                                            | string      |
| `modchart`      | (CH) Indicates if this song is a modchart.<br>Meant for sorting purposes only.                | boolean     |
| `lyrics`        | (CH) Indicates if the song has lyrics or not.<br>Meant for sorting purposes only.             | boolean     |

## Chart Properties

| Tag Name                | Description                                                                              | Data type |
| :-------                | :----------                                                                              | :-------: |
| `pro_drum`              | (FoFiX) Same as `pro_drums`?                                                             | boolean   |
| `drum_fallback_blue`    | (PS) "Sets 5 to 4 Lane Drums Fallback Note" (No solid idea what this means lol)          | boolean   |
|                         |                                                                                          |           |
| `tutorial`              | (FoFiX) Marks a song as a tutorial and hides it from Quickplay.                          | boolean   |
| `boss_battle`           | (FoFiX?) Marks a song as a boss battle.                                                  | boolean?  |
|                         |                                                                                          |           |
| `hopofreq`              | (FoFiX) Overrides the natural HOPO threshold using [numbers from 0 to 5](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L1309).<br>No clue how it actually behaves, but here's my interpretation of the code based on what would be reasonable: 0 = 1/24th, 1 = 1/16th, 2 = 1/12th, 3 = 1/8th, 4 = 1/6th, 5 or higher = 1/4th.<br>Not recommended for new charts, use `hopo_frequency` instead. | number |
| `early_hit_window_size` | (FoFiX) Sets the "early hit window" size.<br>Valid values are "none", "half", or "full". | string    |
| `star_power_note`       | (PS) Same as `multiplier_note`.                                                          | MIDI note |
| `end_events`            | (CH) Sets whether or not end events in the chart will be respected.                      | boolean   |
|                         |                                                                                          |           |
| `sysex_slider`          | (PS) Enables .mid SysEx events for guitar sliders/tap notes.                             | boolean   |
| `sysex_high_hat_ctrl`   | (PS) Enables .mid SysEx events for Real Drums hi-hat pedal control.                      | boolean   |
| `sysex_rimshot`         | (PS) Enables .mid SysEx events for Real Drums rimshot hits.                              | boolean   |
| `sysex_open_bass`       | (PS) Enables .mid SysEx events for guitar open notes.                                    | boolean   |
| `sysex_pro_slide`       | (PS) Enables .mid SysEx events for Pro Guitar/Bass slide directions.                     | boolean   |
|                         |                                                                                          |           |
| `guitar_type`           | (PS) Sound sample set index for guitar.                                                  | number    |
| `bass_type`             | (PS) Sound sample set index for bass.                                                    | number    |
| `kit_type`              | (PS) Sound sample set index for drums.                                                   | number    |
| `keys_type`             | (PS) Sound sample set index for keys.                                                    | number    |
| `dance_type`            | (PS) Sound sample set index for dance.                                                   | number    |

## Images and Other Resources

| Tag Name        | Description                                 | Data type |
| :-------        | :----------                                 | :-------: |
| `link_name_a`   | (PS) Name for banner A.                     | string    |
| `link_name_b`   | (PS) Name for banner B.                     | string    |
| `banner_link_a` | (PS) Link that clicking banner A will open. | string    |
| `banner_link_b` | (PS) Link that clicking banner B will open. | string    |

## Miscellaneous

| Tag Name           | Description                                                                         | Data type     |
| :-------           | :----------                                                                         | :-------:     |
| `scores`           | (FoFiX) High score data.                                                            | string        |
| `scores_ext`       | (FoFiX) Additional score data.                                                      | string        |
| `count`            | (FoFiX) Play count.                                                                 | number        |
| `rating`           | (PS) Player's rating of the song?                                                   | number        |
|                    |                                                                                     |               |
|                    |                                                                                     |               |
|                    | See the [FoFiX Careers doc](../Other/Frets%20on%20Fire%20X/Careers.md) for details. |               |
| `unlock_id`        | (FoFiX) Career ID for this song.                                                    | string        |
| `unlock_require`   | (FoFiX) The career ID that must be completed to unlock this song.                   | string        |
| `unlock_text`      | (FoFiX) Text to display if the song is locked.                                      | string        |
| `unlock_completed` | (FoFiX) Indicates if the song is unlocked.                                          | number        |
|                    |                                                                                     |               |
| `eof_midi_import_drum_accent_velocity` | (Editor on Fire) Sets a velocity number for drums accent notes. | MIDI velocity |
| `eof_midi_import_drum_ghost_velocity`  | (Editor on Fire) Sets a velocity number for drums ghost notes.  | MIDI velocity |

## Resources

These tags are sourced from existing song.ini files and from these resources:

- [grishhung's song.ini guide](https://docs.google.com/document/d/1ped13di4LqDqhaxbCgZEMUoqnyc3gOy3Bw1FCg58FPI/edit#)
- [Phase Shift forums: Song Standard Advancements](https://dwsk.proboards.com/thread/404/song-standard-advancements)
- [FoFiX docs: Songs](https://fofix.readthedocs.io/en/latest/users/songs.html)
- [FoFiX source code: song.py](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py)
