# script.txt

A script.txt file is used to specify certain events that should happen at specific times.

## Basic Format

### Events

Events are laid out like this:

`<timestamp>	<length>	<type>	<data>`

Note that these values must be separated using full tabs.

- `timestamp` - (number) The timestamp of the event in milliseconds.
  - In the FoFiX source code, this value is treated as a float.
- `length` - (number) The length of the event in milliseconds.
  - In the FoFiX source code, this value is treated as a float.
- `type` - (string) The type of the event. Can be either `text`, or `pic` (picture/image).
- `data` - (string) The data for the event.
  - For `text`, this is the text to be displayed.
  - For `pic`, this is the file name of the image to be displayed.

### Miscellaneous

Lines can be empty.

Comments can be specified using a hash `#` character. They must be a line of their own, they cannot be placed at the end of a line.

Script files may be headed with comments that serve as metadata:

- `# [ti:Track Title]`
- `# [ar:Artist Name]`
- `# [al:Album Name]`
- `# [by:Script Author]`

## Example

```
# Text event at 20 seconds that lasts for 4 seconds, which says "This is a text event"
20000	4000	text	This is a text event

# Picture at 25 seconds in that lasts for 5 seconds, which links to a picture named image.png
25000	10000	pic	image.png
```

## Resources

This information comes from referencing the FoFiX source code and a pre-existing script.txt file.

- [FoFiX ScriptReader class]((https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L2139))
- [FoFiX TextEvent class](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L978)
- [FoFiX PictureEvent class](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L987)
