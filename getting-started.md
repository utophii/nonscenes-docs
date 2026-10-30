# Getting started

## Requirements

| | |
|---|---|
| Server | Paper 1.20-1.21.x or a Paper fork (Spigot is not supported) |
| Java | 21 |
| Required plugin | [PacketEvents](https://modrinth.com/plugin/packetevents) 2.7+ |
| Optional | Any permission plugin, e.g. LuckPerms |

Without PacketEvents the plugin logs `packetevents is required and must be
installed` and disables itself.

## Installation

1. Install PacketEvents.
2. Put `nonscenes-<version>.jar` into `plugins/`.
3. Start the server.

On first start the plugin creates:

```text
plugins/nonscenes/
├── config.yml      settings, triggers, storage
├── messages.yml    every chat message
└── cutscenes.db    SQLite database (default storage)
```

`exports/`, `cutscenes/` and `seen.yml` appear later when they are needed, see
[Storage](storage.md) and [Triggers](triggers.md).

PostgreSQL and MongoDB drivers and HikariCP are not inside the jar. Paper
downloads them from Maven Central on start (`libraries` in `plugin.yml`), so the
server needs internet access at least once. SQLite and MySQL drivers ship with
Paper.

## Your first cutscene

1. Go to where the cutscene should start and fly (creative or spectator works best).
2. Run `/ns start tour 20`. After a 3 second countdown the plugin records your
   position and view for 20 seconds. A boss bar shows the progress.
3. Fly the path. Move slowly and turn your head smoothly: what you do is exactly
   what the camera will do.
4. Run `/ns play tour` to watch it. Sneak to skip.

Something not quite right? Open it in the [keyframe editor](keyframe-editor.md)
with `/ns edit tour`, adjust single points and timings, then `/ns save`.

## Next steps

- Show a title when the camera reaches the spawn: [Timeline events](timeline-events.md)
- Play it for every new player: [Triggers](triggers.md)
- Use a black screen from your resource pack instead of blindness: [Playback → Fades](playback.md#fades)
