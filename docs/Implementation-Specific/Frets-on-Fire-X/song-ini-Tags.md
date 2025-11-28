# song.ini Tags

## Song/Chart Metadata

| Tag Name               | Description                                                                      | Data type |
| :-------               | :----------                                                                      | :-------: |
| `cassettecolor`        | Hex color to use in the song screen for the cassette.                            | hex color |
| `tags`                 | Miscellaneous tags for the chart.<br>Only valid value is `cover`.                | string    |

## Chart Properties

| Tag Name               | Description                                                                      | Data type |
| :-------               | :----------                                                                      | :-------: |
| `hopofreq`             | Overrides the natural HOPO threshold using [numbers from 0 to 5](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L1309).<br>No clue how it actually behaves, but here's my interpretation of the code based on what would be reasonable: 0 = 1/24th, 1 = 1/16th, 2 = 1/12th, 3 = 1/8th, 4 = 1/6th, 5 or higher = 1/4th.<br>Not recommended for new charts, use `hopo_frequency` instead. | number |
| `early_hit_window_size` | Sets the "early hit window" size.<br>Valid values are "none", "half", or "full". | string    |
|                        |                                                                                  |           |
| `pro_drum`             | Same as `pro_drums`?                                                             | boolean   |
| `tutorial`             | Marks a song as a tutorial and hides it from Quickplay.                          | boolean   |
| `boss_battle`          | Marks a song as a boss battle.                                                   | boolean?  |

## Save Data

| Tag Name               | Description                                                                      | Data type |
| :-------               | :----------                                                                      | :-------: |
| `scores`               | High score data.                                                                 | string    |
| `scores_ext`           | Additional score data.                                                           | string    |
| `count`                | Play count.                                                                      | number    |

## Career

| Tag Name               | Description                                                                      | Data type |
| :-------               | :----------                                                                      | :-------: |
|                        | See [Careers](Careers.md) for details.                                           |           |
| `unlock_id`            | Career ID for this song.                                                         | string    |
| `unlock_require`       | The career ID that must be completed to unlock this song.                        | string    |
| `unlock_text`          | Text to display if the song is locked.                                           | string    |
| `unlock_completed`     | Indicates if the song is unlocked.                                               | number    |

## Sources

These tags are sourced from existing song.ini files and from these resources:

- [FoFiX docs: Songs](https://fofix.readthedocs.io/en/latest/users/songs.html)
- [FoFiX source code: song.py](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py)
