# Lyrics in .chart

This document details the format for lyrics in .chart files.

## Table of Contents

- [Events Section](#events-section)
  - [Lyric Events](#lyric-events)
  - [Lyrics Markup](#lyrics-markup)
  - [Displaying as Plain Text](#displaying-as-plain-text)
- [Example](#example)

## Events Section

### Lyric Events

Lyrics are charted as global events in the `[Events]` section. They are laid out as `phrase_start`, `phrase_end`, and `lyric` events throughout.

| Event Name         | Description                                                                                                |
| :---               | :----------                                                                                                |
| `phrase_start`     | Marks the start of a new lyrics phrase.<br>A preceding `phrase_end` is not required to start a new phrase. |
| `phrase_end`       | Marks the end of the current lyrics phrase.                                                                |
| `lyric <syllable>` | Contains a syllable of a lyrics phrase.                                                                    |

### Lyrics Markup

Various symbols are used as markup in lyrics, to do things like join syllables together or stand in for a syllable that's reserved as markup. Most of these originate from .mid and don't strictly apply to .chart functionality, and are listed for completeness, since .mid charts converted to .chart may have these symbols.

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

### Displaying as Plain Text

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

## Example

```
[Events]
{
  576 = E "phrase_start"
  768 = E "lyric This"
  864 = E "lyric is"
  960 = E "lyric a"
  1056 = E "lyric lyr-"
  1152 = E "lyric ics"
  1248 = E "lyric test"
  1536 = E "phrase_end"
  2880 = E "phrase_start"
  3072 = E "lyric This"
  3168 = E "lyric is"
  3264 = E "lyric a"
  3312 = E "phrase_start"
  3360 = E "lyric Lyr-"
  3456 = E "lyric ics"
  3552 = E "lyric test"
  3840 = E "phrase_end"
}
```

When concatenated:

```
This is a lyrics test
This is a
Lyrics test
```
