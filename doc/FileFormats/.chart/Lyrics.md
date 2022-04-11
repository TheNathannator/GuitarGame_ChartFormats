# Lyrics in .chart

This document details the format for lyrics in .chart files.

## Table of Contents

- [Events Section](#events-section)
  - [Lyrics](#lyrics)
- [Example](#example)

## Events Section

Lyrics are charted as global events in the `[Events]` section.

### Lyrics

Lyrics are laid out as `phrase_start`, `phrase_end`, and `lyric` events throughout the `Events` section.

| Event Name         | Description                                                                                                     |
| :---               | :----------                                                                                                     |
| `phrase_start`     | Marks the start of a new lyrics phrase.<br>Can be used to mark a new phrase without using a prior `phrase_end`. |
| `phrase_end`       | Marks the end of the current lyrics phrase.                                                                     |
| `lyric <syllable>` | Contains a syllable of a lyrics phrase.                                                                         |

- `phrase_start` marks the start of a phrase of lyrics A preceding `phrase_end` is not required to start a new phrase from an existing one.
- `phrase_end` marks the end of a phrase.
- Any `lyric` events between a `phrase_start` and either a `phrase_end` event or a new `phrase_start` event should be concatenated into a single line. The text of this phrase may be highlighted one syllable at a time as `lyric` events are passed.

There are various symbols used for specific things in lyrics:

| Name           | Symbol | Description                                                                                               |
| :---           | :----: | :----------                                                                                               |
| Hyphens        | `-`    | Indicates that this syllable should be combined with the next.                                            |
| Pluses         | `+`    | In Rock Band, will connect the previous note and the current note into a slide. Usually found standalone. |
| Equals         | `=`    | Indicates that a syllable should be joined with the next using a literal hyphen.                          |
| Pounds         | `#`    | In Rock Band, this marks a note as non-pitched.                                                           |
| Carets         | `^`    | In Rock Band, this marks a note as non-pitched with a more generous scoring.<br>Typically used on short syllables or syllables without sharp attacks. |
| Asterisks      | `*`    | In Rock Band, this marks a note as non-pitched, but their differentiation is unknown.                     |
| Percents       | `%`    | In Rock Band, these are a range divider marker for vocals parts with large octave ranges.                 |
| Sections       | `§`    | In Rock Band, these are used to indicate that two syllables are sung as a single syllable, used in some Spanish lyrics. These are displayed with a tie character `‿` in RB. |
| Dollars        | `$`    | In Rock Band, these are used in harmonies to mark the syllables they are part of to be hidden.<br>In one case it is also on the standard Vocals track for some reason, it appears to not do anything in RB here. |
| Slashes        | `/`    | These appear in some RB charts for an unknown reason, mainly The Beatles: Rock Band.<br>They are also used in a workaround to use quotation marks in global events in older versions of Clone Hero. |
| Underscores    | `_`    | These are replaced with a space by Clone Hero for the purpose of having a character that does that.       |
| Angle brackets | `<>`   | In Clone Hero's public test build, some [formatting tags](http://digitalnativestudios.com/textmeshpro/docs/rich-text/) are supported in lyrics. For other games that don't use these tags, they need to be stripped out. |

Syllables not joined together through any symbols should be separated by a space during parsing.

Here's how these symbols should be handled for displaying as just text:

- Strip out `-`, `+`, `#`, `^`, `*`, `%`, `$`, `/`, `<>`, and anything between `<>`.
- Replace `=` with a hyphen `-`. Make sure to not strip out hyphens added as replacement for equals.
- Replace `§`, `_` with a space.
- Join together a syllable that has `-` or `=` at the end of it with the following syllable.

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
