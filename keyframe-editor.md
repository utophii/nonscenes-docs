# Keyframe editor

The editor builds a cutscene from keyframes: points with a position, a view
direction and a time. Use it to create a cutscene from scratch or to fine-tune a
recording. All editor commands are player-only and need `nonscene.edit`.

## Opening

```text
/ns edit <name> [tolerance]
```

- If `<name>` doesn't exist, you get an empty draft.
- If it does, it is loaded. A raw recording is first reduced to keyframes (see
  [Simplification](#simplification)).

The draft is private: nothing is stored until `/ns save`, and only one player can
edit a cutscene at a time. Leaving the server discards the draft.

## What you see

While editing, the path is drawn only for you:

- **Dots** along the camera path, spaced 0.1 s apart. Denser dots mean a slower
  camera.
- A **flame** and a **view ray** at every keyframe.
- A **numbered label** with the keyframe's time and the duration of the next
  segment.

## Keyframe commands

Keyframe numbers start at 1. Tab completion suggests valid numbers.

| Command | Description |
|---|---|
| `/ns key add [#]` | Add a keyframe at your position and view. Without `#` it is appended at the end. |
| `/ns key move <#>` | Move keyframe `#` to your position and view. |
| `/ns key remove <#>` | Remove keyframe `#`. |
| `/ns key speed <#> <seconds>` | How long the camera takes from keyframe `#` to `#+1`, 0.05–600 s. `2` and `2s` both work. |
| `/ns key tp <#>` | Teleport to keyframe `#`, looking the way the camera does there. |
| `/ns key list` | List keyframes with their times. |

A new segment copies the duration of its neighbour. Without a neighbour it uses
`cutscene.editor.default-segment-seconds` (default 2).

## Finishing

| Command | Description |
|---|---|
| `/ns preview` | Watch the draft. Console and player command events are skipped in previews. |
| `/ns save` | Store the draft. Needs at least two keyframes. |
| `/ns discard` | Throw the draft away. |

If the cutscene got shorter and some [timeline events](timeline-events.md) are
now after its end, saving warns you about them. They are kept, but never fire.

## How the camera moves

The camera follows a timed spline through the keyframes and never moves in
straight lines between them. It passes keyframe `#` at exactly its time and does
not stop there. Changing one segment's duration only changes how fast the camera
moves along that part of the path, not the path's shape.

The curve type is `cutscene.playback.interpolation`:

- `CATMULL_ROM`: round, flowing curves.
- `BEZIER`: tighter curves that never swing past a keyframe.

To make the camera hold still, place two keyframes at the same spot and give the
segment between them the hold time.

## Simplification

Opening a raw recording reduces it to keyframes. A frame is dropped when the path
through the keyframes around it, at the same moment, stays within these limits of
the recorded frame:

```yaml
cutscene:
  editor:
    simplify:
      position-tolerance: 0.25   # blocks
      angle-tolerance: 3.0       # degrees of yaw / pitch
```

Smaller values keep more keyframes. `/ns edit <name> <blocks>` overrides
`position-tolerance` for one session, e.g. `/ns edit tour 1` for a much simpler
path. Every keyframe keeps its original time, so the cutscene's length stays the
same.

Once saved from the editor, a cutscene is stored as keyframes and is not
simplified again the next time you open it.
