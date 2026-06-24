# Restart thermostat valve adaptation

`automation/thermostat_valve_adapt.yaml`

## The bug it fixes

After a **firmware update or battery change**, the thermostat resets its
valve-adaptation state to `none` or `ready_to_calibrate` and **opens the valve
fully** — so it heats unintentionally. That's especially bad during remote
updates when nobody's home.

This catches that state and restarts adaptation immediately, so the valve
recalibrates and the unwanted heating stops.

## Inputs

| Input | Type | Description |
|---|---|---|
| `valve_adapt_status_sensor` | `sensor` | Reports valve-adaptation status (e.g. `sensor.ba_climate_01_valve_adapt_status`). |
| `valve_adapt_button` | `button` | Starts adaptation (e.g. `button.ba_climate_01_valve_adapt_process`). |
| `notify_target` | `text` (required) | Notify service for the critical push, e.g. `notify.mobile_app_<your_phone>`. |
| `language` | `select` (`de` / `en`, default `de`) | Language of the push and voice announcement. Code stays English. |

Room and area are derived from the status sensor via `area_name()` /
`area_id()`.

## Behavior

1. **Trigger:** status sensor changes to `none` or `ready_to_calibrate`.
2. **Fix:** press the adaptation button → the valve recalibrates.
3. **Critical push** via the configured `notify_target`
   (`interruption-level: critical`; a per-sensor `tag` so alerts overwrite
   instead of stacking).
4. **Voice announcement** in the affected room via `script.announce_in_area` —
   only if the sensor has an area.

## Requirements

- A generic `script.announce_in_area` (area + message → the right speaker).
- A notify service, set per automation via `notify_target`.

## Example

```yaml
use_blueprint:
  path: eddyfussel/thermostat_valve_adapt.yaml
  input:
    valve_adapt_status_sensor: sensor.ba_climate_01_valve_adapt_status
    valve_adapt_button: button.ba_climate_01_valve_adapt_process
    notify_target: notify.mobile_app_your_phone
    language: de
```

## Changelog

### v1.1.0
- Added `language` input (`de` / `en`, default `de`) for the push and announcement.
- Added `notify_target` input (required, no default) — set the notify service per automation.

### v1.0.0
- First release.
