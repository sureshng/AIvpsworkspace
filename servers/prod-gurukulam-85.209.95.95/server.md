# prod-gurukulam-85.209.95.95

## Connection

- **SSH alias:** `ssh prod-gurukulam`
  *(Configure this alias in `~/.ssh/config` on each machine — see `SETUP.md`.)*
- **Host:** 85.209.95.95
- **User:** root

## Environment

- **Type:** production
- **Provider:** unknown
- **OS:** Ubuntu (Linux)
- **Region / timezone:** unknown

## What runs here

- **Nginx** — reverse proxy / web server (4 sites, 1 media server)
- **prodcaggurukulamapi** — Caggurukulam Java Spring Boot API (port 8095)
- **api1** — Java utility API (Spring Boot, port 8091, prod profile)
- **MySQL** — database server
- **Media server** — static file serving at `/var/www/appmedia`

## Domains & web apps

| Domain | Type | Root / Backend |
|---|---|---|
| `caggurukulam.com`, `www.caggurukulam.com` | Expo Web (React Native) | `/home/deploy/prod/frontend/caggurukulamapp` → API port 8095 |
| `cagusa.org`, `www.cagusa.org` | Angular / same Expo build | `/home/deploy/prod/frontend/caggurukulamapp` → API port 8095 |
| `cagapp.appofcag.site` | Angular web app | `/home/deploy/prod/frontend/cagorg/dist` |
| `api.appofcag.site` | API proxy | → port 8092 (service unknown — may be inactive) |
| `api.gurukulam.appofcag.site` | API proxy | → port 8094 (service unknown — may be inactive) |
| `caggurukulam.appofcag.site` | Angular attendance app | `/home/deploy/frontend/cagattendanceapp` |
| `appofcag.site`, `www.appofcag.site` | Angular app | `/home/root/frontend/app1/caggurukulam/browser` → API port 8091 |
| `media.appofcag.site` | Static media CDN | `/var/www/appmedia` |

## Key paths (on the VPS)

- Prod API app dir: `/home/deploy/prod/backend/caggurukulamapi/`
- Prod API jar: `/home/deploy/prod/backend/caggurukulamapi/caggurukulamapi.jar`
- Prod API logs: `/home/deploy/prod/backend/caggurukulamapi/logs/app.log`
- API1 jar: `/home/deploy/backend/api1/cag-utility-application-0.0.1-SNAPSHOT.jar`
- Frontend (Expo/prod): `/home/deploy/prod/frontend/caggurukulamapp/`
- Frontend (Angular/prod): `/home/deploy/prod/frontend/cagorg/dist/`
- Frontend (attendance): `/home/deploy/frontend/cagattendanceapp/`
- Frontend (app1): `/home/root/frontend/app1/caggurukulam/browser/`
- Media storage: `/var/www/appmedia`
- Web root: `/var/www/html`
- Nginx config: `/etc/nginx/sites-available/`
- Disabled nginx configs: `/root/disabled_nginx/`
- System app logs: `/var/log/cagform-api-prod.log`, `/var/log/cagform-api.log`
- Source code (explore): `/home/source/`

## Services (systemd)

- `prodcaggurukulamapi.service` — Caggurukulam Java API (prod, port 8095) — **running**
- `api1.service` — Java utility API (Spring Boot, prod profile, port 8091) — **running**
- `cag-api.service` — CAG Attendance API, jar at `/home/deploy/backend/cagattendanceapi/attendanceapi.jar` — **inactive/stopped**
- `nginx.service` — web server / reverse proxy — **running**
- `mysql.service` — MySQL database — **running**

## Script-managed processes (not systemd)

- `cagformapi` — Spring Boot API, port 8092, jar at `/home/deploy/backend/cagformapi/cagutilityforms.jar`
  - Start: `/home/deploy/scripts/start-cagform-api.sh <prod|dev>`
  - Restart: `/home/deploy/scripts/restart-cagform-api.sh <prod|dev>`
  - Logs: `/var/log/cagform-api-prod.log`, `/var/log/cagform-api-dev.log`
  - **Note:** Managed by shell scripts, NOT systemd — won't auto-restart on reboot

## Source code on server

- `/home/source/javapp/cagattendanceapi/` — source for attendance API
- `/home/source/nativeapp/cagnativeapp/` — source for Expo native app

## Common operations

- Check all service status: `systemctl status prodcaggurukulamapi api1 nginx mysql`
- Tail prod API log: `tail -f /home/deploy/prod/backend/caggurukulamapi/logs/app.log`
- Tail nginx access log: `tail -f /var/log/nginx/access.log`
- Tail nginx error log: `tail -f /var/log/nginx/error.log`
- Restart prod API: `sudo systemctl restart prodcaggurukulamapi`
- Restart api1: `sudo systemctl restart api1`
- Restart nginx: `sudo systemctl restart nginx`
- Test nginx config: `nginx -t`
- MySQL prompt: `mysql -u root -p`

## Warnings / notes

- **Production server** — double-confirm before any restart, delete, or migration.
- Ports 8092 and 8094 referenced in nginx but no running service found — investigate before touching.
- No maintenance window defined yet — confirm before disruptive changes.

## Related local repo(s)

<!-- Repo names only, not absolute paths. Paths vary per machine. -->

- unknown — to be confirmed
