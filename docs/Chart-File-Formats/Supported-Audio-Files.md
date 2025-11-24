# Common Audio File Info

## Recommended Supported Formats

| File type | Recommended Quality  | Comments
| :-------- | :------------------  | :-------
| .opus     | 80 kbps              | Highly recommended for its file size to quality ratio.
| .ogg      | Quality 8 (256 kbps) | Generally fine for single-file audio charts. For stems, consider using .opus instead.
| .mp3      | 320 kbps             | Not very recommended for new charts, use .ogg or .opus instead.

## File Names

There are a number of audio file names reserved for use as specific audio tracks.

| File name           | Description
| :--------           | :----------
| `preview`           | Preview audio to play on the song select menu.
| `song`              | Background audio not associated with any particular instrument, or the main audio stream if it is the only one present..
| `guitar`            | Lead guitar audio, or the main audio stream if it is the only one present.
| `rhythm`            | Rhythm guitar audio, or bass guitar audio if no rhythm guitar track is present.
| `bass`              | Bass guitar audio.
| `keys`              | Keys audio.
| `drums`             | Drums audio as a single stem.<br>Should be ignored if numbered stems (`drums_#`) exist.
| `drums_1`           | Drums audio for the kick drum, plus snare, tom, and cymbal audio if 2-4 are not present.
| `drums_2`           | Drums audio for the snare drum, plus tom and cymbal audio if 3 and 4 are not present.
| `drums_3`           | Drums audio for toms, plus cymbal audio if 4 is not present.
| `drums_4`           | Drums audio for cymbals.
| `vocals`            | Vocals audio as a single stem.<br>Should be ignored if numbered stems (`vocals_#`) exist.
| `vocals_1`          | Main vocals audio.
| `vocals_2`          | Secondary vocals audio.
| `vocals_explicit`   | Contains only explicit lyrics which have been removed from the vocals audio.
| `vocals_explicit_1` | Contains only explicit lyrics which have been removed from the main vocals audio.
| `vocals_explicit_2` | Contains only explicit lyrics which have been removed from the secondary vocals audio.
| `crowd`             | Background crowd noise/singing audio.
