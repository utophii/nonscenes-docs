# Playback

```text
/ns play <name> [player]
```

Needs `nonscene.play`; playing for someone else also needs `nonscene.play.others`.
From the console the player argument is required.

## What happens

1. The chunks along the path are loaded.
2. The screen fades out (see [Fades](#fades)).
3. The viewer is hidden from other players, made to fly and teleported to the
   start of the path. They now look through a camera that exists only in their
   own client: no hotbar, no hand, no way to look around.
4. The camera follows the path, updated `update-rate` times per second on its own
   thread. [Timeline events](timeline-events.md) fire as their time comes.
5. The screen fades out again, the player is put back exactly where they started,
   with their flight state restored, and fades back in.

The player entity follows the camera on the server so that chunks keep loading and
sounds and particles are heard and seen. Cutscenes may cross worlds.

## Restrictions for the viewer

While watching, a player can't:

- move horizontally, interact with blocks, entities or armor stands
- open, click or drag in inventories, drop, pick up or swap items
- deal or take damage or get hungry
- leave a vehicle
- run any command except `/ns stop`, `/ns skip`, `/ns pause`, `/ns resume`
  (other commands are also hidden from tab completion)

## Skip, pause, stop

| Action | How | Permission |
|---|---|---|
| Skip | `/ns skip [player]` or sneak | `nonscene.skip`, `nonscene.skip.others` |
| Pause / resume | `/ns pause [player]`, `/ns resume [player]` | `nonscene.pause`, `nonscene.pause.others` |
| Stop | `/ns stop` | `nonscene.stop` |

- **Skip** ends the cutscene as completed: `console` and `player` events that
  haven't fired run right away. Sneak-to-skip can be turned off with
  `cutscene.playback.skip-on-sneak: false`.
- **Pause** freezes both the camera and the timeline. It works only while the
  camera moves, not during the fades around it.
- **Stop** cancels the cutscene; remaining events don't run.

## Fades

```yaml
cutscene:
  playback:
    fade:
      mode: BLINDNESS
      out-ticks: 10   # fade to black
      hold-ticks: 10  # stay black while the camera switches
      in-ticks: 20    # fade back in
      title-glyph: ""
```

| Mode | Effect |
|---|---|
| `NONE` | Hard cut. |
| `BLINDNESS` | Blindness fog. Works without a resource pack, the client eases out of it on its own. |
| `TITLE` | Shows `title-glyph` as a title, faded with the given timings. Use a full-screen black glyph from your resource pack for a true smooth fade, e.g. `"<font:mypack:fade></font>"`. |

`TITLE` without a `title-glyph` falls back to `BLINDNESS` with a warning in the
log.

## Camera height

`cutscene.playback.eye-height` (default 1.62) is added to the recorded feet
position. 1.62 matches a standing player's eyes, so a recording looks the same as
what you saw while recording.

## If the server stops mid-cutscene

The viewer's starting location and flight state are stored on the player. If the
server crashes or stops during a cutscene, the player is put back there when they
join again.
