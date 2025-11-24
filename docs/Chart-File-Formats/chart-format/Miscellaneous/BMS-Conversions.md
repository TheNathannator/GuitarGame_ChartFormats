# BMS Conversions

This track comes from a couple of BME/BMS to .chart converters, linked in the [References](#references) section below. It seems that there is/was a planned project meant to make use of this format, but the status of that project is unknown.

## Track Names

- `ExpertBeatmania` - `ANOTHER` difficulty BMS track
- `HardBeatmania` - `HYPER` difficulty BMS track
- `MediumBeatmania` - `NORMAL` difficulty BMS track
- `EasyBeatmania` - `BEGINNER` difficulty BMS track

## Event Types

This track uses the `A` type code to mark auto-play notes (equivalent to BME channel 01), and also extends the `N` type code to include a keysound number after the length.

```
<Position> = A <Sound>
<Position> = N <Type> <Length> <Sound>
```

- `Sound` is a number indicating which keysound to play.

## Note and Modifier Types

| Note Type | Description                                 |
| :-------: | :----------                                 |
| 0         | Key 1<br>Equivalent to BME channel 11/51.   |
| 1         | Key 2<br>Equivalent to BME channel 12/52.   |
| 2         | Key 3<br>Equivalent to BME channel 13/53.   |
| 3         | Key 4<br>Equivalent to BME channel 14/54.   |
| 4         | Key 5<br>Equivalent to BME channel 15/55.   |
| 5         | Key 6<br>Equivalent to BME channel 18/58.   |
| 6         | Key 7<br>Equivalent to BME channel 19/59.   |
| 7         | Scratch<br>Equivalent to BME channel 16/56. |

## References

- https://github.com/iDestyKK/bme2chart
- https://github.com/iDestyKK/bme2chart_rx
