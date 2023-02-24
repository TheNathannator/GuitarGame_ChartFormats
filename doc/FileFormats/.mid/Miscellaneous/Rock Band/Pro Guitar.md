# Rock Band Pro Guitar/Bass

- `PART REAL_GUITAR` - Pro Guitar (17-Fret)
- `PART REAL_GUITAR_22` - Pro Guitar (22-Fret)
- `PART REAL_BASS` - Pro Bass (17-Fret)
- `PART REAL_BASS_22` - Pro Bass (22-Fret)

Only the new/unsupported events and other differences are listed here, to avoid duplicating information.

## Text Events

Character animation text events are inherited from the `PART GUITAR`/`PART BASS` track, they are not placed on any of the Pro Guitar/Bass tracks.

Song trainers:

- Not usable in RBN2/custom charts, only in custom Pro upgrades

| Event Text                            | Description                                                    |
| :---------                            | :----------                                                    |
| `[begin_pg song_trainer_pg_<number>]` | Marks the start of a Pro Guitar trainer section.<br>`number` is an index for the trainer, starting at 1. |
| `[end_pg song_trainer_pg_<number>]`   | Marks the end of a Pro Guitar trainer section.                 |
| `[pg_norm song_trainer_pg_<number>]`  | Marks a seamless loop point for a Pro Guitar trainer section.  |
|                                       |                                                                |
| `[begin_pb song_trainer_pb_<number>]` | Marks the start of a Pro Bass trainer section.                 |
| `[end_pb song_trainer_pb_<number>]`   | Marks the end of a Pro Bass trainer section.                   |
| `[pb_norm song_trainer_pb_<number>]`  | Marks a seamless loop point for a Pro Bass trainer section.    |
