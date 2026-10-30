# Timeline events

Events fire at a set time after the camera starts moving. They are stored with
the cutscene and survive editing, renaming, copying and export.

Needs `nonscene.event`. Works from the console too.

## Commands

```text
/ns event add <name> <type> <seconds> <data…>
/ns event list <name>
/ns event clear <name>
```

`<seconds>` may be a decimal, e.g. `2.5`. There is no command to remove a single
event; use `clear` and add the rest again, or edit an [exported](storage.md#export-and-import)
file.

## Event types

| Type | Data | Example |
|---|---|---|
| `title` | `<title>[\|<subtitle>]` | `/ns event add intro title 1 <gold>Welcome\|to the server` |
| `sound` | `<sound> [volume] [pitch]` | `/ns event add intro sound 0 entity.player.levelup 1 0.8` |
| `particle` | `<particle> [count]` | `/ns event add intro particle 4 heart 20` |
| `console` | `<command>` | `/ns event add intro console 10 give {player} bread 3` |
| `player` | `<command>` | `/ns event add intro player 10 spawn` |

- **title**: text before `|` is the title, after it the subtitle. Text that
  contains `<` is read as [MiniMessage](https://docs.advntr.dev/minimessage/format.html),
  otherwise as `&` color codes (including `&#rrggbb`). Shown for 0.5 s fade in,
  2 s stay, 0.5 s fade out.
- **sound**: a vanilla sound such as `entity.player.levelup` or
  `ENTITY_PLAYER_LEVELUP`, or a custom resource pack sound such as
  `mypack:intro.music`. Volume and pitch default to 1.
- **particle**: a Bukkit particle name such as `heart` or `HAPPY_VILLAGER`,
  spawned at the viewer's position. Count defaults to 10. Unknown names fall back
  to `FLAME`.
- **console**: runs as the console. The leading `/` is optional.
- **player**: runs as the viewer, with their permissions. Commands are otherwise
  blocked during a cutscene, but `player` events are not.

The particle and sound play at the player's real position, which during playback
follows the camera.

## Placeholders

All event data may use:

| Placeholder | Value |
|---|---|
| `{player}` | viewer's name |
| `{uuid}` | viewer's UUID |
| `{world}` | viewer's world |
| `{cutscene}` | cutscene name |

## Skipping and previews

- When a cutscene is skipped (`/ns skip` or sneaking), `console` and `player`
  events that haven't fired yet run immediately, in order. Titles, sounds and
  particles are dropped. So a reward given at the end of an intro can't be lost by
  skipping it.
- When a cutscene is stopped with `/ns stop`, by the API's `stop`, or the viewer
  leaves, remaining events don't run.
- `/ns preview` in the [keyframe editor](keyframe-editor.md) never runs `console`
  or `player` events.

## Title timing in YAML

Exported cutscenes store events like this. `fade-in`, `stay` and `fade-out` (in
ticks) can only be set here:

```yaml
events:
  '0':
    time-ms: 1000
    type: TITLE
    title: <gold>Welcome
    subtitle: to the server
    fade-in: 10
    stay: 60
    fade-out: 10
```
