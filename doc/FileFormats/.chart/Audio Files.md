# Common Audio Files

This document details audio files that .chart files may have.

## Names

Audio files may have any name, as they can be specified in the .chart file itself through the following metadata tags:

| Entry Name     | Description                                                                              | Data type |
| :---------     | :----------                                                                              | :-------- |
| `MusicStream`  | The main audio stream.<br>When other audio stems are present, this is background audio not in the other tracks and/or instruments not charted. | file path |
| `GuitarStream` | Lead Guitar audio.                                                                       | file path |
| `RhythmStream` | Rhythm Guitar audio.                                                                     | file path |
| `BassStream`   | Bass Guitar audio.                                                                       | file path |
| `KeysStream`   | Keys audio.                                                                              | file path |
| `DrumStream`   | Drums audio for the kick drum, plus snare, tom, and cymbal audio if 2-4 are not present. | file path |
| `Drum2Stream`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.    | file path |
| `Drum3Stream`  | Drums audio for toms, plus cymbal audio if 4 is not present.                             | file path |
| `Drum4Stream`  | Drums audio for cymbals.                                                                 | file path |
| `VocalStream`  | Vocals audio.                                                                            | file path |
| `CrowdStream`  | Background crowd noise/singing audio.                                                    | file path |

Most modern .chart files don't deliberately specify audio file names though, and instead rely on these static file names:

| File name  | Description                                                                                         |
| :--------  | :----------                                                                                         |
| `preview`  | Preview audio to play on the song select menu.                                                      |
| `song`     | The main audio stream.<br>When other audio stems are present, this is background audio not in the other tracks and/or instruments not charted. |
| `guitar`   | Lead Guitar audio, or the main audio stream if this is the only one present.                        |
| `rhythm`   | Rhythm Guitar audio.                                                                                |
| `bass`     | Bass Guitar audio.                                                                                  |
| `keys`     | Keys audio.                                                                                         |
| `drums`    | Drums audio as a single stem.<br>Should not be present at the same time as `drums_#` stems.         |
| `drums_1`  | Drums audio for the kick drum, plus snare, tom, and cymbal audio if 2-4 are not present.<br>Should not be used for a single-stem configuration. |
| `drums_2`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.               |
| `drums_3`  | Drums audio for toms, plus cymbal audio if 4 is not present.                                        |
| `drums_4`  | Drums audio for cymbals.                                                                            |
| `vocals`   | Vocals audio as a single stem.<br>Should not be present at the same time as `vocals_#` stems.       |
| `vocals_1` | Main vocals audio.<br>Should not be used for a single-stem configuration.                           |
| `vocals_2` | Secondary vocals audio.                                                                             |
| `crowd`    | Background crowd noise/singing audio.                                                               |
