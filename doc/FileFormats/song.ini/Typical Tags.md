# Typical song.ini Tags

The tags listed here are all the common and recommended tags that should be supported by most consumers of the format. While there are many tags in existence, very few are commonly used or needed in order for things to work correctly.

Some tags are specific to certain game modes, and do not need to be supported if the game mode is not supported.

## Table of Contents

## Song/Chart Metadata

| Tag Name             | Description                                                                        | Data type |
| :-------             | :----------                                                                        | :-------: |
| `name`               | Title of the song.                                                                 | string    |
| `artist`             | Artist(s) or band(s) behind the song.                                              | string    |
| `album`              | Title of the album the song is featured in.                                        | string    |
| `genre`              | Genre of the song.                                                                 | string    |
| `sub_genre`          | Sub-genre for the song.                                                            | string    |
| `year`               | Year of the song’s release.                                                        | string    |
|                      |                                                                                    |           |
| `charter`            | Community member responsible for charting the song.                                | string    |
| `frets`              | Same as `charter`. Ignore this one if it's present.                                | string    |
| `version`            | Version number for the song.                                                       | number    |
|                      |                                                                                    |           |
| `album_track`        | Track number of the song within the album it's from.                               | number    |
| `track`              | Same as `album_track`. Ignore this one if it's present.                            | number    |
| `playlist_track`     | Track number of the song within the playlist/setlist it's from.                    | number    |
|                      |                                                                                    |           |
| `song_length`        | Length of the song's audio in milliseconds.<br>Metadata only, don't rely on this.  | number    |
| `preview_start_time` | Timestamp in milliseconds where the song preview starts.                           | number    |
| `preview_end_time`   | Timestamp in milliseconds that the preview should stop at.                         | number    |
|                      |                                                                                    |           |
| `loading_phrase`     | Flavor text for this song, usually shown after picking the song or during loading. | string    |

## Track Difficulties

| Tag Name              | Description                                   | Data type |
| :-------              | :----------                                   | :-------: |
| `diff_band`           | Overall difficulty of the song.               | number    |
|                       |                                               |           |
| `diff_guitar`         | Difficulty of the Lead Guitar track.          | number    |
| `diff_guitarghl`      | Difficulty of the 6-Fret Lead track.          | number    |
| `diff_guitar_coop`    | Difficulty of the Guitar Co-op track.         | number    |
| `diff_guitar_coop_ghl`| Difficulty of the 6-Fret Guitar Co-op track.  | number    |
| `diff_guitar_real`    | Difficulty of the Pro Guitar track.           | number    |
| `diff_guitar_real_22` | Difficulty of the Pro Guitar 22-fret track.   | number    |
| `diff_rhythm`         | Difficulty of the Rhythm Guitar track.        | number    |
| `diff_rhythm_ghl`     | Difficulty of the 6-Fret Rhythm Guitar track. | number    |
|                       |                                               |           |
| `diff_bass`           | Difficulty of the Bass Guitar track.          | number    |
| `diff_bassghl`        | Difficulty of the 6-Fret Bass track.          | number    |
| `diff_bass_real`      | Difficulty of the Pro Bass track.             | number    |
| `diff_bass_real_22`   | Difficulty of the Pro Bass 22-fret track.     | number    |
|                       |                                               |           |
| `diff_drums`          | Difficulty of the Drums track.                | number    |
| `diff_drums_real`     | Difficulty of the Pro Drums track.            | number    |
| `diff_drums_real_ps`  | Difficulty of the Drums Real track.           | number    |
|                       |                                               |           |
| `diff_keys`           | Difficulty of the Keys track.                 | number    |
| `diff_keys_real`      | Difficulty of the Pro Keys track.             | number    |
| `diff_keys_real_ps`   | Difficulty of the Keys Real track.            | number    |
|                       |                                               |           |
| `diff_vocals`         | Difficulty of the Vocals track.               | number    |
| `diff_vocals_harm`    | Difficulty of the Harmonies track.            | number    |
|                       |                                               |           |
| `diff_dance`          | Difficulty of the Dance track.                | number    |

## Chart Properties

These tags affect the parsing or configuration of a chart.

| Tag Name                     | Description                                                        | Data type |
| :-------                     | :----------                                                        | :-------: |
| `pro_drums`                  | Forces the Drums track to be Pro Drums.                            | boolean   |
| `five_lane_drums`            | Forces the Drums track to be 5-lane.                               | boolean   |
|                              |                                                                    |           |
| `vocal_gender`               | Specifies a voice type for the singer (either "male" or "female"). | string    |
|                              |                                                                    |           |
| `real_guitar_tuning`         | Specifies a tuning for 17-fret Pro Guitar.                         | Pro Guitar tuning |
| `real_guitar_22_tuning`      | Specifies a tuning for 22-fret Pro Guitar.                         | Pro Guitar tuning |
| `real_bass_tuning`           | Specifies a tuning for 17-fret Pro Bass.                           | Pro Guitar tuning |
| `real_bass_22_tuning`        | Specifies a tuning for 22-fret Pro Bass.                           | Pro Guitar tuning |
|                              |                                                                    |           |
| `real_keys_lane_count_right` | Specifies the number of lanes for the right hand in Real Keys.     | number    |
| `real_keys_lane_count_left`  | Specifies the number of lanes for the left hand in Real Keys.      | number    |

- `Pro Guitar tuning` - 4 to 6 numbers representing semitone differences from standard tuning, and an optional string for the tuning name.
  - Examples:
  - `0 0 0 0 0 0 "Standard tuning"`
  - `0 2 5 7 10 10`
  - `-2 0 0 0 "Drop D tuning"`
  - `0 0 0 0 0`

### Not Recommended

These tags are not recommended to be used in new charts, as either there are better alternatives, or they can affect how the chart plays without affecting the hash of the chart file itself (ideally these would instead be set using events or properties in the chart file, but so far no one's implemented or standardized anything for that).

| Tag Name                     | Description                                                                                      | Data type |
| :-------                     | :----------                                                                                      | :-------: |
| `delay`                      | Delays the chart relative to the audio by the specified number of milliseconds.<br>Higher = later notes. Can be negative. | number |
|                              |                                                                                                  |           |
| `sustain_cutoff_threshold`   | Overrides the default sustain cutoff threshold with a specified value in ticks.                  | number    |
| `hopo_frequency`             | Overrides the default HOPO threshold with a specified value in ticks.                            | number    |
| `eighthnote_hopo`            | Sets the HOPO threshold to be a 1/8th step.                                                      | boolean   |
| `multiplier_note`            | Overrides the .mid note number for Star Power on 5-Fret Guitar.<br>Valid values are 103 and 116. | MIDI note |

## Images and Other Resources

| Tag Name           | Description                                | Data type |
| :-------           | :----------                                | :-------: |
| `icon`             | Name of an icon image to display for this song.<br>Included in either the chart folder or the game the chart was made for, or sourced from [this repository of icons](https://gitlab.com/clonehero/sources). | string |
| `background`       | Name for a background image file.          | file name |
| `video`            | Name for a background video file.          | file name |
| `video_loop`       | Sets whether or not the video should loop. | boolean   |
| `video_start_time` | Timestamp in milliseconds where playback of an included video will start. Can be negative.<br>This tag controls the time relative to the video, not relative to the chart. Negative values will delay the video, positive values will make the video be at a further point in when the chart starts. | number |
| `video_end_time`   | Timestamp in milliseconds where playback of an included video will end. -1 means no time is specified.<br>This is assumed to also be relative to the video, not the chart. | number |
| `cover`            | Name for a cover image file.               | file name |

## Resources

These tags are sourced from existing song.ini files and from these resources:

- [grishhung's song.ini guide](https://docs.google.com/document/d/1ped13di4LqDqhaxbCgZEMUoqnyc3gOy3Bw1FCg58FPI/edit#)
- [Phase Shift forums: Song Standard Advancements](https://dwsk.proboards.com/thread/404/song-standard-advancements)
- [FoFiX docs: Songs](https://fofix.readthedocs.io/en/latest/users/songs.html)
- [FoFiX source code: song.py](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py)
