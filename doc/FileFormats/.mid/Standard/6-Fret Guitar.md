# 6-Fret Guitar in .mid

## Table of Contents

- [Track Names](#track-names)
- [Track Notes](#track-notes)
  - [Note Mechanics](#note-mechanics)
  - [Phrase Mechanics](#phrase-mechanics)
- [Track SysEx Events](#track-sysex-events)
  - [SysEx Event Mechanics](#sysex-event-mechanics)

## Track Names

- `PART GUITAR GHL` - 6-Fret Lead Guitar
- `PART BASS GHL` - 6-Fret Bass Guitar
- `PART RHYTHM GHL` - 6-Fret Rhythm Guitar
- `PART GUITAR COOP GHL` - 6-Fret Co-op Guitar

## Track Notes

| MIDI Note | Description               |
| :-------: | :----------               |
| Markers   |                           |
| 116       | Star Power marker         |
| 104       | Tap note marker           |
| 103       | Solo marker               |
|           |                           |
| Expert    |                           |
| 102       | Expert force strum        |
| 101       | Expert force HOPO         |
| 100       | Expert Black 3 (3rd lane) |
| 99        | Expert Black 2 (2nd lane) |
| 98        | Expert Black 1 (1st lane) |
| 97        | Expert White 3 (6th lane) |
| 96        | Expert White 2 (5th lane) |
| 95        | Expert White 1 (4th lane) |
| 94        | Expert Open               |
|           |                           |
| Hard      |                           |
| 90        | Hard force strum          |
| 89        | Hard force HOPO           |
| 88        | Hard Black 3 (3rd lane)   |
| 87        | Hard Black 2 (2nd lane)   |
| 86        | Hard Black 1 (1st lane)   |
| 85        | Hard White 3 (6th lane)   |
| 84        | Hard White 2 (5th lane)   |
| 83        | Hard White 1 (4th lane)   |
| 82        | Hard Open                 |
|           |                           |
| Medium    |                           |
| 78        | Medium force strum        |
| 77        | Medium force HOPO         |
| 76        | Medium Black 3 (3rd lane) |
| 75        | Medium Black 2 (2nd lane) |
| 74        | Medium Black 1 (1st lane) |
| 73        | Medium White 3 (6th lane) |
| 72        | Medium White 2 (5th lane) |
| 71        | Medium White 1 (4th lane) |
| 70        | Medium Open               |
|           |                           |
| Easy      |                           |
| 66        | Easy force strum          |
| 65        | Easy force HOPO           |
| 64        | Easy Black 3 (3rd lane)   |
| 63        | Easy Black 2 (2nd lane)   |
| 62        | Easy Black 1 (1st lane)   |
| 61        | Easy White 3 (6th lane)   |
| 60        | Easy White 2 (5th lane)   |
| 59        | Easy White 1 (4th lane)   |
| 58        | Easy Open                 |

### Note Mechanics

Notes are strum notes by default. They get turned into HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord. In .mid, the default threshold is `(<chart resolution> / 3) + 1` ticks, rounded down (the additional tick is for leniency, since some charts work better with it).

- This threshold can be changed using the `hopo_frequency`, `hopofreq`, or `eighthnote_hopo` song.ini tags.

Notes can have their natural state overridden using the force strum/HOPO markers, referred to as forcing. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.

Sustains shorter than a 1/12th step are cut off and turned into a normal (non-sustain) note. This allows charters using a DAW to not have to make their notes 1 tick long in order for it to not be a sustain.

- This threshold can be changed using the `sustain_cutoff_threshold` song.ini tag.

Chords may have individual notes with different lengths. These are referred to as extended sustains (start at different times) and disjoint chords (start at the same time, end at different times).

The `[ENHANCED_OPENS]` text event is *not* required for 6-fret note-based open notes.

### Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

Solo markers designate sections of the song where a part in the song is playing a solo. These sections display a percentage of notes hit, and award bonus points at the end of the solo based on the percentage of notes hit.

- The backwards-compatibility needs of 5-fret guitar are not present in 6-fret guitar.

## Track SysEx Events

These are carried over from 5-fret guitar.

| Modifier | Description |
| :------: | :---------- |
| `0x01`   | Open note   |
| `0x04`   | Tap note    |

### SysEx Event Mechanics

The open note SysEx event marks all notes within the phrase as an open note, not just green notes.

The tap note SysEx event marks all notes within the phrase as tap notes.

For maximum compatibility with programs, the SysEx-based markers for tap notes should be used when writing charts instead of the note-based markers, as the note-based markers are relatively newer and not supported by much yet. When writing a 6-fret track, the open note SysEx markers can be safely not used, as the note-based opens have been there since the start of the 6-fret format.
