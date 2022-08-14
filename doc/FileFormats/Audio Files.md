# Common Audio File Info

## Recommended Supported Formats

- .mp3
- .ogg
- .opus
- .wav

## File Names

There are a number of audio file names reserved for use as specific audio tracks.

| File name  | Description                                                                               |
| :--------  | :----------                                                                               |
| `preview`  | Preview audio to play on the song select menu.                                            |
| `song`     | The main audio stream.<br>When other audio stems are present, this is background audio not in the other tracks and/or instruments not charted. |
| `guitar`   | Lead Guitar audio, or the main audio stream if this is the only one present.              |
| `rhythm`   | Rhythm Guitar audio.                                                                      |
| `bass`     | Bass Guitar audio.                                                                        |
| `keys`     | Keys audio.                                                                               |
| `drums`    | Drums audio as a single stem.<br>Should be ignored if numbered stems (`drums_#`) exist.   |
| `drums_1`  | Drums audio for the kick drum, plus snare, tom, and cymbal audio if 2-4 are not present.  |
| `drums_2`  | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.     |
| `drums_3`  | Drums audio for toms, plus cymbal audio if 4 is not present.                              |
| `drums_4`  | Drums audio for cymbals.                                                                  |
| `vocals`   | Vocals audio as a single stem.<br>Should be ignored if numbered stems (`vocals_#`) exist. |
| `vocals_1` | Main vocals audio.                                                                        |
| `vocals_2` | Secondary vocals audio.                                                                   |
| `crowd`    | Background crowd noise/singing audio.                                                     |
