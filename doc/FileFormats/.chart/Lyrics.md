# Lyrics in .chart

This document details the format for lyrics in .chart files.

## Table of Contents

- [Events Section](#events-section)
  - [Lyrics](#lyrics)
- [Example](#example)

## Events Section

### Lyrics

Lyrics are charted as global events in the `[Events]` section. They are laid out as `phrase_start`, `phrase_end`, and `lyric` events throughout.

| Event Name         | Description                                                                                                     |
| :---               | :----------                                                                                                     |
| `phrase_start`     | Marks the start of a new lyrics phrase.<br>Can be used to mark a new phrase without using a prior `phrase_end`. |
| `phrase_end`       | Marks the end of the current lyrics phrase.                                                                     |
| `lyric <syllable>` | Contains a syllable of a lyrics phrase.                                                                         |

- `phrase_start` marks the start of a phrase of lyrics. A preceding `phrase_end` is not required to start a new phrase from an existing one.
- `phrase_end` marks the end of a phrase.
- Any `lyric` events between a `phrase_start` and either a `phrase_end` event or a new `phrase_start` event should be concatenated into a single line. The text of this phrase may be highlighted one syllable at a time as `lyric` events are passed.

Various symbols are used as markup in lyrics, to do things like join syllables together or stand in for a syllable that's reserved as markup. Most of these originate from .mid and don't strictly apply to .chart functionality, and are listed for completeness, since .mid charts converted to .chart may have those symbols.

| Name           | Symbol | Description                                                                                      |
| :---           | :----: | :----------                                                                                      |
| Hyphens        | `-`    | Joins this syllable with the next.                                                               |
| Pluses         | `+`    | In .mid, connects the previous note and the current note into a slide. Usually found standalone. |
| Equals         | `=`    | Stands in for a literal hyphen, and joins this syllable with the next if placed at the end.      |
| Pounds         | `#`    | In .mid, marks a note as non-pitched.                                                            |
| Carets         | `^`    | In .mid, marks a note as non-pitched with a more lenient scoring.<br>Typically used on short syllables or syllables without sharp attacks. |
| Asterisks      | `*`    | In .mid, marks a note as non-pitched. Its exact function is unknown.                             |
| Percents       | `%`    | In .mid, marks a range division for vocals parts with large octave ranges.                       |
| Sections       | `§`    | Indicates that the text before and after in the syllable event are two different syllables sung as one.<br>Replaced by a space when displayed as just lyrics, or, in .mid, with a tie character `‿`. |
| Dollars        | `$`    | In .mid, marks syllables in harmony parts as hidden.<br>In one case these are also on the standard Vocals track for some reason, its purpose is not known (likely a charting error, as there are also harmony parts on that song). |
| Slashes        | `/`    | Unknown. These appear in some Rock Band charts, mainly The Beatles: Rock Band.                   |
| Underscores    | `_`    | Stands in for a space.                                                                           |
| Angle brackets | `<>`   | Either marks a vocal action such as `<whistle>`, or holds a [Unity](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/StyledText.html#supported-tags)/[TextMeshPro](http://digitalnativestudios.com/textmeshpro/docs/rich-text/) text formatting tag (in particular, [these ones](https://strikeline.myjetbrains.com/youtrack/issue/CH-226)). |

Syllables not joined together through any symbols should be separated by a space during parsing.

To display lyric phrases as just text, follow these steps:

1. Strip out `-`, `+`, `#`, `^`, `*`, `%`, `$`, and `/`.
2. Join together a syllable that has `-` at the end of it with the following syllable with a space between.
3. Replace `=` with a hyphen `-`, and join the syllable containing it with the following syllable without a space between. Make sure to not strip out hyphens added as replacement for equals.
4. Replace `§` and `_` with a space.
5. For `<>`, either display as-is (Rock Band 3 and earlier), replace `<>` with asterisks `**` (e.g. `<whistle>` -> `*whistle*`) (Rock Band 4), or strip out both `<>` and the text within (Clone Hero).
   - It may be possible to combine the first or second with the third, as there are a set amount of Unity/TextMeshPro text formatting tags, and none of them could be confused with verbs.

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
