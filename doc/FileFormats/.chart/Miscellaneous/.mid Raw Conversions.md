# MIDI Track Names as Instrument Tracks

Some .chart files may have section names that match those of some .mid tracks. This appears to be [a behavior in older versions of Moonscraper](https://github.com/FireFox2000000/Moonscraper-Chart-Editor/blob/c240a72ec78011e2a38453a4d2f85f2f6275c052/Moonscraper%20Chart%20Editor/Assets/Scripts/Game/Charts/IO/Chart/ChartWriter.cs#L229), done in an attempt to preserve unknown track data as best as possible.

Unfortunately, very little of the MIDI data is actually preserved. Only notes and text events are converted, and only the text and note number/length are stored: note velocity/channel and meta event type are discarded. This makes them not really all that useful for tracks that depend on this information, such as Pro Guitar.

Example (truncated from an existing chart that has this):

```
[Song]
{
  Name = "midi_export"
  Offset = 0
  Resolution = 960
  Player2 = bass
  Difficulty = 0
  PreviewStart = 0
  PreviewEnd = 0
  Genre = "rock"
  MediaType = "cd"
  MusicStream = "song.ogg"
}
[SyncTrack]
{
  0 = TS 4
  0 = B 67000
  7680 = B 67416
  12480 = B 69011
  15360 = B 65603
  19200 = B 66342
  23040 = B 66051
  26880 = B 67640
  30720 = B 66934
  34560 = B 68250
  38400 = B 65953
  42240 = B 68189
  46080 = B 67167
  49920 = B 66798
  61440 = B 67414
  69120 = B 67097
  76800 = B 67111
  80640 = B 64017
  84480 = B 65333
  88320 = B 65945
  90240 = B 66690
  92160 = B 69520
  96000 = B 66455
  98880 = B 67621
}
[Events]
{
  7602 = E "crowd_noclap"
  7680 = E "section intro"
  9600 = E "music_start"
  38400 = E "section verse_1"
  61440 = E "section verse_2"
  96000 = E "section chorus_1"
  122880 = E "section chorus_2"
  149760 = E "section bridge_1"
  174720 = E "crowd_noclap"
  174720 = E "section gtr_break_1"
  182400 = E "crowd_clap"
  182400 = E "section gtr_solo_1"
  205440 = E "section chorus_3"
  232320 = E "section postchorus_1"
  251520 = E "section bridge_2"
  276480 = E "crowd_noclap"
  276480 = E "section gtr_break_2"
  284160 = E "crowd_clap"
  284160 = E "section gtr_solo_2"
  341760 = E "crowd_noclap"
  343680 = E "music_end"
  352318 = E "end"
}
[ExpertSingle]
{
  7680 = N 0 360
  7680 = N 1 360
  7680 = N 3 360
  7680 = E mellow
  8160 = N 0 0
  8400 = N 0 0
  8400 = N 1 0
  8400 = N 3 0
  8640 = N 0 360
  8640 = N 1 360
  8640 = N 3 360
  9120 = N 0 0
  9360 = N 0 0
  9360 = N 1 0
  9360 = N 3 0
  9600 = N 0 0
  9840 = N 0 0
  9840 = N 1 0
  9840 = N 3 0
  10080 = N 0 0
  10080 = N 1 0
}
[ExpertDoubleBass]
{
  21120 = E map HandMap_DropD2
  22080 = N 4 720
  22080 = E mellow
  23040 = N 3 0
  23206 = N 1 0
  23360 = N 3 0
  23440 = N 1 0
  23520 = N 3 0
  23600 = N 1 0
  23680 = N 3 0
  23760 = N 1 0
  23840 = N 3 0
  23920 = N 1 0
  24000 = N 3 0
  24080 = N 1 0
  24160 = N 3 0
  24240 = N 1 0
  24320 = N 3 0
  24400 = N 1 0
  24480 = N 3 0
  24560 = N 1 0
}
[PART REAL_BASS]
{
  22080 = N 74 840
  22080 = N 79 840
  22080 = N 98 840
  22080 = N 103 840
  22080 = N 108 120
  23040 = N 26 2400
  23040 = N 50 2880
  23040 = N 74 60
  23040 = N 98 60
  23040 = N 108 60
  23040 = N 126 3440
  23040 = E begin_pb song_trainer_pb_1
  23160 = N 98 60
  23200 = N 74 60
}
[PART REAL_GUITAR]
{
  7680 = N 5 120
  7680 = N 16 120
  7680 = N 24 120
  7680 = N 48 120
  7680 = N 49 120
  7680 = N 72 120
  7680 = N 73 120
  7680 = N 74 120
  7680 = N 75 120
  7680 = N 76 120
  7680 = N 77 120
  7680 = N 96 120
  7680 = N 97 120
  7680 = N 98 120
  7680 = N 99 120
  7680 = N 100 120
  7680 = N 101 120
  7680 = N 108 120
  7680 = E begin_pg song_trainer_pg_1
  7680 = E chrd1 F5
  8160 = N 72 120
}
[PART REAL_KEYS_X]
{
  0 = N 9 240
  23040 = N 64 3600
  26880 = N 59 3600
  30720 = N 64 3600
  34560 = N 71 3600
  38400 = N 60 3600
  38400 = N 64 1920
  42240 = N 59 3360
  46080 = N 60 3600
  46080 = N 64 1920
  46080 = N 116 7680
  49920 = N 59 3600
  49920 = N 71 1920
  53760 = N 60 3600
  53760 = N 64 1920
  57600 = N 62 3600
  61440 = N 60 3600
}
```
