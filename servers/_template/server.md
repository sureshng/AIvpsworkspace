# <VPS NAME>

<!--
This file is Claude's memory of static facts about this server.
Update it whenever we learn something new that will matter next session.
Do NOT put secrets, passwords, or private keys here.
-->

## Connection

- **SSH alias:** `ssh <alias>`
  *(Configure this alias in `~/.ssh/config` on each machine — see `SETUP.md`.)*
- **Host:** <public IP or hostname>
- **Actual LAN IP:** <LAN IP if behind NAT, else same as above>
- **User:** <username>
- **Port:** <22 or custom>

## Architecture

<!--
Is this server directly internet-accessible, or behind NAT/proxy?
Document the full traffic path so future sessions don't waste time debugging routing.
-->

- **Type:** public VPS | home server behind NAT | internal server
- **Traffic path:** `Internet → <entry point> → <this server>`
- **WireGuard IP:** <10.0.0.x if on WireGuard network, else N/A>
- **Jump host / proxy:** <alias of the server that sits in front, if any>

## Environment

- **Type:** production | staging | dev | mixed
- **Provider:** <Hostinger / DigitalOcean / home / etc.>
- **OS:** <Ubuntu 22.04 / Debian / etc.>
- **Region / timezone:** <e.g. US East / EDT>

## What runs here

- <service / app> — <brief description>
- <service / app> — <brief description>

## Docker containers

<!--
Remove this section if Docker is not used.
Check with: docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}'
-->

| Container | Image | Port | Status |
|---|---|---|---|
| `<name>` | `<image>` | `<host:container>` | running / healthy / unhealthy |

## Domains & routing

<!--
Map every public domain/path to what serves it.
For proxy servers: note where traffic is forwarded to.
-->

| Domain / Path | Proxies to / Serves from | Notes |
|---|---|---|
| `<domain>` | `<port or upstream>` | HTTP / HTTPS |

## Key paths (on the server)

- App directory: 
- Logs: 
- Configs: `/etc/nginx/` or similar
- Backups: 
- Scripts: 

## Services (systemd)

<!--
Check with: systemctl list-units --type=service --state=running --no-pager
Also check: ls /etc/systemd/system/*.service
-->

- `<service>.service` — <purpose> — **running** | **stopped**

## Script-managed processes

<!--
Some processes are started via shell scripts, not systemd — they won't survive reboots.
Check with: ps aux | grep java && ls ~/scripts/ or /home/deploy/scripts/
-->

- `<process>` — port `<n>`, script at `<path>`, logs at `<path>` — **no systemd, won't auto-restart**

## Tunnel / reverse tunnel ports

<!--
Check with: ss -tlnp | grep -E '222|909'
-->

| Port on this server | Forwards to | Purpose |
|---|---|---|
| `<port>` | `<destination>` | SSH / monitor / API |

## Sudo access

<!--
Document which commands work without a password — important for automation.
Check with: sudo -n <command> 2>&1
-->

- Passwordless: `nginx`, `<other commands>`
- Requires password: most `systemctl` commands, file edits in `/etc/`
- **nginx reload note:** `nginx -s reload` may not pick up new server blocks — use `nginx -s stop && nginx` if needed

## Common operations

- Check all services: `systemctl list-units --type=service --state=running --no-pager`
- Check docker: `docker ps`
- Tail nginx error log: `tail -f /var/log/nginx/error.log`
- Test nginx config: `nginx -t`
- Reload nginx: `nginx -s reload` (or `nginx -s stop && nginx` if reload doesn't work)
- Check listening ports: `ss -tlnp`
- Check WireGuard: `wg show`

## Warnings / notes

- 
- **Nginx changes:** reload may not pick up new server blocks — stop/start if needed
- **Script-managed processes** won't survive reboots — check before maintenance

## Related local repo(s)

<!-- Repo names only, not absolute paths. Paths vary per machine. -->

- `<repo-name>`
