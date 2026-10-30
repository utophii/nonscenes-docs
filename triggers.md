# Triggers

Triggers play cutscenes automatically. They are set in the `triggers` section of
`config.yml` and reloaded by `/ns reload`.

A trigger does nothing if the cutscene doesn't exist or the player is already
recording, watching or previewing something.

## First join

```yaml
triggers:
  first-join:
    enabled: true
    cutscene: intro
    delay-ticks: 40   # wait 2 s so the client has loaded the world
```

Plays once for players who have never joined the server before (Bukkit's
`hasPlayedBefore`).

## Custom commands

```yaml
triggers:
  commands:
    intro: spawn_tour     # /intro plays "spawn_tour"
    tour: spawn_tour
```

Each key becomes a command that plays the cutscene for whoever runs it. Command
names may contain lowercase letters, digits, `_` and `-`. They need no
permission; if you want to restrict them, use your permission plugin's command
blocking or play the cutscene through `/ns play`.

## Regions

```yaml
triggers:
  regions:
    spawn:                # region id
      world: world
      min-x: 0
      min-y: 64
      min-z: 0
      max-x: 20
      max-y: 90
      max-z: 20
      cutscene: spawn_intro
      once: true          # default true
```

Fires when a player walks into the box from outside it. Standing inside doesn't
retrigger it; leaving and walking back in does, unless `once` is true. Corner
order doesn't matter.

With `once: true`, the plugin remembers who has seen it in
`plugins/nonscenes/seen.yml`. Remove a player's entry there to let them see it
again.

## Renaming cutscenes

Triggers refer to cutscenes by name and are not updated when a cutscene is
renamed. `/ns rename` lists the triggers that still point to the old name so you
can fix them.
