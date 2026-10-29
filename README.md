# De'Longhi Comfort for Home Assistant

[![GitHub release](https://img.shields.io/github/v/release/Boisti13/delonghi-comfort-integration)](https://github.com/Boisti13/delonghi-comfort-integration/releases)
[![License](https://img.shields.io/github/license/Boisti13/delonghi-comfort-integration)](LICENSE)
[![HACS](https://img.shields.io/badge/HACS-custom-orange.svg)](https://hacs.xyz/docs/faq/custom_repositories/)
[![Validate](https://github.com/Boisti13/delonghi-comfort-integration/actions/workflows/validate.yml/badge.svg)](https://github.com/Boisti13/delonghi-comfort-integration/actions/workflows/validate.yml)

A Home Assistant integration for De'Longhi Comfort devices — Pinguino portable
air conditioners, dehumidifiers and heaters that you normally control with the
De'Longhi Comfort app. It logs in to De'Longhi's cloud with your app account
and polls your devices, so they show up in Home Assistant with a thermostat,
sensors and switches.

> **Maintained fork.** This is a fork of
> [philm5/delonghi-comfort-integration](https://github.com/philm5/delonghi-comfort-integration),
> which has had no activity since November 2025. It keeps the same domain
> (`delonghi_comfort`), so switching an existing install over keeps your
> devices, entities and settings. Fixes so far are in the
> [CHANGELOG](CHANGELOG.md).

## Highlights

- **Thermostat entity** with cool, heat, dry, fan-only and off, target
  temperature and fan speed.
- **Sensors** for room temperature and humidity, status, mode, fan speed,
  filter and operating hours, power use, Wi-Fi signal and error code.
- **Switches and controls** for power, silent mode, temperature and humidity
  setpoints and device mode.
- **Heaters** (De'Longhi `DL-heater` models) get their own climate entity,
  display brightness, eco and child lock.
- **Set up in the UI** — no YAML; polling interval adjustable under
  *Configure*.
- **Works with current Home Assistant**: the options dialog and device
  registration are fixed for HA 2025.12 and later.

## Install

| Step | What to do |
|---|---|
| 1 | In HACS: ⋮ → **Custom repositories** → add `https://github.com/Boisti13/delonghi-comfort-integration`, category **Integration**. |
| 2 | Find **De'Longhi Comfort** in HACS and download it. |
| 3 | Restart Home Assistant. |
| 4 | **Settings → Devices & services → Add integration → De'Longhi Comfort**, then log in with your De'Longhi Comfort app account. |

Manual install: copy `custom_components/delonghi_comfort/` into your Home
Assistant `config/custom_components/` folder and restart.

**Coming from philm5's version?** Remove that repository in HACS (your
integration entry stays), add this one as above, download it and restart.
The domain is the same, so nothing else changes.

Requires Home Assistant 2024.11 or newer.

## Entities

Example for an air conditioner named *Pinguino* (entity IDs follow the
device name you gave it in the app):

| Entity | What it is |
|---|---|
| `climate.pinguino` | Thermostat: mode, target temperature, fan speed |
| `switch.pinguino_device_power`, `switch.pinguino_silent_mode` | Power, silent mode |
| `number.pinguino_temperature_setpoint`, `number.pinguino_humidity_setpoint` | Setpoints |
| `select.pinguino_device_mode` | Operating mode |
| `sensor.pinguino_room_temperature`, `sensor.pinguino_room_humidity` | Room readings |
| `sensor.pinguino_device_status`, `_device_mode`, `_fan_speed`, `_silent_mode` | Current state |
| `sensor.pinguino_filter_hours`, `_operating_hours`, `_power_consumption` | Usage |
| `sensor.pinguino_wifi_signal_strength`, `sensor.pinguino_error_code` | Diagnostics |

## Example

Cool the room in the afternoon when it gets warm and you're home:

```yaml
automation:
  - alias: "Pinguino – Cool on hot afternoons"
    triggers:
      - trigger: numeric_state
        entity_id: sensor.pinguino_room_temperature
        above: 26
    conditions:
      - condition: time
        after: "13:00:00"
        before: "20:00:00"
      - condition: state
        entity_id: person.you
        state: home
    actions:
      - action: climate.set_hvac_mode
        target:
          entity_id: climate.pinguino
        data:
          hvac_mode: cool
      - action: climate.set_temperature
        target:
          entity_id: climate.pinguino
        data:
          temperature: 24
```

## Configuration

*Configure* on the integration sets the **polling interval** (default 15 s).
The data comes from De'Longhi's cloud, so a longer interval (for example
60 s) is kinder to it and is fine for a room air conditioner. The integration
needs internet access; while the cloud is unreachable the entities show as
unavailable.

Debug logging:

```yaml
logger:
  logs:
    custom_components.delonghi_comfort: debug
```

## Layout

```
custom_components/delonghi_comfort/
  __init__.py       # setup: log in, find devices, one coordinator per device
  api.py            # De'Longhi cloud API (login, token, device properties)
  coordinator.py    # polling, shared device info
  climate.py        # air-conditioner and heater thermostats
  sensor.py, switch.py, number.py, select.py
  config_flow.py    # login dialog and options (polling interval)
  translations/en.json, icons.json, manifest.json
.github/workflows/validate.yml   # syntax, manifest and HACS checks on every push
```

## Development

- Branches: `main` (releases) and `dev` (work). Work on `dev`, merge to
  `main` for a release.
- Release: bump `version` in `manifest.json`, move *Unreleased* in the
  CHANGELOG to the new version, tag `vX.Y.Z` on `main` and create the GitHub
  release with that CHANGELOG section as notes (`gh release create`). HACS
  offers the new version from the release.
- Test on a real Home Assistant before releasing: the integration talks to
  De'Longhi's cloud, so there are no offline tests.
- Changes from the original repository can be pulled in with
  `git fetch upstream` (remote `upstream` = philm5's repository).

## Credits and license

Apache License 2.0 — see [LICENSE](LICENSE) and [NOTICE](NOTICE).

- **[philm5](https://github.com/philm5/delonghi-comfort-integration)** wrote
  this integration; this fork continues it.
- **[rtfpessoa/delonghi-comfort-client](https://github.com/rtfpessoa/delonghi-comfort-client)**
  for the original client, and **[duckwc/ECAMpy](https://github.com/duckwc/ECAMpy)**
  for the authentication and token code.

Provided as-is, without warranty. Not affiliated with De'Longhi.
