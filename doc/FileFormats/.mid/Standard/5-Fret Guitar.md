# 5-Fret Guitar in .mid

## Table of Contents

- [Track Names](#track-names)
- [Track Notes](#track-notes)
  - [Note Mechanics](#note-mechanics)
  - [Phrase Mechanics](#phrase-mechanics)
- [Track SysEx Events](#track-sysex-events)
  - [SysEx Event Mechanics](#sysex-event-mechanics)
- [Important Text Events](#important-text-events)

## Track Names

- `T1 GEMS` - Guitar Hero 1 Lead Guitar
  - While this track is uncommon and has some slightly different notes, the important parts are the same, and any important differences are noted later. It is safe to support the same way as the other tracks.
- `PART GUITAR` - Lead Guitar
- `PART GUITAR COOP` - Co-op Guitar
- `PART BASS` - Bass Guitar
- `PART RHYTHM` - Rhythm Guitar
- `PART KEYS` - 5-Lane Keys
  - While this track is not named for a guitar-type instrument, the game it comes from allows for playing it as if it were one.

## Track Notes

The notes listed here are not the only ones with meaning that may be seen. Notes that only pertain to Rock Band or Guitar Hero 1/2 are excluded here.

| MIDI Note | Description                         |
| :-------: | :----------                         |
| Markers   |                                     |
| 127       | Trill lane marker                   |
| 126       | Tremolo lane marker                 |
| 124       | Big Rock Ending marker 1            |
| 123       | Big Rock Ending marker 2            |
| 122       | Big Rock Ending marker 3            |
| 121       | Big Rock Ending marker 4            |
| 120       | Big Rock Ending marker 5            |
| 116       | Star Power marker                   |
| 106       | Player 2 versus phrase marker       |
| 105       | Player 1 versus phrase marker       |
| 104       | Tap note marker                     |
| 103       | Solo marker/GH1-2 Star Power marker |
|           |                                     |
| Expert    |                                     |
| 102       | Expert force strum                  |
| 101       | Expert force HOPO                   |
| 100       | Expert Orange (5th lane)            |
| 99        | Expert Blue (4th lane)              |
| 98        | Expert Yellow (3rd lane)            |
| 97        | Expert Red (2nd lane)               |
| 96        | Expert Green (1st lane)             |
| 95        | Expert Open*                        |
|           |                                     |
| Hard      |                                     |
| 90        | Hard force strum                    |
| 89        | Hard force HOPO                     |
| 88        | Hard Orange (5th lane)              |
| 87        | Hard Blue (4th lane)                |
| 86        | Hard Yellow (3rd lane)              |
| 85        | Hard Red (2nd lane)                 |
| 84        | Hard Green (1st lane)               |
| 83        | Hard Open*                          |
|           |                                     |
| Medium    |                                     |
| 78        | Medium force strum                  |
| 77        | Medium force HOPO                   |
| 76        | Medium Orange (5th lane)            |
| 75        | Medium Blue (4th lane)              |
| 74        | Medium Yellow (3rd lane)            |
| 73        | Medium Red (2nd lane)               |
| 72        | Medium Green (1st lane)             |
| 71        | Medium Open*                        |
|           |                                     |
| Easy      |                                     |
| 66        | Easy force strum                    |
| 65        | Easy force HOPO                     |
| 64        | Easy Orange (5th lane)              |
| 63        | Easy Blue (4th lane)                |
| 62        | Easy Yellow (3rd lane)              |
| 61        | Easy Red (2nd lane)                 |
| 60        | Easy Green (1st lane)               |
| 59        | Easy Open*                          |

### Note Mechanics

Notes are strum notes by default. They get turned into HOPOs (hammer-ons/pull-offs) automatically if they are close enough to the previous note, unless they are the same lane as the previous note, or are a chord. In .mid, the default threshold is `(<chart resolution> / 3) + 1` ticks, rounded down (the additional tick is for leniency, since some charts work better with it).

- Some sources say the threshold is, assuming a 480 tick resolution, either a 1/16th note (120 ticks), or 170 ticks instead of 160, but the modern threshold is the above formula.
- This threshold can be changed using the `hopo_frequency`, `hopofreq`, or `eighthnote_hopo` song.ini tags. The `hopo_frequency` tag is recommended above the others.

Notes can have their natural state overridden using the force strum/HOPO markers, referred to as forcing. Both single notes and chords can be forced, and it is possible to create same-fret consecutive HOPOs (both single and chord) through forcing.

Notes can be marked as tap notes by using either the tap note SysEx event, or the tap marker note. The latter is a newer method and is not supported by many games yet.

Sustains shorter than a 1/12th step are cut off and turned into a normal (non-sustain) note. This allows charters using a DAW to not have to make their notes 1 tick long in order for it to not be a sustain.

- This threshold can be changed using the `sustain_cutoff_threshold` song.ini tag.

Chords may have individual notes with different lengths. These are referred to as extended sustains (start at different times) and disjoint chords (start at the same time, end at different times).

Some .mid charts contain notes that are almost on the same position, but not quite. Guitar Hero 1/2 and Rock Band have a mechanism to snap notes with a position difference of 10 ticks or less together as a chord, with the position to snap to being the start position of the earliest note within the chord. Doing this snapping is not particularly recommended for new games since it'll rarely be an issue and sloppy charting should not be encouraged, but handling this in chart editors when importing is fine.

*The note-based open notes are disabled by default since note 59 is normally part of left hand animations in GH2/RB tracks. An `[ENHANCED_OPENS]` text event (with or without brackets) needs to be placed at the start of the track to enable them.

### Phrase Mechanics

Star Power phrases mark sections of the chart where the player may gain Star Power. When Star Power is activated, points gained from notes are doubled (this applies on top of the standard combo multiplier), and health gained from hit notes drastically increases.

Solo markers designate sections of the song where a part in the song is playing a solo. These sections display a percentage of notes hit, and award bonus points at the end of the solo based on the percentage of notes hit.

- For backwards-compatibility with GH1/2-era charts, solo markers should be treated as Star Power if there aren't any modern Star Power phrases placed on note 116. This behavior can be forced on or off by using the `multiplier_note` or `star_power_note` song.ini tags. The only valid options for these tags are 103 or 116: set to 103 to force this on, set to 116 to force it off.

Trill and tremolo lanes are used to make imprecise/indiscernible trills or strumming easier to play. They prevent overstrumming and only require you to hit faster than a certain threshold to hit the charted notes.

- Tremolo lanes can be used on chords.
- Trill lanes should only be used with two-note trills.
- These only apply to Expert unless they are marked at a velocity between 50-41, in which case it applies to Hard as well.

The versus phrases designate a section of the chart to be played by a specific player in certain 2-player versus modes. During player 1 phrases, only player 1 plays the chart, and during player 2 phrases, only player 2 plays the chart. Both phrases can occur at the same time to make both players play at the same time.

Big Rock Endings (BREs) are freestyle phrases at the end of songs with so-called "big rock endings" where the band rocks out for a period of time before playing their final notes. These freestyle phrases let players gain bonus points by playing whatever they want during the phrase. Following the phrase is at least one note, which the player must hit in order to earn those bonus points. If they miss or overstrum, they lose the points.

- BREs are started by placing a `[coda]` text event on the `EVENTS` track. Then, the duration of the BRE markers determines how long the freestyle section will last. All 5 BRE notes must be used to mark the phrase.
- After the freestyle phrase, there must be at least one note for the player to play. If no notes are present, the player cannot get their score.
- Notes still need to be charted under Guitar and Bass BRE sections for the purpose of character animations. In general, notes should still be charted regardless because not every game supports BREs.
- [More than one freestyle phrase may be placed during a BRE](https://youtube.com/watch?v=2iegD-LR8RE&t=208) (warning: this video is loud and the chart is awful), though this was probably never intended as it was never documented, and it might not work in some games. This is *very* edge-case, and almost no charts will have it.

## Track SysEx Events

| Modifier | Description |
| :------: | :---------- |
| `0x01`   | Open note   |
| `0x04`   | Tap note    |

### SysEx Event Mechanics

The open note SysEx event marks all notes within the phrase as an open note.

The tap note SysEx event marks all notes within the phrase as tap notes.

For maximum compatibility with programs, the SysEx-based markers for tap notes and open notes should be used when writing charts instead of the note-based markers, as the note-based markers are relatively newer and not supported by much yet.

## Important Text Events

| Event Text         | Description                           |
| :---------         | :----------                           |
| `[ENHANCED_OPENS]` | Enables note-based open note marking. |
