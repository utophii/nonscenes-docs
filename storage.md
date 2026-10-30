# Storage, export and import

## Backends

Cutscenes are stored in a database set by `storage.type` in `config.yml`. Changes
need a server restart.

### SQLite (default)

```yaml
storage:
  type: SQLITE
  sqlite:
    file-path: "cutscenes.db"   # relative to plugins/nonscenes/
```

Nothing to set up. Good for a single server.

### MySQL / MariaDB

```yaml
storage:
  type: MYSQL
  mysql:
    host: localhost
    port: 3306
    database: minecraft
    username: root
    password: password
    pool:                        # HikariCP
      maximum-pool-size: 10
      minimum-idle: 2
      connection-timeout: 30000  # ms
      idle-timeout: 600000       # ms
      max-lifetime: 1800000      # ms
```

The database must exist, tables are created automatically.

### PostgreSQL

```yaml
storage:
  type: POSTGRESQL
  postgresql:
    host: localhost
    port: 5432
    database: minecraft
    username: postgres
    password: password
    schema: public
    pool: { … same as MySQL … }
```

### MongoDB

```yaml
storage:
  type: MONGODB
  mongodb:
    host: localhost
    port: 27017
    database: minecraft
    collection: cutscenes
    username: ""                # leave empty for no authentication
    password: ""
```

### Tables

The SQL backends create two tables, `cutscenes` and `cutscene_frames`.

Cutscenes are loaded into memory on start. If several servers share a database, a
cutscene made on one appears on the others after they restart. Don't edit the
same cutscene on two servers at once.

## When the database is unavailable

If the database can't be reached on start, the plugin logs the error and falls
back to YAML files in `plugins/nonscenes/cutscenes/`, one per cutscene.

When the database works again, these files are migrated into it on the next start
and renamed to `*.yml.migrated`. You can delete those once you've checked the
cutscenes.

## Export and import

To move a single cutscene to another server:

1. `/ns export intro` writes `plugins/nonscenes/exports/intro.yml`.
2. Copy that file into the other server's `plugins/nonscenes/exports/`.
3. `/ns import intro` there, or `/ns import intro intro_v2` to give it another name.

Imported cutscenes need the same world names. If a world is missing, the import
still works but warns you, and the cutscene can't be played until that world is
loaded.

### File format

```yaml
name: intro
frame-duration-ms: 50
schema-version: 1
frames:
  '0':
    world: world
    x: 10.5
    y: 70.0
    z: -3.2
    yaw: 90.0
    pitch: 10.0
    segment-ms: 2000     # only for keyframed cutscenes: time to the next frame
  '1':
    ...
events:
  '0':
    time-ms: 1000
    type: TITLE
    title: Welcome
    subtitle: ''
```

Positions are the feet position, the camera is placed `eye-height` above it. See
[Timeline events](timeline-events.md#title-timing-in-yaml) for event fields.

## Backups

- SQLite: copy `cutscenes.db` (with the server stopped, or together with
  `cutscenes.db-wal`).
- MySQL / PostgreSQL / MongoDB: your usual database backups.
- Any backend: `/ns export` each important cutscene.
