# song.ini Tags

## Song/Chart Metadata

| Tag Name               | Description                                                                      | Data type |
| :-------               | :----------                                                                      | :-------: |
| `preview`              | Two timestamps in milliseconds for preview start and end time.<br>Example: `55000 85000` | two numbers |

## Chart Properties

| Tag Name               | Description                                                                      | Data type |
| :-------               | :----------                                                                      | :-------: |
| `drum_fallback_blue`   | "Sets 5 to 4 Lane Drums Fallback Note"                                           | boolean   |
|                        |                                                                                  |           |
| `sysex_slider`         | Enables .mid SysEx events for guitar sliders/tap notes.                          | boolean   |
| `sysex_high_hat_ctrl`  | Enables .mid SysEx events for Real Drums hi-hat pedal control.                   | boolean   |
| `sysex_rimshot`        | Enables .mid SysEx events for Real Drums rimshot hits.                           | boolean   |
| `sysex_open_bass`      | Enables .mid SysEx events for guitar open notes.                                 | boolean   |
| `sysex_pro_slide`      | Enables .mid SysEx events for Pro Guitar/Bass slide directions.                  | boolean   |
|                        |                                                                                  |           |
| `guitar_type`          | Sound sample set index for guitar.                                               | number    |
| `bass_type`            | Sound sample set index for bass.                                                 | number    |
| `kit_type`             | Sound sample set index for drums.                                                | number    |
| `keys_type`            | Sound sample set index for keys.                                                 | number    |
| `dance_type`           | Sound sample set index for dance.                                                | number    |
|                        |                                                                                  |           |
| `real_keys_lane_count_right` | The number of lanes for the right hand in Real Keys.                       | number    |
| `real_keys_lane_count_left` | The number of lanes for the left hand in Real Keys.                         | number    |

## Images and Other Resources

| Tag Name               | Description                                                                      | Data type |
| :-------               | :----------                                                                      | :-------: |
| `link_name_a`          | Name for banner A.                                                               | string    |
| `link_name_b`          | Name for banner B.                                                               | string    |
| `banner_link_a`        | Link that clicking banner A will open.                                           | string    |
| `banner_link_b`        | Link that clicking banner B will open.                                           | string    |

## Sources

These tags are sourced from existing song.ini files and from these resources:

- [Phase Shift forums: Song Standard Advancements](https://dwsk.proboards.com/thread/404/song-standard-advancements)
