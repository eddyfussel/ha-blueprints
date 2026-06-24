<!-- 🇬🇧 English below · 🇩🇪 Deutsch weiter unten -->

# ha-blueprints

Home Assistant automation blueprints by **eddyfussel**.

Blueprints live in [`automation/`](automation/); detailed docs per blueprint in
[`docs/`](docs/).

## Blueprints

| Blueprint | What it does | Docs |
|---|---|---|
| [`sync_room_temp.yaml`](automation/sync_room_temp.yaml) | Mirror a room sensor onto a thermostat's external-temperature input; raise a Repairs issue if the sensor drops out for long. | [docs](docs/sync_room_temp.md) |
| [`thermostat_valve_adapt.yaml`](automation/thermostat_valve_adapt.yaml) | Bugfix: detect a reset valve-adaptation (valve stuck wide open) after a firmware update or battery change and recalibrate at once. | [docs](docs/thermostat_valve_adapt.md) |
| [`vacuum_smart_clean.yaml`](automation/vacuum_smart_clean.yaml) | Run the robot while you're away and clean only the overdue rooms (mop when due). | [docs](docs/vacuum_smart_clean.md) |

### Output language

Blueprints with user-facing text (`sync_room_temp`, `thermostat_valve_adapt`)
have an **Output language** input (`de` / `en`, default `de`). Only the
notification / announcement / Repairs text is translated — the code stays
English.

## Import into Home Assistant

Settings → Automations & Scenes → Blueprints → **Import blueprint**, then paste
the raw URL, e.g.:

```
https://raw.githubusercontent.com/eddyfussel/ha-blueprints/main/automation/sync_room_temp.yaml
```

Use a `…/main/…` URL to always pull the latest, or a tag URL (`…/v1.1.0/…`) to
pin a fixed version.

## Updates & versioning

- An imported blueprint is a **local copy**, not a live link. HA stores its
  `source_url` and only updates on demand: on the blueprint, ⋮ →
  **Re-import blueprint**.
- A blueprint update applies to **every** automation that uses it — you don't
  touch the automations.
- Stable versions are tracked with **git tags** (`v1.0.0`, …); changes are noted
  in each blueprint's `docs/` changelog.

## License

See [`LICENSE.md`](LICENSE.md).

---

# ha-blueprints (Deutsch)

Home-Assistant-Automatisierungs-Blueprints von **eddyfussel**.

Die Blueprints liegen unter [`automation/`](automation/), die ausführliche Doku
je Blueprint unter [`docs/`](docs/) (englisch).

## Blueprints

| Blueprint | Zweck | Doku |
|---|---|---|
| [`sync_room_temp.yaml`](automation/sync_room_temp.yaml) | Raumsensor auf den Remote-Temperatur-Eingang eines Thermostats spiegeln; meldet längeren Sensorausfall als Reparatur. | [docs](docs/sync_room_temp.md) |
| [`thermostat_valve_adapt.yaml`](automation/thermostat_valve_adapt.yaml) | Bugfix: erkennt zurückgesetzte Ventil-Adaption (Ventil voll offen) nach Firmware-Update/Batteriewechsel und kalibriert sofort neu. | [docs](docs/thermostat_valve_adapt.md) |
| [`vacuum_smart_clean.yaml`](automation/vacuum_smart_clean.yaml) | Startet den Saugroboter bei Abwesenheit und reinigt nur überfällige Räume (wischt bei Bedarf). | [docs](docs/vacuum_smart_clean.md) |

### Ausgabesprache

Blueprints mit Nutzer-Ausgabe (`sync_room_temp`, `thermostat_valve_adapt`)
haben einen Input **Output language** (`de` / `en`, Standard `de`). Übersetzt
wird nur der Benachrichtigungs-/Ansage-/Reparatur-Text — der Code bleibt
englisch.

## Import in Home Assistant

Einstellungen → Automatisierungen & Szenen → Blueprints → **Blueprint
importieren**, dann die Raw-URL einfügen, z. B.:

```
https://raw.githubusercontent.com/eddyfussel/ha-blueprints/main/automation/sync_room_temp.yaml
```

`…/main/…` zieht immer den aktuellen Stand, eine Tag-URL (`…/v1.1.0/…`) pinnt
einen festen Stand.

## Updates & Versionierung

- Ein importierter Blueprint ist eine **lokale Kopie**, keine Live-Verbindung.
  HA merkt sich die `source_url` und aktualisiert nur auf Klick: beim Blueprint
  ⋮ → **Blueprint neu importieren**.
- Ein Blueprint-Update wirkt auf **alle** Automationen, die ihn nutzen — die
  Automationen müssen nicht angefasst werden.
- Stabile Stände werden über **Git-Tags** (`v1.0.0`, …) festgehalten; Änderungen
  stehen im jeweiligen Changelog unter `docs/`.

## Lizenz

Siehe [`LICENSE.md`](LICENSE.md).
