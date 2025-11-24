# FoFiX Careers

FoFiX has a career mode that enables song packs to configure sorting and unlock conditions.

## Structure

Careers are structured using multiple tiers of songs. In the career folder, there is a titles.ini file which specifies display names and tier IDs. Then, each song within the career contains a couple tags in its song.ini file indicating which tier it belongs to, and any tier that needs to be completed before the song is unlocked.

## titles.ini

The titles.ini file is headed with a `[titles]` section. It has one entry:

| Tag Name   | Description                                                       | Data type |
| :-------   | :----------                                                       | :-------: |
| `sections` | A space-separated list of .ini sections to include in the career. | string    |

This entry points to other sections that should be used as part of the career. Each of those sections has two entries:

| Tag Name    | Description               | Data type |
| :-------    | :----------               | :-------: |
| `name`      | Display name of the tier. | string    |
| `unlock_id` | Name used for associating a song with this tier, and for checking unlock requirements. | string |

## song.ini Tags

| Tag Name           | Description                                                              | Data type |
| :-------           | :----------                                                              | :-------: |
| `unlock_id`        | Name of the career tier that this song belongs to.                       | string    |
| `unlock_require`   | Name of the career song/tier that must be completed to unlock this song. | string    |
| `unlock_text`      | Text to display if the song is locked.                                   | string    |
| `unlock_completed` | Indicates whether or not the song is unlocked. (*not* a boolean value)   | number    |

`unlock_id` can be appended with `.<song number>` to respectively sort songs in a specific order, and with `enc` (affixed before affixing the song number) to tag songs as an encore song (though FoFiX doesn't seem to check for this suffix at all, so it's likely also just for sorting, and special unlocks are done manually). Examples: `gh3_1.5`, `gh3_2enc.2`

The value of `unlock_completed` is determined by removing the song order prefix from the song name using a regular expression of `^[0-9]+\. *`, then multiplying the length of the result by the length of the artist name, then adding 1.

## Example

```
My Career
|- titles.ini
   ---
   [titles]
   sections = tier01 tier02 tier03
  
   [tier01]
   name = 01. Getting Started
   unlock_id = tier_01
 
   [tier02]
   name = 02. Warmed Up
   unlock_id = tier_02
 
   [tier03]
   name = 03. Rockin' Out
   unlock_id = tier_03
   ---
|- Tier 01
   |- Song 01
      |- notes.mid
      |- song.ogg
      |- song.ini
         ---
         [song]
         ...
         unlock_id = tier_01
         # No unlock requirements since it's the first tier
         ---
   |- Song 02
      |- notes.mid
      |- song.ogg
      |- song.ini
         ---
         [song]
         ...
         unlock_id = tier_01
         ---
|- Tier 02
   |- Song 01
      |- notes.mid
      |- song.ogg
      |- song.ini
         ---
         [song]
         ...
         unlock_id = tier_02
         unlock_require = tier_01
         unlock_text = Complete tier "01. Getting Started" to unlock this song!
         ---
   |- Song 02
      |- notes.mid
      |- song.ogg
      |- song.ini
         ---
         [song]
         ...
         unlock_id = tier_02
         unlock_require = tier_01
         unlock_text = Complete tier "01. Getting Started" to unlock this song!
         ---
|- Tier 03
   |- Song 01
      |- notes.mid
      |- song.ogg
      |- song.ini
         ---
         [song]
         ...
         unlock_id = tier_03
         unlock_require = tier_02
         unlock_text = Complete tier "02. Warmed Up" to unlock this song!
         ---
   |- Song 02
      |- notes.mid
      |- song.ogg
      |- song.ini
         ---
         [song]
         ...
         unlock_id = tier_03
         unlock_require = tier_02
         unlock_text = Complete tier "02. Warmed Up" to unlock this song!
         ---
```

## References

- https://fofix.readthedocs.io/en/latest/users/careers.html
