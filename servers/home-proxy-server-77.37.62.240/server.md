# home-proxy-server-77.37.62.240

## Connection

- **SSH alias:** `ssh home-proxy-server`
  *(Port 2222, User sysadmin — configure in `~/.ssh/config`, see `SETUP.md`.)*
- **Host:** 77.37.62.240
- **User:** sysadmin
- **Port:** 2222
- **Hostname:** intomyappserver

## Environment

- **Type:** production / dev (mixed — runs both prod and dev workloads)
- **Provider:** unknown (public VPS)
- **OS:** Ubuntu 22.04 (5.15.0-173)
- **Region / timezone:** unknown
- **WireGuard network:** 10.0.0.x — this server is a WireGuard peer, tunnels to 10.0.0.1

## What runs here

- **Nginx** — reverse proxy for all Docker apps + domain routing
- **Docker** — 9 containers (apps, APIs, databases)
- **MySQL** — system MySQL instance
- **MS SQL Server** — `mssql-server.service`
- **Reverse SSH tunnel** — `autossh` back to 10.0.0.1 (ports 2223, 9092)
- **CAG Monitor** — Python backup monitoring dashboard
- **Media server** — static files at `/var/www/appmedia` (HTTP, `media.appofcag.space`)

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

## Domains & routing

| Domain / Path | Target | Notes |
|---|---|---|
| `appofcag.space` | port 8083 (cagappnative) | HTTP only |
| `media.appofcag.space` | `/var/www/appmedia` | HTTP only, static files |
| `<IP>/cagattendanceapi/` | port 8091 | IP-based, path-routed |
| `<IP>/cag-ayyapa/` | port 4200 | IP-based |
| `<IP>/cagayyappa/` | port 5173 | IP-based |
| `<IP>/cagyathraapi/` `/yathra/` | port 8095 | IP-based |
| `<IP>/manageserverapi/` | port 8086 | IP-based |
| `<IP>/servermanageui/` | port 5174 | IP-based |

## Key paths (on the VPS)

- Nginx sites: `/etc/nginx/sites-enabled/`
- Nginx app locations: `/etc/nginx/app-locations/`
- Media files: `/var/www/appmedia`
- App server files: `/home/appserver/` (devserver, logs, nginx-backups, prodserver, scripts)
- Storage/backup: `/home/storageserver/` (backups, data, docs, logs, monitoring, scripts)
- Monitor script: `/home/storageserver/monitoring/cag-monitor.py`
- Monitor logs: `/home/storageserver/logs/monitor.log`
- Prod deploy: `/home/sysadmin/prodserver/` (deploy, source)
- Dev deploy: `/home/sysadmin/devserver/` (deploy, source)
- Backups: `/home/backups/`

## Services (systemd)

- `nginx.service` — reverse proxy
- `docker.service` — container runtime
- `mysql.service` — system MySQL
- `mssql-server.service` — Microsoft SQL Server
- `reverse-tunnel.service` — autossh tunnel to 10.0.0.1 (ports 2223 SSH, 9092)
- `cag-monitor.service` — Python backup monitor dashboard

## Jump host — reaching the home network

This server is a WireGuard peer and SSH jump host. To reach internal machines:

```bash
# Home server via WireGuard (see homewg alias in ~/.ssh/config)
ssh homewg   # → 10.0.0.3 via ProxyJump vps-prod
```

The `reverse-tunnel.service` exposes this server's SSH (port 22) back to 10.0.0.1 on port 2223.

## Common operations

- Check all containers: `docker ps`
- Restart a container: `docker restart <name>`
- Container logs: `docker logs -f <name>`
- Check unhealthy containers: `docker ps --filter health=unhealthy`
- Check monitor log: `tail -f /home/storageserver/logs/monitor.log`
- Restart tunnel: `sudo systemctl restart reverse-tunnel`
- Nginx reload: `sudo nginx -s reload`

## Warnings / notes

- **Mixed prod/dev** — both production apps and dev containers run here. Be careful which you restart.
- **Reverse tunnel** — `reverse-tunnel.service` is critical for reaching home network. Don't stop without a plan to reconnect.
- `cagattendanceapi` and `manageserverapi` Docker containers are currently **unhealthy** — investigate.
- `media.appofcag.space` is HTTP-only here (no SSL), unlike `media.appofcag.site` on prod-gurukulam which has Certbot SSL.

## Related local repo(s)

- unknown — to be confirmed
