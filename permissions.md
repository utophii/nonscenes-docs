# Permissions

All permissions default to **op**. `nonscene.use` is required for every
subcommand, because it is the permission of `/nonscene` itself.

| Permission | Allows |
|---|---|
| `nonscene.admin` | Everything below |
| `nonscene.use` | Using `/nonscene` at all |
| `nonscene.start` | Recording cutscenes |
| `nonscene.stop` | Cancelling your own recording, playback or path preview |
| `nonscene.play` | Playing cutscenes for yourself |
| `nonscene.play.others` | Playing cutscenes for other players |
| `nonscene.skip` | Skipping your own cutscene, by command or by sneaking |
| `nonscene.skip.others` | Skipping another player's cutscene |
| `nonscene.pause` | Pausing and resuming your own cutscene |
| `nonscene.pause.others` | Pausing and resuming another player's cutscene |
| `nonscene.list` | `/ns all` |
| `nonscene.showpath` | `/ns showpath` |
| `nonscene.edit` | The keyframe editor |
| `nonscene.event` | Timeline events |
| `nonscene.rename` | Renaming cutscenes |
| `nonscene.copy` | Copying cutscenes |
| `nonscene.delete` | Deleting cutscenes |
| `nonscene.export` | Exporting cutscenes |
| `nonscene.import` | Importing cutscenes |
| `nonscene.reload` | `/ns reload` |

## Typical setups

**Players can skip cutscenes they're shown, by sneaking or with `/ns skip`** (LuckPerms):

```text
/lp group default permission set nonscene.use true
/lp group default permission set nonscene.skip true
```

Sneak-to-skip only checks `nonscene.skip`; `nonscene.use` is only needed to type
`/ns skip`. Triggers and `/ns play <name> <player>` from the console or another
plugin don't need any permission from the viewer.

**Builders who make cutscenes but can't delete or force them on others:**

```text
/lp group builder permission set nonscene.use true
/lp group builder permission set nonscene.start true
/lp group builder permission set nonscene.stop true
/lp group builder permission set nonscene.play true
/lp group builder permission set nonscene.list true
/lp group builder permission set nonscene.showpath true
/lp group builder permission set nonscene.edit true
/lp group builder permission set nonscene.event true
```

`nonscene.event` lets a builder add `console` events, which run with full console
rights when the cutscene plays. Only give it to people you'd trust with the
console.

Commands created by [triggers](triggers.md#custom-commands) need no permission.
