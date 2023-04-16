# Vocals in .mid

## Table of Contents

- [Track Names](#track-names)
- [Track Notes](#track-notes)
  - [Phrase Mechanics](#phrase-mechanics)
- [Text Events](#text-events)
  - [Lyrics](#lyrics)
    - [Lyrics Markup](#lyrics-markup)
    - [Displaying in Vocals Gameplay](#displaying-in-vocals-gameplay)
    - [Displaying as Plain Text](#displaying-as-plain-text)

## Track Names

- `PART VOCALS` - Standard vocals track
- `HARM1`, `HARM2`, `HARM3` - Harmonies track 1, 2, and 3 respectively
- `PART HARM1`, `PART HARM2`, `PART HARM3` - Alternative harmony track names

## Track Notes

| MIDI Note  | Description                   |
| :-------:  | :----------                   |
| Markers    |                               |
| 116        | Star Power marker             |
| 106        | Player 2 lyrics phrase marker |
| 105        | Player 1 lyrics phrase marker |
|            |                               |
| Percussion |                               |
| 97         | Not displayed percussion      |
| 96         | Displayed percussion          |
|            |                               |
| Notes      |                               |
| 84         | C6 (Highest)                  |
| 83         | B5                            |
| 82         | Bb5                           |
| 81         | A5                            |
| 80         | G#5                           |
| 79         | G5                            |
| 78         | F#5                           |
| 77         | F5                            |
| 76         | E5                            |
| 75         | Eb5                           |
| 74         | D5                            |
| 73         | C#5                           |
| 72         | C5                            |
| 71         | B4                            |
| 70         | Bb4                           |
| 69         | A4                            |
| 68         | G#4                           |
| 67         | G4                            |
| 66         | F#4                           |
| 65         | F4                            |
| 64         | E4                            |
| 63         | Eb4                           |
| 62         | D4                            |
| 61         | C#4                           |
| 60         | C4                            |
| 59         | B3                            |
| 58         | Bb3                           |
| 57         | A3                            |
| 56         | G#3                           |
| 55         | G3                            |
| 54         | F#3                           |
| 53         | F3                            |
| 52         | E3                            |
| 51         | Eb3                           |
| 50         | D3                            |
| 49         | C#3                           |
| 48         | C3                            |
| 47         | B2                            |
| 46         | Bb2                           |
| 45         | A2                            |
| 44         | G#2                           |
| 43         | G2                            |
| 42         | F#2                           |
| 41         | F2                            |
| 40         | E2                            |
| 39         | Eb2                           |
| 38         | D2                            |
| 37         | C#2                           |
| 36         | C2 (Lowest)                   |
|            |                               |
| Shifts     |                               |
| 1          | Lyric Shift                   |
| 0          | Range Shift                   |

### Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained are doubled (this applies on top of the standard combo multiplier), and health gained drastically increases.

- Standard Vocals and Harmonies have independent Star Power. `HARM2` and `HARM3` get their Star Power from `HARM1`.

Lyric shift markers set points at which a static lyric display should divide the current phrase into another segment that can be scrolled to, as if it were a separate phrase.

Range shift markers set points at which the note display should re-calculate the range it displays and gradually shift towards the new range. The length of these markers determine the speed of the shift, not how long it stays shifted for.

The player 1 and 2 phrases designate a section of the song to be sung by a specific player in certain 2-player versus modes. During player 1 phrases, only player 1 sings the song, and during player 2 phrases, only player 2 sings the song. Both phrases can occur at the same time to make both players play at the same time.

## Text Events

| Event Text      | Description                                                                                    |
| :---------      | :----------                                                                                    |
| `[range_shift]` | Shifts the range of the vocals display to match the range of the song from this point forward. |

### Lyrics

Lyrics are stored as meta events (usually text or lyric), usually paired up with notes in the 36 to 84 range to determine pitch.

- Some charts don't have notes for pitch, these charts are lyrics-only.
- Some charts may have more than one lyric event at the same position. In the known cases of this, the events have the same text and can be safely ignored. Other cases are best ignored or should make the chart be rejected.

Lyric phrases are marked using either note 105 or note 106, or both at the same time (same start and end).

- The two separate phrase markers was done in Rock Band to handle competitive modes where two players playing the same part take turns playing the part, with sections where they both play at the same time. These modes were dropped in Rock Band 3 however, and thus the main phrase marker is note 105.
- For harmonies, the `HARM1` phrase is used for all 3 harmony tracks. The `HARM2` phrase is used to mark when harmony 2/3 lyrics shift in static vocals, and it must cover all notes in `HARM3`. Phrase markers are not used in `HARM3`.

#### Lyrics Markup

| Name           | Symbol | Description                                                                                       |
| :---           | :----: | :----------                                                                                       |
| Hyphen         | `-`    | Joins this syllable with the next syllable in the phrase.                                         |
| Plus           | `+`    | Connects the previous note and the current note into a slide. Usually found standalone.           |
| Equals         | `=`    | Stands in for a literal hyphen, and joins this syllable with the next in the phrase.              |
| Pound          | `#`    | Marks a note as non-pitched.                                                                      |
| Caret          | `^`    | Marks a note as a more lenient non-pitched note. Typically used on short syllables or syllables without sharp attacks. |
| Asterisk       | `*`    | Marks a note as non-pitched, but its exact function is unknown.                                   |
| Percent        | `%`    | Marks a point at the end of a phrase where the note display should re-calculate the range of notes it displays. |
| Section        | `§`    | Used in a single syllable to indicate that two syllables (typically part of two different words) are being sung as a single syllable. |
| Dollar         | `$`    | Marks syllables in harmony parts 2 and 3 as hidden. Does nothing on part 1 or on standard Vocals. |
| Slash          | `/`    | Marks a point at which a static lyric display should divide the current phrase into another segment that can be scrolled to, as if it were a separate phrase. |
| Underscore     | `_`    | Stands in for a space.<br>Some games allow spaces to exist normally within lyric events, this symbol exists for cases where doing so may not be desirable. |
| Angle brackets | `<>`   | Either notates an action such as whistling (`<whistle>`), or holds a [Unity](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/StyledText.html#supported-tags)/[TextMeshPro](http://digitalnativestudios.com/textmeshpro/docs/rich-text/) rich text formatting tag. |

Static lyrics refers to a setting where the notes scroll across the screen as normal, but lyrics are displayed in a constant position, only scrolling when a phrase has been completed or a shift point has been hit.

#### Displaying in Vocals Gameplay

| Name           | Symbol | Action                                                                              |
| :---           | :----: | :----------                                                                         |
| Hyphen         | `-`    | Display as-is.                                                                      |
| Plus           | `+`    | Remove, and connect the previous note with the current note.                        |
| Equals         | `=`    | Replace with a hyphen.                                                              |
| Pound          | `#`    | Remove, and display the note as a non-pitched note.                                 |
| Caret          | `^`    | Remove, and display the note as a non-pitched note. Additionally, be less strict with timing at the beginning and end of the note. |
| Asterisk       | `*`    | Remove, and display the note as a non-pitched note.                                 |
| Percent        | `%`    | Remove, and recalculate the range of the note display.                              |
| Section        | `§`    | Replace with a tie character `‿`                                                   |
| Dollar         | `$`    | If on harmony parts 2 or 3, hide the entire syllable. Otherwise, remove just the $. |
| Slash          | `/`    | If using static lyrics, divide the phrase shifting on this syllable (don't divide the actual scoring phrase, however). |
| Underscore     | `_`    | Replace with a space.                                                               |
| Angle brackets | `<>`   | Remove any that are text formatting tags, and either leave the rest as-is, or replace the brackets with asterisks. |

Syllables should not be joined together when using scrolling lyrics, but should be when using static lyrics.

#### Displaying as Plain Text

| Name           | Symbol | Action                                                                         |
| :---           | :----: | :----------                                                                    |
| Hyphen         | `-`    | Remove, and concatenate the next syllable into the current one.                |
| Plus           | `+`    | Remove.                                                                        |
| Equals         | `=`    | Replace with a hyphen, and concatenate the next syllable into the current one. |
| Pound          | `#`    | Remove.                                                                        |
| Caret          | `^`    | Remove.                                                                        |
| Asterisk       | `*`    | Remove.                                                                        |
| Percent        | `%`    | Remove.                                                                        |
| Section        | `§`    | Replace with a tie character `‿` or with a space.                             |
| Dollar         | `$`    | Remove.                                                                        |
| Slash          | `/`    | Either remove, or split the phrase at this point.                              |
| Underscore     | `_`    | Replace with a space.                                                          |
| Angle brackets | `<>`   | Remove any that are text formatting tags, and either leave the rest as-is, or replace the brackets with asterisks. |

Syllables not joined together by anything should be separated with a space.
