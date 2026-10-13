# Home Assistant helpers for Teal'c rooms

The stock Ecovacs map entity (`image.tealc_map`) does **not** draw segment numbers when the bot is Bumper / HA-only (no Ecovacs app). HA still knows segments `0`–`4` on `vacuum.tealc` → `rooms.default`.

## Install

1. On HA OS, ensure packages are enabled in `configuration.yaml`:

   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```

2. Copy `tealc_rooms.yaml` to `/config/packages/tealc_rooms.yaml`.

3. Reload **Input texts**, **Scripts**, and **Template entities** (Developer Tools → YAML), or restart HA.

## Use

1. Put `image.tealc_map` on a dashboard.
2. Run **Teal'c identify segment** with `0`, watch which room the bot goes to.
3. Type that name into helper **Teal'c room 0**. Repeat for `1`–`4`.
4. Clean later with **Teal'c clean named room** (e.g. `kitchen`) or **Teal'c clean segment** by ID.

Sensor `sensor.tealc_room_labels` shows how many are labeled and a `by_name` attribute for automations.

## Dashboard (sidebar)

Lovelace YAML dashboards must be registered in **`configuration.yaml`** (not packages).

1. Copy `dashboards/tealc.yaml` → `/config/dashboards/tealc.yaml`.
2. Under `lovelace.dashboards` in `/config/configuration.yaml`, add:

   ```yaml
   tealc-control:
     mode: yaml
     title: Teal'c
     icon: mdi:robot-vacuum
     show_in_sidebar: true
     require_admin: false
     filename: dashboards/tealc.yaml
   ```

3. Restart HA.

Sidebar entry: **Teal'c** (`tealc-control`) — map, vacuum controls, room label helpers, Identify/Clean segment buttons 0–4, and clean-by-name.
