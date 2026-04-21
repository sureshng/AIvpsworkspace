# Session Log — home-server-192.168.86.69 (Home Server)

Most recent sessions at the top.

---

## 2026-04-21 — Media server setup + architecture discovery

**Goal:** Set up `media.cagutility.click` static file server on this machine.

**What we did:**
- Created `/home/mediaserver/appmedia/` (needed sudo — sysadmin can't write to `/home` directly)
- Rsynced 58 media files (22MB) from local backup to `/home/mediaserver/appmedia/`
- Created nginx config `media-cagutility.conf` pointing to `/home/mediaserver/appmedia/`
- Removed old `media-appofcag-space.conf` from sites-enabled (domain was already in use elsewhere)
- Changed ownership of `/home/mediaserver/` to `www-data` for nginx read access
- Discovered this is a home server behind NAT — port 80 is not directly internet-accessible
- Discovered VPS at 10.0.0.1 already proxies all traffic via WireGuard to this server (10.0.0.3)
- Fixed by adding `media-cagutility.conf` on the VPS to proxy `media.cagutility.click → 10.0.0.3`
- Verified: `http://media.cagutility.click/icons/icon.png` returns 200 ✅

**Findings:**
- `sudo nginx -s reload` does NOT reliably pick up new server blocks — use `sudo -n nginx -s stop && sudo -n nginx`
- `sudo` requires password for most commands; `nginx` and passwordless only for specific commands
- WireGuard peer 10.0.0.1 confirmed as the public VPS (77.37.62.240) — not Hostinger
- This server's WireGuard IP is `10.0.0.3`; the VPS is `10.0.0.1`
- All public domains proxy through VPS → WireGuard → this server (same pattern as `appofcag.space`)

**Open items:**
- [ ] Investigate why `cagattendanceapi` is unhealthy
- [ ] Investigate why `manageserverapi` is unhealthy
- [ ] Add SSL (Certbot) for `media.cagutility.click` — currently HTTP only
- [ ] Confirm local repo names for each Docker app

**Decisions:**
- Media files stored at `/home/mediaserver/appmedia/` (not `/var/www/`) to keep home server paths clean
- VPS nginx is the SSL termination point — Certbot should run on the VPS, not this server

---

## 2026-04-21 — Onboarding session

**Goal:** Onboard this server into the workspace and discover what's running.

**What we did:**
- Added `home-proxy-server` SSH alias to `~/.ssh/config` (Port 2222, User sysadmin)
- Copied public key via `ssh-copy-id`
- Scanned home directory, `/home/`, systemd services, Docker containers, nginx configs

**Findings:**
- This is NOT just a proxy — it's a full app server running 9 Docker containers
- Docker stack: `cagattendanceapi`, `cag-ayyapa`, `yathraapi`, `cagayyappa`, `cagappnative`, `manageserverapi`, `servermanageui`, `servermanager-db` (PostgreSQL), `devmysql`
- **Two containers are unhealthy:** `cagattendanceapi` (port 8091) and `manageserverapi` (port 8086)
- System services: nginx, MySQL, MS SQL Server, Docker, autossh reverse tunnel, Python monitor
- `reverse-tunnel.service` uses autossh to tunnel back to 10.0.0.1 (WireGuard peer) — critical for home network access
- Claude Code and GitHub Copilot are both installed on this server
- Nginx uses a modular `app-locations/` include pattern for path-based routing by IP

**Open items:**
- [x] Confirm what WireGuard peer 10.0.0.1 is → confirmed: the public VPS at 77.37.62.240
- [ ] Investigate why `cagattendanceapi` is unhealthy
- [ ] Investigate why `manageserverapi` is unhealthy
- [ ] Confirm local repo names for each Docker app
- [x] Check if `media.appofcag.space` needs SSL → replaced by `media.cagutility.click`

**Decisions:**
- Keeping existing `vps-prod` alias (root, port 22) intact — new `home-proxy-server` alias is sysadmin, port 2222

---
