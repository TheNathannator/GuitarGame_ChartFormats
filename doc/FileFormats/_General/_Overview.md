# Common Custom Chart Files Overview

This is an overview of common files that may be found in custom charts.

## Chart Files

### Formats

Common:

- [.chart](Chart.md)
- [.mid](Midi.md)

Older:

- Freetar Hero's .sng, not documented currently (not to be confused with Clone Hero's .sng, which is different)

There are likely other undocumented chart formats out there, feel free to contribute your findings!

Sometimes multiple chart file types may be present at the same time in a chart folder. There is no standard way to handle this, whichever format is preferred is up to the parser.

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

## Audio

### Common Formats

- .mp3
- .ogg
- .wav
- .opus

### Names

| File name  | Description
| :--------  | :----------
| `preview`  | Preview audio to play on the song select menu.
| `song`     | Background audio that never gets muted by gameplay.
| `guitar`   | Lead Guitar audio.<br>For legacy, if there are no other tracks, this should be treated like `song`.
| `rhythm`   | Rhythm Guitar audio.
| `bass`     | Bass Guitar audio.
| `keys`     | Keys audio.
| `drums`    | Drums audio as a single stem.<br>Should not be present at the same time as `drums_1`.
| `drums_1`  | Drums kick drum audio.<br>Should not be used for a single-stem configuration.
| `drums_2`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.
| `drums_3`  | Drums audio for toms, plus cymbal audio if 4 is not present.
| `drums_4`  | Drums audio for cymbals.
| `vocals`   | Vocals audio.<br>Should not be present at the same time as `vocals_1`.
| `vocals_1` | Main vocals audio.
| `vocals_2` | Secondary vocals audio.
| `crowd`    | Background crowd noise/singing.

#### Legacy Names

These are audio filenames with an unknown purpose.

| File name, minus extension | Description                      |
| :------------------------: | :----------                      |
| `acoustic`                 | Audio track that FoFiX supports. |
| `piano`                    | Audio track that FoFiX supports. |

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

## Miscellaneous

- [scripts.txt](Scripts_txt.md) - (FoFiX) Defines text and picture events for a song.
