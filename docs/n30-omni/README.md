# N30 OMNI / Teal’c overlay (this fork)

This fork of [MVladislav/bumper](https://github.com/MVladislav/bumper) adds **deployment notes and Home Assistant helpers** used to run Bumper offline with an Ecovacs **DEEBOT N30 OMNI** (example entity name: Teal’c).

## What changed vs upstream

| Area | Notes |
|------|--------|
| Robot cert unpin | N30 needs Bumper CA/leaf on `/data` via UART — see [certificate-unpinning-bot.md](./certificate-unpinning-bot.md) |
| Deploy examples | nginx SNI stream + compose under [`configs/`](./configs/) (placeholder IPs; no PEMs) |
| Home Assistant | Room-label helpers, clean-by-name dropdown, YAML sidebar dashboard under [`homeassistant/`](./homeassistant/) |

## Privacy

This overlay is intentionally **sanitized**:

- No PEM/key material
- No serial numbers, DIDs, or account tokens
- No live LAN IPs, hostnames, or personal Book/Git URLs — replace placeholders with yours

## Quick path

1. Deploy Bumper (+ nginx SNI on `:443`) using upstream docs and/or [`configs/`](./configs/).
2. Point IoT DNS rewrites for Ecovacs names at your Bumper VIP.
3. Replace certs on the robot ([certificate-unpinning-bot.md](./certificate-unpinning-bot.md)).
4. Install HA helpers + dashboard ([homeassistant/README.md](./homeassistant/README.md)).
