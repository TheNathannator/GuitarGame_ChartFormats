# info.json

Encore originally used an `info.json` file for storing metadata. It is considered deprecated in favor of a typical song.ini, as it lacks important metadata tags and stopped being maintained due to being annoying to work with in code.

{
  // Title of the song.
  "name": string,
  // Artist(s) or band(s) behind the song.
  "artist": string,
  // The album the song is featured in.
  "album": string,
  // Year of the song's release.
  "release_year": number,
  // Length of the song in seconds.
  "length": number,

  // Genre(s) of the song.
  "genres": string[],
  // Community member(s) responsible for charting the song.
  "charters": string[],
  // Name of an icon image to display for this song. Included in either the chart folder or the game the chart was made for, or sourced from this repository of icons - https://opensource.yarg.in/
  "source": string,
  // Timestamp in milliseconds where the song preview starts. 
  "preview_start_time": number,

  // Likely used to be for setting the icon of of a instrument, has no effect in-game.
  "icon_drums": string,
  "icon_bass": string,
  "icon_lead": string,
  "icon_vocals": string,
  "icon_keyboard": string,

  // Difficulties for each instrument track in the chart.
  "diff": {
    "guitar": number,
    "bass": number,
    "drums": number,
    "vocals": number,
    "keys": number,
    "plastic_guitar": number,
    "plastic_bass": number,
    "plastic_drums": number,
    "pitched_vocals": number,
    "plastic_vocals": number,
    "plastic_keys": number
  },
  // File names for audio stems.
  "stems": {
    "backing": string,
    "guitar": string,
    "bass": string,
    "drums": string,
    "vocals": string,
    "keys": string
  }
}
