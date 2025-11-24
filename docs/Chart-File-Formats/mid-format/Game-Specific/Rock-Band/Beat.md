# Rock Band Beat Track

The `BEAT` track marks where upbeats and downbeats in the song are, separately from the tempo and time signature. This drives character animations, lighting, the crowd, and how quickly Overdrive depletes.

Rock Band requires that the last note in this track must occur one beat before the `[end]` event on the `EVENTS` track.

## MIDI Notes

| MIDI Note | Description                           |
| :-------: | :----------                           |
| 13        | Weak beat (other beats)               |
| 12        | Strong beat (first beat of a measure) |
