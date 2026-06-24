# Smart Vacuum — Vacuum & Mop

`automation/vacuum_smart_clean.yaml`

## What it does

When I get home, it shouldn't be dirty. The robot runs **while you're away** and
cleans only the rooms that are overdue. If the weekly mop is due, the next run
switches to `vacuum_and_mop`.

It works with the stop automation `vacuum_comfort_home`, which pauses the robot
when someone gets home: if a run is cut short, the unfinished rooms are simply
due again on the next absence — no daily counter needed, because each room's
"last vacuumed" timestamp is the source of truth.

## Inputs (grouped)

**Vacuum & control**

| Input | Type | Description |
|---|---|---|
| `vacuum_entity` | `vacuum` | Used to check whether the robot is docked. |
| `mode_select` | `select` | Switches between `vacuum` and `vacuum_and_mop`. |
| `mqtt_segment_topic` | `text` | Valetudo segment-cleaning topic (`valetudo/<Robot>/MapSegmentationCapability/clean/set`). |

**Presence**

| Input | Type | Description |
|---|---|---|
| `presence_count` | `number` | How many residents are home. `0` for the wait time → a run starts. |

**Rooms & tracking**

| Input | Type | Description |
|---|---|---|
| `rooms` | `object` | List of `{ sensor: <last-vacuumed sensor>, segment_id: <Valetudo segment> }`. |
| `last_mopped_sensor` | `sensor` (`device_class: timestamp`) | Last mop run. |

**Schedule & thresholds**

| Input | Default | Description |
|---|---|---|
| `min_absence_minutes` | 30 | Minimum absence before a run starts. |
| `max_days_per_room` | 3 | Rooms older than this are due. |
| `max_days_between_mops` | 3 | Exceeded → next run mops too. |
| `earliest_start` | 09:00 | No run before this. |
| `latest_start` | 20:00 | No run after this. |

## Behavior

- **Trigger:** `presence_count` stays below 1 for `min_absence_minutes` (edge
  trigger), or HA restart (catches an absence already in progress).
- **Conditions:** presence < 1, absence long enough (`last_changed` check),
  inside the time window, vacuum `docked`, and at least one segment is due.
- **Action:** set the mode (mop or not) → wait 2 s → publish the segment-clean
  command via `mqtt.publish` with the due `segment_ids`.

### Due rooms / mop logic

- `due_segments` collects every room whose "last vacuumed" timestamp is older
  than `max_days_per_room` (or unknown/empty).
- `should_mop` is true when the last mop is older than `max_days_between_mops`
  (or unknown).

## Requirements

- **Valetudo** (or a compatible MQTT vacuum) with segment cleaning.
- Per room a "last vacuumed" timestamp sensor (must survive restarts) plus a
  "last mopped" sensor. Writing those timestamps happens outside this blueprint
  (e.g. a separate automation or template sensor).
- A `number` entity with the count of residents present.

## Example

```yaml
use_blueprint:
  path: eddyfussel/vacuum_smart_clean.yaml
  input:
    vacuum_entity: vacuum.valetudo_pointedilliterateferret
    mode_select: select.valetudo_pointedilliterateferret_operation_mode
    mqtt_segment_topic: valetudo/pointedilliterateferret/MapSegmentationCapability/clean/set
    presence_count: number.residents_present
    rooms:
      - sensor: sensor.living_room_last_vacuumed
        segment_id: 7
      - sensor: sensor.kitchen_last_vacuumed
        segment_id: 1
    last_mopped_sensor: sensor.last_mopped
```

## Changelog

### v1.0.0
- First release.
