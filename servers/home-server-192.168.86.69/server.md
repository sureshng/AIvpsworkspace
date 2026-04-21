# home-server-192.168.86.69 (Home Server)

## Connection

- **SSH alias:** `ssh home-proxy-server`
  *(Reaches this machine via reverse tunnel on the public VPS — Port 2222 on 77.37.62.240)*
- **Actual LAN IP:** 192.168.86.69
- **WireGuard IP:** 10.0.0.3
- **User:** sysadmin
- **Hostname:** intomyappserver

## Architecture — IMPORTANT

This is a **home server behind NAT**, not a public VPS.
It is NOT directly reachable from the internet.

```
Internet → VPS (77.37.62.240, 10.0.0.1) → WireGuard → This server (10.0.0.3)
```

- All public traffic goes through the VPS nginx first, which proxies to `10.0.0.3` via WireGuard
- SSH access: port 2222 on VPS → reverse tunnel → this server's port 22
- The VPS (alias `vps-prod`) is the public-facing proxy — see `servers/vps-prod-77.37.62.240/`

## Environment

- **Type:** production / dev (mixed — runs both prod and dev workloads)
- **Provider:** home server (behind home router NAT)
- **OS:** Ubuntu 22.04 (5.15.0-173)
- **Region / timezone:** EDT (US East)
- **WireGuard IP:** 10.0.0.3 (peer: VPS at 10.0.0.1)

## What runs here

- **Nginx** — local reverse proxy routing Docker app paths
- **Docker** — 9 containers (apps, APIs, databases)
- **MySQL** — system MySQL instance
- **MS SQL Server** — `mssql-server.service`
- **Reverse SSH tunnel** — `autossh` to VPS 10.0.0.1 (exposes ports 2223 SSH and 9092 monitor)
- **CAG Monitor** — Python web dashboard monitoring backups, exposed on port 9092 → `http://77.37.62.240:9092`
- **Media server** — static files at `/home/mediaserver/appmedia/` served via `media.cagutility.click`

## Docker containers

| Container | Image | Port | Status |
|---|---|---|---|
| `cagattendanceapi` | cagattendanceapi | 8091 | **unhealthy** |
| `cag-ayyapa` | cag-ayyapa | 4200→80 | running |
| `servermanager-db` | postgres:16-alpine | 5433→5432 | healthy |
| `devmysql` | mysql:8.0 | 3308→3306 | running |
| `yathraapi` | yathraapi | 8095 | healthy |
| `cagayyappa` | cagayyappa | 5173→80 | running |
| `cagappnative` | cagappnative | 8083→80 | running |
| `manageserverapi` | manageserverapi | 8086 | **unhealthy** |
| `servermanageui` | servermanageui | 5174→80 | running |

## Domains & routing (all traffic enters via VPS proxy)

| Public Domain | VPS proxies to | This server handles |
|---|---|---|
| `appofcag.space` | `http://10.0.0.3/` | nginx → port 8083 (cagappnative) |
| `media.cagutility.click` | `http://10.0.0.3/` | nginx → `/home/mediaserver/appmedia/` |
| `<VPS-IP>/cagattendanceapi/` | `http://10.0.0.3/cagattendanceapi/` | nginx → port 8091 |
| `<VPS-IP>/cag-ayyapa/` | `http://10.0.0.3/cag-ayyapa/` | nginx → port 4200 |
| `<VPS-IP>/cagayyappa/` | `http://10.0.0.3/cagayyappa/` | nginx → port 5173 |
| `<VPS-IP>/cagyathraapi/` `/yathra/` | `http://10.0.0.3/cagyathraapi/` | nginx → port 8095 |
| `<VPS-IP>/manageserverapi/` | `http://10.0.0.3/manageserverapi/` | nginx → port 8086 |
| `<VPS-IP>/servermanageui/` | `http://10.0.0.3/servermanageui/` | nginx → port 5174 |

## Key paths (on this server)

- Nginx sites: `/etc/nginx/sites-enabled/`
- Nginx app locations: `/etc/nginx/app-locations/`
- Media files: `/home/mediaserver/appmedia/` (served as `media.cagutility.click`)
- App server files: `/home/appserver/` (devserver, logs, nginx-backups, prodserver, scripts)
- Storage/backup: `/home/storageserver/` (backups, data, docs, logs, monitoring, scripts)
- Monitor script: `/home/storageserver/monitoring/cag-monitor.py`
- Monitor logs: `/home/storageserver/logs/monitor.log`
- Prod deploy: `/home/sysadmin/prodserver/` (deploy, source)
- Dev deploy: `/home/sysadmin/devserver/` (deploy, source)
- Backups: `/home/backups/`

## Services (systemd)

- `nginx.service` — local reverse proxy
- `docker.service` — container runtime
- `mysql.service` — system MySQL
- `mssql-server.service` — Microsoft SQL Server
- `reverse-tunnel.service` — autossh to 10.0.0.1 (port 2223 → SSH, port 9092 → CAG Monitor)
- `cag-monitor.service` — Python web dashboard (port 9092), monitors `/home/storageserver/backups/`

## Common operations

- Check all containers: `docker ps`
- Restart a container: `docker restart <name>`
- Container logs: `docker logs -f <name>`
- Check unhealthy containers: `docker ps --filter health=unhealthy`
- Check monitor log: `tail -f /home/storageserver/logs/monitor.log`
- Restart tunnel: `sudo systemctl restart reverse-tunnel`
- Nginx reload: `sudo -n nginx -s reload`
- Nginx stop/start (if reload not enough): `sudo -n nginx -s stop && sudo -n nginx`

## Warnings / notes

- **Home server behind NAT** — cannot receive direct internet connections. All traffic via VPS proxy.
- **Nginx changes require stop+start** — `nginx -s reload` has known issues on this server; use `nginx -s stop && nginx` instead.
- **Mixed prod/dev** — both production apps and dev containers run here. Be careful which you restart.
- **Reverse tunnel is critical** — stopping `reverse-tunnel.service` cuts SSH access via the VPS. Don't stop without a direct LAN access plan.
- `cagattendanceapi` and `manageserverapi` Docker containers are currently **unhealthy** — investigate.
- `sudo` requires password for most commands — only `nginx` and `docker`-related commands are passwordless.

## Related local repo(s)

- unknown — to be confirmed
