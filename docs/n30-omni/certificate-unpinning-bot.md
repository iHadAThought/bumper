# DEEBOT N30 OMNI — Replace Bumper certs on the robot (UART)

Newer Ecovacs models (including **N30 OMNI**, mid `zwkcqc`) pin Ecovacs CA material. After DNS points the robot at Bumper, TLS often **completes the leaf then resets** until Bumper’s CA/leaf are installed on the robot’s `/data` trust store.

Upstream guide (tested on **T10 Plus**; N30 paths may differ):  
https://mvladislav.github.io/bumper/internals/certificate-unpinning-bot/

N30 is **not** on Bumper’s tested-model list. Prefer **Option 1** (replace certs under `/data`). Stop if boot/UART layout does not match.

## What to copy (host → robot)

From your Bumper host cert directory (default `certs/` next to the Bumper data volume, or wherever `BUMPER_CERTS` points):

| Host file | On robot |
|-----------|----------|
| `ca.crt` | overwrite `/data/ca.crt` |
| `ca.crt` | overwrite `/data/ca-certificates/ca.pem` |
| `bumper.crt` then `ca.crt` | **append** to `/data/ca-certificates/ca-certificates.crt` |

Do **not** copy `ca.key` / `bumper.key` onto the robot — only trust-store PEMs.

Keep PEMs out of git. Stage copies on a secure host path (example: `/opt/bumper/certs-export/{ca.crt,bumper.crt}`) and transfer over SSH when you are ready for UART work.

## Hardware

- USB–TTL UART adapter (**3.3 V only** — never 5 V)
- Access to robot UART pads (model-specific; T10 photos in the upstream guide are reference only)
- Host with `picocom` / `tio` / `minicom` @ **115200 8N1**, no flow control

## Option 1 checklist (on the bot)

1. Attach UART; open console @ 115200; power-cycle the robot; interrupt autoboot (often any key / `CTRL+C` into Hobot / U-Boot).
2. Boot to shell: `setenv bootargs ${bootargs} init=/bin/bash` then `boot`.
3. Mount `/data` (prefer `/etc/rc.d/mount_data.sh start`; else GPT `data` partition → `mount -t ext4 -o rw /dev/rkflash0pN /data` — **confirm `N` from this bot’s GPT log**).
4. On the bot:
   - Overwrite `/data/ca.crt` ← host `ca.crt`
   - Overwrite `/data/ca-certificates/ca.pem` ← host `ca.crt`
   - **Append** (do not wipe) to `/data/ca-certificates/ca-certificates.crt`: first `bumper.crt`, then `ca.crt`
5. `sync`; reboot; leave the robot on the SSID that resolves Ecovacs names to your Bumper VIP.
6. Within ~60s, confirm on the Bumper host that the robot’s client IP shows successful HTTPS/MQTT (no endless `SSL handshake failed` / `ConnectionResetError`), and that Bumper’s bot list shows the vacuum as connected. Home Assistant (`DeebotUniversity` / Bumper integration) can then create vacuum entities.

## Rollback / caveats

- Factory reset or firmware OTA can wipe `/data` certs — repeat Option 1.
- Prefer completing official cloud pairing **before** the first cert replace.
- Option 2 (rootfs reflash) in upstream docs is **incomplete / brick risk** — do not use unless Option 1 fails and you accept that risk.
- Warranty / ToS: voided; proceed at your own risk.

## Related in this fork

- Overlay overview: [`docs/n30-omni/README.md`](./README.md)
- Example compose / nginx / DNS checklist: [`docs/n30-omni/configs/`](./configs/)
- Home Assistant room helpers + Teal’c dashboard: [`docs/n30-omni/homeassistant/`](./homeassistant/)
