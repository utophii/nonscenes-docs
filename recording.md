# Recording cutscenes

```text
/ns start <name> <seconds>
```

Records your position and view direction for `<seconds>` (1-300). Needs
`nonscene.start`.

1. A countdown of `settings.countdown-seconds` (default 3) runs in chat.
2. Recording starts. A boss bar shows the progress (`cutscene.creation.progress-bar`).
3. When the time is up the cutscene is saved and `Cutscene <name> recorded successfully with N frames` is shown.

`/ns stop` cancels the countdown or the recording, nothing is saved.

## Names

Cutscene names may contain only letters, digits, `_` and `-`, up to 64
characters. Names are case-insensitive: `Intro` and `intro` are the same
cutscene, but the original spelling is kept for display.

## Frame rate

`cutscene.recording.capture-interval-ticks` sets how often a frame is captured:
`1` is 20 frames per second, `2` is 10, and so on. Playback is always smooth
regardless, because the camera follows a spline through the frames and is updated
at `cutscene.playback.update-rate` (default 60 Hz). A lower capture rate gives
smaller cutscenes and a calmer path, a higher one keeps every small movement.

## Tips

- Fly in spectator mode so blocks don't get in the way.
- Everything you do is replayed, including shaky mouse movement. If a part looks
  rough, open the cutscene in the [keyframe editor](keyframe-editor.md): it
  reduces the recording to keyframes and smooths it out.
- A cutscene may cross worlds, every frame stores its world.
- `/ns showpath <name>` draws the path with particles for 30 seconds so you can
  check it without watching it. The particles are visible to every player nearby.
  `/ns stop` hides them early. See `settings.path-visualization` in
  [Configuration](configuration.md).
