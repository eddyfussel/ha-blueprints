# Sync room temperature to thermostat

`automation/sync_room_temp.yaml`

## What it does

Feeds a dedicated room sensor's temperature into a thermostat's
external-temperature input (`number.*`), so the thermostat regulates on the real
room temp instead of the warmer reading at the valve — more accurate, more
efficient.

If the sensor drops out for a while, it raises a Repairs issue and clears it
automatically once a real value returns.

## Inputs

| Input | Type | Description |
|---|---|---|
| `temperature_sensor` | `sensor` (`device_class: temperature`) | Source of the room temperature. |
| `thermostat_remote_temperature` | `number` | Thermostat's external-temperature input. |
| `language` | `select` (`de` / `en`, default `de`) | Language of the Repairs message. Code stays English. |

The room name in the issue is derived from the sensor via `area_name()` — no
extra input needed.

## Behavior

- **Sync:** on every real value, it's written straight to the `number` input.
- **Debounced alert:** `unavailable` and `unknown` both count as down. The issue
  is only raised after **5 minutes** of continuous downtime, so short
  Zigbee2MQTT/MQTT blips (~1 min) don't trigger a false alarm.
- **Reliable clear:** the issue clears on the first real value — whether the
  recovery came from `unavailable` or via the `unknown` step.
- **Self-healing:** a 30-minute check re-raises the issue if it was dismissed by
  hand while the sensor is still down (≥ 5 min).

The `issue_id` is derived from the sensor's `entity_id`
(`sync_temp_unavailable_<entity_with_underscores>`), so it's unique per sensor.

## Example

```yaml
use_blueprint:
  path: eddyfussel/sync_room_temp.yaml
  input:
    temperature_sensor: sensor.wz_thrm_local_temperature
    thermostat_remote_temperature: number.wz_climate_01_remote_temperature
```

## Changelog

### v1.1.0
- Added `language` input (`de` / `en`, default `de`) for the Repairs message.

### v1.0.0
- 5-minute debounce before raising the issue (was: instantly on the first blip).
- Treat `unknown` as down, alongside `unavailable`.
- Clear the issue on the first real value (the old `from: "unavailable"` branch
  missed recoveries that passed through `unknown`).
- Periodic re-check only re-raises on **sustained** downtime (≥ 5 min, via
  `last_changed`).
