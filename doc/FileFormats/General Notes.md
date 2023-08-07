# General Notes

General notes that apply to most/all chart formats.

## Table of Contents

- [Tick \<-\> Time Conversions](#tick---time-conversions)
  - [Ticks to Time](#ticks-to-time)
  - [Time to Ticks](#time-to-ticks)

## Tick \<-\> Time Conversions

Here are some formulas for converting between ticks and time.

For these conversions to work, you must first determine the current tempo's starting position, and then calculate the difference between that and the position you wish to convert. If a tempo change happens at the same time as that position, you can use either the new tempo's position or the old tempo's position, they'll both work.

### Ticks to Time

$seconds = ({60 \div {beats \over minute}} \times {\Delta ticks \over resolution}) + seconds_\text{bpm}$

Breakdown of the formula:

- $60 = {seconds \over minute}$
- ${seconds \over beat} = {seconds \over minute} \div {beats \over minute}_\text{current}$
- $\Delta ticks = ticks_\text{target} - ticks_\text{bpm}$
- $\Delta beat = {\Delta ticks \over resolution}$
- $\Delta seconds = {seconds \over beat} \times \Delta beat$
- $seconds = \Delta seconds + seconds_\text{bpm}$

### Time to Ticks

$ticks = (\Delta seconds \div (60 \div {beats \over minute}) \times resolution) + ticks_\text{bpm}$

Breakdown of the formula:

- $60 = {seconds \over minute}$
- ${seconds \over beat} = {seconds \over minute} \div {beats \over minute}_\text{current}$
- $\Delta seconds = seconds_\text{target} - seconds_\text{bpm}$
- $\Delta beat = {\Delta seconds \div {seconds \over beat}}$
- $\Delta ticks = \Delta beat \times resolution$
- $ticks = \Delta ticks + ticks_\text{bpm}$
