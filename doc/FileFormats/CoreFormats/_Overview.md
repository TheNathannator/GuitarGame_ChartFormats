# Common Custom Chart Files Overview

This folder contains a general overview of files that may be found in custom charts. Documents here go over as much as possible as to what may be found, but leave more game-specific stuff to their respective folders. In other words, they detail the core format necessary to parse these files correctly in general situations. More specific documentation of formats, such as the GH2-specific layout of .mid charts, are detailed in their respective folders.

## Table of Contents

- [Chart Files](#chart-files)
  - [Chart Formats](#chart-formats)
  - [File Names](#file-names)
- [Metadata](#metadata)
  - [Metadata Formats](#metadata-formats)
- [Audio](#audio)
  - [Common Formats](#common-formats)
  - [Stem Names](#stem-names)
- [Images](#images)
  - [Common Image Formats](#common-image-formats)
  - [Image Names](#image-names)
- [Videos](#videos)
  - [Common Video Formats](#common-video-formats)
  - [Video Names](#video-names)

## Chart Files

### Chart Formats

- [.chart](Chart.md)
- [.mid](Midi.md)

Sometimes multiple chart file types may be present at the same time in a chart folder. There is no standard way to handle this, whether one format is chosen over the other or a song entry for each chart file is added is up to the game/program.

### File Names

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

### Metadata Formats

- [song.ini](Song_ini.md)
- [.chart's `[Song]` section](Chart.md#song-section-details)

## Audio

### Common Formats

- .mp3
- .ogg
- .wav (maybe not *common*, but is supported by some games/programs)
- .opus

### Stem Names

| File name  | Description                                                                                         |
| :--------  | :----------                                                                                         |
| `preview`  | Preview audio to play on the song select menu.                                                      |
| `song`     | Background audio that never gets muted by gameplay.                                                 |
| `guitar`   | Lead Guitar audio.<br>For legacy, if there are no other tracks, this should be treated like `song`. |
| `rhythm`   | Rhythm Guitar audio.                                                                                |
| `bass`     | Bass Guitar audio.                                                                                  |
| `keys`     | Keys audio.                                                                                         |
| `drums`    | Drums audio as a single stem.<br>Should not be present at the same time as `drums_#` stems.         |
| `drums_1`  | Drums kick drum audio.<br>Should not be used for a single-stem configuration.                       |
| `drums_2`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.               |
| `drums_3`  | Drums audio for toms, plus cymbal audio if 4 is not present.                                        |
| `drums_4`  | Drums audio for cymbals.                                                                            |
| `vocals`   | Vocals audio as a single stem.<br>Should not be present at the same time as `vocals_#` stems.       |
| `vocals_1` | Main vocals audio.                                                                                  |
| `vocals_2` | Secondary vocals audio.                                                                             |
| `crowd`    | Background crowd noise/singing.                                                                     |

## Images

### Common Image Formats

Images:

- .png
- .jpg/.jpeg

### Image Names

| File name, minus extension | Description
| :------------------------: | :----------
| `album`                    | Album art/cover to show on the song menu.
| `background`               | Image to show in the background during gameplay.
| `banner`                   | Image to show as a banner.

Custom names may be used for some things, see the `icon`, `banner`, and `cover` song.ini tags.

## Videos

### Common Video Formats

- .mp4 (x264 or x265)
- .webm (VP8)

Other formats supported by games:

- .mpeg
- .avi
- .ogv

### Video Names

| File name, minus extension | Description
| :------------------------: | :----------
| `video`                    | Video to play in the background during gameplay.

A custom name may be used, see the `video` song.ini tag.
