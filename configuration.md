# Configuration

`plugins/nonscenes/config.yml`. Apply changes with `/ns reload`; only the
`storage` section needs a server restart. If the file isn't valid YAML, reload
keeps the current settings and says so.

When the plugin updates, new keys are added to your `config.yml` automatically
and obsolete ones are removed. Your values are kept.

## settings

```yaml
settings:
  countdown-seconds: 3          # countdown before /ns start begins recording
  path-visualization:           # /ns showpath
    particle: END_ROD           # Bukkit particle name
    duration: 30                # seconds
    spacing: 0.5                # blocks between particles
```

## cutscene.playback

```yaml
cutscene:
  playback:
    update-rate: 60             # camera updates per second, 20–240
    eye-height: 1.62            # camera height above the recorded feet position
    fade:
      mode: BLINDNESS           # NONE, BLINDNESS or TITLE
      out-ticks: 10
      hold-ticks: 10            # at least 1
      in-ticks: 20
      title-glyph: ""           # MiniMessage, used by TITLE
    interpolation: CATMULL_ROM  # CATMULL_ROM or BEZIER
    skip-on-sneak: true
```

- **update-rate**: higher is smoother on high refresh rate monitors and costs a
  few more packets per viewer. 60 is a good default.
- **fade**: see [Playback → Fades](playback.md#fades).
- **interpolation**: see [Keyframe editor → How the camera moves](keyframe-editor.md#how-the-camera-moves).

## cutscene.creation and cutscene.recording

```yaml
cutscene:
  creation:
    progress-bar:
      enabled: true
      style: SOLID              # SOLID, SEGMENTED_6, SEGMENTED_10, SEGMENTED_12, SEGMENTED_20
      color: BLUE               # BLUE, GREEN, PINK, PURPLE, RED, WHITE, YELLOW
  recording:
    capture-interval-ticks: 1   # 1 = 20 frames per second, 2 = 10, …
```

## cutscene.editor

```yaml
cutscene:
  editor:
    simplify:
      position-tolerance: 0.25  # blocks
      angle-tolerance: 3.0      # degrees
    default-segment-seconds: 2.0
```

See [Keyframe editor → Simplification](keyframe-editor.md#simplification).

## triggers

First join, custom commands and regions. See [Triggers](triggers.md).

## storage

```yaml
storage:
  type: SQLITE                  # SQLITE, MYSQL, POSTGRESQL or MONGODB
```

See [Storage](storage.md) for every backend's settings.
