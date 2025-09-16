# info.json

Info.json is the metadata format Encore supports, though it is mostly deprecated now as seemingly only Encore uses the format and lacks important metadata tags.

## Main Tags

| Tag Name             | Description                                                                        | Data type |
| :-------             | :----------                                                                        | :-------: |
| `name`               | Title of the song.                                                                 | string    |
| `artist`             | Artist(s) or band(s) behind the song.                                              | string    |
| `album`              | Title of the album the song is featured in.                                        | string    |
| `release_year`       | Year of the song’s release.                                                        | string    |
| `length`             | Length of the song in seconds.                                                     | number    |
| `loading_phrase`     | Flavor text for this song, usually shown after picking the song or during loading. | string    |
|                      |                                                                                    |           |
| `midi`               | File name of the song's midi chart.                                                | string    |
| `art`                | File name of the song's cover art.                                                 | string    |
| `source`             | Name of an icon image to display for this song.<br>Included in either the chart folder or the game the chart was made for, or sourced from [this repository of icons](https://opensource.yarg.in/). | string |
| `preview_start_time` | Timestamp in milliseconds where the song preview starts.                           | number    |
|                      |                                                                                    |           |
| `icon_drums`         | Likely used to be for setting the icon of drums--Has no effect in-game.            | string    |
| `icon_bass`          | Likely used to be for setting the icon of bass--Has no effect in-game.             | string    |
| `icon_lead`          | Likely used to be for setting the icon of guitar--Has no effect in-game.           | string    |
| `icon_vocals`        | Likely used to be for setting the icon of vocals--Has no effect in-game.           | string    |
| `icon_keyboard`      | Likely used to be for setting the icon of keys--Has no effect in-game.             | string    |

## Arrays

| Array Name           | Tag Names             | Description                                                                        | Data type |
| :-------             | :-------              | :----------                                                                        | :-------: |
| `genres`             |                       | Genre(s) of the song.                                                              | string    |
| `charters`           |                       | Community member(s) responsible for charting the song.                             | string    |
|                      |                       |                                                                                    |           |
| `diff`               | `drums`               | Difficulty of the Pad Drums track.                                                 | number    |
| `diff`               | `bass`                | Difficulty of the Pad Bass track.                                                  | number    |
| `diff`               | `guitar`              | Difficulty of the Pad Guitar track.                                                | number    |
| `diff`               | `vocals`              | Difficulty of the Pad Vocals track.                                                | number    |
| `diff`               | `keys`                | Difficulty of the Pad Keys track.                                                  | number    |
| `diff`               | `plastic_drums`       | Difficulty of the Classic Drums track.                                             | number    |
| `diff`               | `plastic_bass`        | Difficulty of the Classic Bass track.                                              | number    |
| `diff`               | `plastic_guitar`      | Difficulty of the Classic Guitar track.                                            | number    |
| `diff`               | `pitched_vocals`      | Difficulty of the Classic Vocals track.                                            | number    |
| `diff`               | `plastic_vocals`      | Difficulty of the Plastic Vocals track.*                                           | number    |
| `diff`               | `plastic_keys`        | Difficulty of the Classic Keys track.                                              | number    |
|                      |                       |                                                                                    |           |
| `stems`              | `backing`             | File name of the background audio.                                                 | string    |
| `stems`              | `drums`               | File name of the drums stem.                                                       | string    |
| `stems`              | `bass`                | File name of the bass stem.                                                        | string    |
| `stems`              | `lead`                | File name of the guitar stem.                                                      | string    |
| `stems`              | `vocals`              | File name of the vocals stem.                                                      | string    |
