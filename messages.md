# Messages

Every chat message is in `plugins/nonscenes/messages.yml`. Reload with
`/ns reload`. New messages added by plugin updates are appended to your file
automatically.

## Formatting

- `&a`, `&l`, … legacy color and format codes
- `&#3498db` hex colors
- `${prefix}` is replaced with the `prefix` value at the top of the file

```yaml
prefix: "&8[&#3498db&lnonscenes&8] &r"
cutscene-playing: "${prefix}&#2ecc71Playing cutscene &#f39c12{name}&#2ecc71..."
```

Titles in [timeline events](timeline-events.md) also accept MiniMessage; chat
messages don't.

## Placeholders

Placeholders in braces such as `{name}`, `{player}`, `{seconds}` or `{frames}`
are filled in by the plugin. Each message supports the placeholders that appear
in its default text; others are left as they are.

## Lists

`help-messages` (shown by `/ns` with no arguments) and `editor-help` (shown when
the editor opens) are lists: one line per entry.
