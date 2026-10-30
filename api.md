# Developer API

nonscenes exposes a small API for quest, NPC and other plugins: play a cutscene
for a player and control it.

## Setup

The API isn't published to a Maven repository. Add the plugin jar as a
compile-only dependency (the license allows compiling against it, but not
shipping it inside your plugin):

```kotlin
// build.gradle.kts
dependencies {
    compileOnly(files("libs/nonscenes-1.0.0.jar"))
}
```

and depend on the plugin in `plugin.yml`:

```yaml
depend: [nonscenes]       # or softdepend if it's optional
```

## Getting the API

Through Bukkit's services manager (recommended):

```java
NonscenesAPI api = Bukkit.getServicesManager().load(NonscenesAPI.class);
```

or directly:

```java
NonscenesAPI api = Nonscenes.getAPI();
```

The service is registered in `onEnable` and removed in `onDisable`.

## Methods

Package `com.nonxedy.api`. Call them from the main server thread.

| Method | Returns |
|---|---|
| `boolean play(Player player, String name)` | `true` if playback started. `false` if the cutscene doesn't exist, has fewer than two frames, uses a world that isn't loaded, or the player is already recording, watching or previewing. The player gets the matching chat message. |
| `boolean stop(Player player)` | `true` if the player was watching. Remaining command events don't run. |
| `boolean isPlaying(Player player)` | Whether the player is watching a cutscene, including the fades. |
| `boolean pause(Player player)` | `true` if paused. Only works while the camera moves, not during fades. |
| `boolean resume(Player player)` | `true` if resumed. |
| `boolean isPaused(Player player)` | Whether the player's cutscene is paused. |
| `boolean skip(Player player)` | `true` if skipped. Pending `console` and `player` events run immediately. |

Cutscene names are case-insensitive.

## Example

```java
public final class QuestHook {
    private final NonscenesAPI cutscenes =
        Bukkit.getServicesManager().load(NonscenesAPI.class);

    public void onQuestComplete(Player player) {
        if (cutscenes == null || cutscenes.isPlaying(player)) return;
        cutscenes.play(player, "quest_reward");
    }
}
```

There are no events for when a cutscene starts or ends yet. To run something at
the end, add a `console` [timeline event](timeline-events.md) at the cutscene's
last second; it also runs when the cutscene is skipped.
