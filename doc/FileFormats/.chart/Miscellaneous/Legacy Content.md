# Legacy Content

Since .chart is an old format, there is of course content considered to be legacy. These were either never implemented in anything (at least as far as anyone knows), or are no longer relevant to modern programs/games.

## Table of Contents

- [Section Names](#section-names)
- [Metadata](#metadata)
- [References](#references)

## Section Names

Some older charts have a `SingleBass` track present, which was used for GH3/GHTCP. This is just another 5-fret guitar track, the semantics of which are not fully clear yet; however, most everything nowadays doesn't support it.

The following tracks are present in Feedback Editor, but they were never actually fleshed out and simply use the regular 5-fret guitar layout.

- `EnhancedGuitar`
- `CoopLead`
- `CoopBass`
- `10KeyGuitar`
- `DoubleDrums`
- `Vocals`

## Metadata

Some definitions:

- GHTCP stands for Guitar Hero Three Control Panel, a tool to modify Guitar Hero 3, which includes importing songs.
- Feedback refers to the Feedback Editor program.

| Key            | Description                                                                                                  | Data type |
| :---           | :----------                                                                                                  | :-------- |
| `ArtistText`   | (GHTCP) Text to use before the artist name.<br>Can be custom, but most commonly "by" or "as made famous by". | string    |
| `Singer`       | (GHTCP) Indicates which singer should be used in-game.                                                       | string    |
| `Bassist`      | (GHTCP) Indicates which bassist should be used in-game.                                                      | string    |
| `Boss`         | (GHTCP) Indicates whether or not this is a boss song?<br>Unsure what data is valid for this tag.             | string    |
| `Player2`      | (GHTCP) Instrument to use for player 2.<br>Valid values are `Bass`/`bass` and `Rhythm`/`rhythm`.             | bare string |
| `CountOff`     | (GHTCP) The countoff sample to use in-game.                                                                  | string    |
| `GuitarVolume` | (GHTCP) Sets the volume for the guitar audio track.                                                          | decimal   |
| `GuitarVol`    | (GHTCP) Same as the above.                                                                                   | decimal   |
| `BandVolume`   | (GHTCP) Sets the volume for the band audio track.                                                            | decimal   |
| `BandVol`      | (GHTCP) Same as the above.                                                                                   | decimal   |
| `HoPo`         | (GHTCP) The HOPO threshold that should be used for this chart.<br>This value is equal to `(<step size denominator>) / 4` (1/4 step = 1.00, 1/8 = 2.00, 1/2 = 0.50, etc.). | decimal |
| `Fretboard`    | (Feedback) The fretboard image to use for this song, without the file extension.                             | file path |
| `MediaType`    | (Feedback) Type of media to display in-game?                                                                 | string    |
| `MusicURL`     | (Feedback) URL for the song audio file.                                                                      | string    |
| `PreviewURL`   | (Feedback) URL for the preview audio file.                                                                   | string    |

## Battle Mode Special Phrases

These special phrases are used by GHTCP to mark phrases used in battles. The specifics aren't clear yet, more research needs to be done to figure out what exactly they're for.

| Special Type | Description              |
| :----------: | :----------              |
| 3            | Battle Star Power phrase |
| 4            | Battle player 1 phrase   |
| 5            | Battle player 2 phrase   |

## References

- https://github.com/szymmirr/Open-GHTCP-2021/blob/master/GHNamespace8/ChartParser.cs
- https://github.com/TurkeyMan/feedback-editor/blob/master/FeedBack/Source/Song.cpp
