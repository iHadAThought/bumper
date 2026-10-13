# Deploy examples (sanitized)

- `docker-compose.bumper.yml` — Bumper + nginx; set `BUMPER_ANNOUNCE_IP` to your VIP
- `nginx.conf` — SNI stream router for `:443`
- `bumper-dns-rewrites.txt` — checklist of Ecovacs names to rewrite

No certificates or private keys are included. Generate or copy PEMs on the host only.
