# <VPS NAME>

<!--
This file is Claude's memory of static facts about this server.
Update it whenever we learn something new that will matter next session.
Do NOT put secrets, passwords, or private keys here.
-->

## Connection

- **SSH alias:** `ssh <alias>`
  *(Configure this alias in `~/.ssh/config` on each machine you use —
   see `SETUP.md`.)*
- **Host:** <hostname or IP>
- **User:** <username>

## Environment

- **Type:** production | staging | dev
- **Provider:** 
- **OS:** 
- **Region / timezone:** 

## What runs here

- <service 1> — <brief description>
- <service 2> — <brief description>

## Key paths (on the VPS)

- App directory: 
- Logs: 
- Configs: 
- Backups: 

## Services (systemd)

- `<service-name>.service` — <purpose>

## Common operations

- Check status: `systemctl status <service>`
- Tail logs: `tail -f <log-path>`
- Restart: `sudo systemctl restart <service>`
- Test health: `curl -s http://localhost:<port>/<health-endpoint>`

## Warnings / notes

- 

## Related local repo(s)

<!-- Repo names only, not absolute paths. Paths vary per machine. -->

- `<repo-name>`
