# Common Custom Chart Files Overview

This folder contains a general overview of files that may be found in custom charts. Documents here go over as much as possible as to what may be found, but leave more game-specific stuff like .chart global/local events and .mid text events to their respective folders. In other words, they detail the core format necessary to parse these files correctly in general situations.More specific documentation of formats, such as the GH2-specific layout of .mid charts, will later be detailed in their respective folders.

There may be some non-band game items documented here, such as .mid `PART DANCE` and .chart `<diff>Beatmania`, mostly for preservation purposes.

## Table of Contents

- [Chart Files](#chart-files)
  - [Formats](#formats)
  - [Names](#names)
- [Metadata](#metadata)
  - [Format](#format)
- [Audio](#audio)
  - [Common Formats](#common-formats)
  - [Names](#names-1)
    - [Legacy Names](#legacy-names)
- [Images](#images)
  - [Common Formats](#common-formats-1)
  - [Names](#names-2)
- [Videos](#videos)
  - [Common Formats](#common-formats-2)
  - [Names](#names-3)
- [Miscellaneous](#miscellaneous)

## Chart Files

### Formats

- [.chart](Chart.md)
- [.mid](Midi.md)

Sometimes multiple chart file types may be present at the same time in a chart folder. There is no standard way to handle this, whichever format is preferred is up to the program/game.

### Names

Chart files may be found either with a custom name, or with a name of `notes` (casing may vary) for use in FoFiX, Phase Shift, or Clone Hero.

Some charts have multiple .chart files whose names are suffixed with one of the following:

- `[Y]`/`[F]` - Chart has forced notes
- `[N]` - Chart does not have forced notes

`[Y]` and `[F]` should be preferred over `[N]` or another .chart file in the same folder with no suffix. These suffixes may also be found in lowercase.

Example:

```
Song Name [Y].chart
Song Name [N].chart
```

.mid files are usually always named `notes`, though there may be some with a custom name, and possibly the same suffixing as some .chart files.

## Metadata

### Format

- [song.ini](Song_ini.md)
- [.chart's `[Song]` section](Chart.md#song-section-details)

## Audio

### Common Formats

- .mp3
- .ogg
- .wav (maybe not *common*, but is supported by some games/programs)
- .opus

### Names

| File name  | Description                                                                                         |
| :--------  | :----------                                                                                         |
| `preview`  | Preview audio to play on the song select menu.                                                      |
| `song`     | Background audio that never gets muted by gameplay.                                                 |
| `guitar`   | Lead Guitar audio.<br>For legacy, if there are no other tracks, this should be treated like `song`. |
| `rhythm`   | Rhythm Guitar audio.                                                                                |
| `bass`     | Bass Guitar audio.                                                                                  |
| `keys`     | Keys audio.                                                                                         |
| `drums`    | Drums audio as a single stem.<br>Should not be present at the same time as `drums_1`.               |
| `drums_1`  | Drums kick drum audio.<br>Should not be used for a single-stem configuration.                       |
| `drums_2`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.               |
| `drums_3`  | Drums audio for toms, plus cymbal audio if 4 is not present.                                        |
| `drums_4`  | Drums audio for cymbals.                                                                            |
| `vocals`   | Vocals audio as a single stem.<br>Should not be present at the same time as `vocals_1`.             |
| `vocals_1` | Main vocals audio.                                                                                  |
| `vocals_2` | Secondary vocals audio.                                                                             |
| `crowd`    | Background crowd noise/singing.                                                                     |

#### Legacy Names

These are audio filenames with an unknown purpose.

| File name, minus extension | Description                                                                                      |
| :------------------------: | :----------                                                                                      |
| `acoustic`                 | Audio track that the [FoFiX docs list](https://fofix.readthedocs.io/en/latest/users/songs.html). |
| `piano`                    | Audio track that the [FoFiX docs list](https://fofix.readthedocs.io/en/latest/users/songs.html). |

## Images

### Common Formats

Images:

- .png
- .jpg/.jpeg

### Names

| File name, minus extension | Description
| :------------------------: | :----------
| `album`                    | Album art/cover to show on the song menu.
| `background`               | Image to show in the background during gameplay.
| `banner`                   | Image to show as a banner.

Custom names may be used for some things, see the `icon`, `banner`, and `cover` song.ini tags.

## Videos

### Common Formats

- .mp4 (x264)
- .mpeg
- .avi
- .webm (VP8)
- .ogv

### Names

| File name, minus extension | Description
| :------------------------: | :----------
| `video`                    | Video to play in the background during gameplay.

A custom name may be used, see the `video` song.ini tag.

## Miscellaneous

- [scripts.txt](Scripts_txt.md) - (FoFiX) Defines text and picture events for a song.
