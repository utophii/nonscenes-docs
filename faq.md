# FAQ and troubleshooting

## The plugin disables itself on start

Check the log above `Plugin will be disabled`:

- `packetevents is required and must be installed`: install
  [PacketEvents](https://modrinth.com/plugin/packetevents).
- `PacketEvents API is not initialized`: PacketEvents failed to start; look for
  its own errors earlier in the log, and make sure its version matches your
  server.

## Does it work on Spigot?

No. nonscenes uses Paper API. Paper forks such as Purpur work.

## Does it work on Folia?

Not yet.

## "Failed to initialize database, falling back to file storage"

The database in `storage` couldn't be reached. Cutscenes are saved to
`plugins/nonscenes/cutscenes/` until it works again and are migrated into the
database on the next successful start. See [Storage](storage.md#when-the-database-is-unavailable).

With PostgreSQL or MongoDB, also check that the server could download the
drivers from Maven Central on its first start.

## "Unable to play cutscene: world '…' is not loaded"

The cutscene was recorded in a world that isn't loaded, often after an import from
another server. Load the world, or re-record that part.

## The camera is jerky

- Raise `cutscene.playback.update-rate` (up to 240).
- For recordings, open the cutscene in the [keyframe editor](keyframe-editor.md)
  to smooth out shaky movement, or record with a higher
  `capture-interval-ticks` for a calmer path.
- Check the server's TPS: when the server lags, chunk loading along the path can
  lag behind the camera.

## A region trigger doesn't fire again

With `once: true` each player sees it once. Remove the player's entry from
`plugins/nonscenes/seen.yml`, or set `once: false`.

## A player is stuck after a crash

When a player joins after the server stopped mid-cutscene, they are teleported
back and their flight state is restored automatically. If that didn't happen, the
world they started in may not exist any more.

## Metrics

nonscenes uses [FastStats](https://faststats.dev) for anonymous usage statistics
and error reports, which help decide what to work on. It sends:

- the number of worlds, plugins and online players
- the number of cutscenes
- the storage type and fade mode
- stack traces of errors thrown by the plugin

nonscenes has no option to turn this off.
