# FoFiX Scripting

A script.txt file is used to lay out events that should happen at specific times.

## Format

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

Events may be spaced out with empty lines.

Example:

```
# Text event at 20 seconds that lasts for 4 seconds, which says "This is a text event"
20000	4000	text	This is a text event

# Picture at 25 seconds in that lasts for 10 seconds, which displays a picture included with the chart named image.png
25000	10000	pic	image.png
```

### Comments

Comments may be placed by starting a line with the hash `#` character.

Note that, [at least with how FoFiX parses them](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L2148), they *must* be at the start of a line, comments at the end of lines are not supported.

Example:

```
# This is a valid comment
20000	4000	text	This is a text event  # This is an invalid comment, and will be part of the text event
```

### Metadata

Script files may be headed with comments that serve as metadata. These comments follow this format:

`# [<id>:<text>]`

- `id` is a 2-character ID representing the type of metadata:
  - `ti` - Track title
  - `ar` - Artist name
  - `al` - Album name
  - `by` - Script author
- `text` is the text for the metadata.

Example:

```
# [ti:Example Song]
# [ar:Example Artist]
# [al:Example Album]
# [by:Example Author]
```

## Full Example

```
# [ti:Example Song]
# [ar:Example Artist]
# [al:Example Album]
# [by:Example Author]

# Text event at 20 seconds that lasts for 4 seconds, which says "This is a text event"
20000	4000	text	This is a text event

# Picture at 25 seconds in that lasts for 10 seconds, which displays a picture included with the chart named image.png
25000	10000	pic	image.png
```

## Resources

This information comes from referencing the FoFiX source code and a pre-existing script.txt file.

- [FoFiX ScriptReader class](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L2139)
- [FoFiX TextEvent class](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L978)
- [FoFiX PictureEvent class](https://github.com/fofix/fofix/blob/7730d1503c66562b901f62b33a5bd46c3d5e5c34/fofix/game/song/song.py#L987)
