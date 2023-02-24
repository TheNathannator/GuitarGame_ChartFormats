# Dance in .mid

This track is for dance pad modes. It was created after [a discussion](https://www.fretsonfire.org/forums/viewtopic.php?f=28&t=51670) on the Frets on Fire forums started by a member of the StepMania Online community, and a preliminary format was documented on both the [FoF forums](https://www.fretsonfire.org/forums/viewtopic.php?f=28&t=51810) and the [Phase Shift forums](https://dwsk.proboards.com/thread/404/song-standard-advancements).

So far the only implementations of the format are in Phase Shift and Editor on Fire, and the format doesn't seem to have been fleshed out very much. It may have been designed to support up to 10 lanes for each difficulty, but things are vague and nothing of the sort is stated explicitly, nor is there any (known) way of determining which type of mode a chart is, so for the purposes of documentation, this track is assumed to have only supported 4-lane dance.

## Table of Contents

- [Track Name](#track-name)
- [Track Notes](#track-notes)
- [Track Channels](#track-channels)
- [Track Velocities](#track-velocities)

## Track Name

- `PART DANCE` - Dance

## Track Notes

| MIDI Note | Description              |
| :-------: | :----------              |
| Markers   |                          |
| 116       | Star Power marker        |
| 103       | Solo marker              |
|           |                          |
| Challenge |                          |
| 99        | Challenge right (lane 4) |
| 98        | Challenge up (lane 3)    |
| 97        | Challenge down (lane 2)  |
| 96        | Challenge left (lane 1)  |
|           |                          |
| 87        | Hard right (lane 4)      |
| 86        | Hard up (lane 3)         |
| 85        | Hard down (lane 2)       |
| 84        | Hard left (lane 1)       |
|           |                          |
| 75        | Medium right (lane 4)    |
| 74        | Medium up (lane 3)       |
| 73        | Medium down (lane 2)     |
| 72        | Medium left (lane 1)     |
|           |                          |
| 63        | Easy right (lane 4)      |
| 62        | Easy up (lane 3)         |
| 61        | Easy down (lane 2)       |
| 60        | Easy left (lane 1)       |
|           |                          |
| Beginner  |                          |
| 51        | Beginner right (lane 4)  |
| 50        | Beginner up (lane 3)     |
| 49        | Beginner down (lane 2)   |
| 48        | Beginner left (lane 1)   |

## Track Channels

| MIDI Channel | Description |
| :----------: | :---------- |
| 0            | Normal note |
| 1            | Mine        |
| 2            | Lift        |
| 3            | Roll        |

## Track Velocities

Only one velocity level is defined, and chances are this definition was not respected. It is only listed here for completeness.

| MIDI Velocity | Description |
| :-----------: | :---------- |
| 127           | Normal note |
