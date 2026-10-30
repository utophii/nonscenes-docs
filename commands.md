# Commands

Main command: `/nonscene`, alias `/ns`. Running it without arguments shows the
help.

Every subcommand needs `nonscene.use` (the permission on the command itself) plus
the permission listed below. See [Permissions](permissions.md).

**Console**: `play`, `skip`, `pause`, `resume`, `event`, `rename`, `copy`,
`delete`, `export`, `import` and `reload` work from the console. The rest are
player-only.

## Recording and playback

| Command | Description | Permission |
|---|---|---|
| `/ns start <name> <seconds>` | Record a cutscene for 1-300 seconds after a countdown. [More](recording.md) | `nonscene.start` |
| `/ns stop` | Cancel your own recording, playback or path preview. | `nonscene.stop` |
| `/ns play <name> [player]` | Play a cutscene. From the console `player` is required. [More](playback.md) | `nonscene.play`, `nonscene.play.others` |
| `/ns skip [player]` | Skip to the end. Pending command events still run. | `nonscene.skip`, `nonscene.skip.others` |
| `/ns pause [player]` | Freeze the camera and timeline. | `nonscene.pause`, `nonscene.pause.others` |
| `/ns resume [player]` | Continue a paused cutscene. | `nonscene.pause`, `nonscene.pause.others` |
| `/ns showpath <name>` | Draw the path with particles. | `nonscene.showpath` |

## Managing cutscenes

| Command | Description | Permission |
|---|---|---|
| `/ns all` | List cutscenes with their frame counts. | `nonscene.list` |
| `/ns rename <name> <new-name>` | Rename. Lists triggers that still use the old name. | `nonscene.rename` |
| `/ns copy <name> <new-name>` | Copy, including timeline events. | `nonscene.copy` |
| `/ns delete <name>` | Asks for confirmation, then run `/ns delete <name> confirm` (or click the button) within 30 s. | `nonscene.delete` |
| `/ns export <name>` | Write `plugins/nonscenes/exports/<name>.yml`. [More](storage.md#export-and-import) | `nonscene.export` |
| `/ns import <file> [new-name]` | Load a file from `exports/`. `.yml` is optional. | `nonscene.import` |
| `/ns reload` | Reload `config.yml`, `messages.yml` and triggers. Storage changes need a restart. | `nonscene.reload` |

A cutscene that is being played, recorded or edited can't be renamed or deleted.

## Timeline events

| Command | Description |
|---|---|
| `/ns event add <name> <type> <seconds> <data…>` | Add an event. Types: `title`, `sound`, `particle`, `console`, `player`. |
| `/ns event list <name>` | List events. |
| `/ns event clear <name>` | Remove all events. |

Permission: `nonscene.event`. See [Timeline events](timeline-events.md).

## Keyframe editor

Player-only, permission `nonscene.edit`. See [Keyframe editor](keyframe-editor.md).

| Command | Description |
|---|---|
| `/ns edit <name> [tolerance]` | Open a cutscene, or create a new one. |
| `/ns key add [#]` | Add a keyframe at your position and view. |
| `/ns key move <#>` | Move a keyframe to your position and view. |
| `/ns key remove <#>` | Remove a keyframe. |
| `/ns key speed <#> <seconds>` | Duration from keyframe `#` to the next. |
| `/ns key tp <#>` | Teleport to a keyframe. |
| `/ns key list` | List keyframes. |
| `/ns preview` | Watch the draft. |
| `/ns save` | Save the draft. |
| `/ns discard` | Throw the draft away. |

## Commands during a cutscene

Viewers can only run `/ns stop`, `/ns skip`, `/ns pause` and `/ns resume`. All
other commands are blocked and hidden from tab completion until the cutscene
ends.
