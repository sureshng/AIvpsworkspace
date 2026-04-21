# Session Log — home-proxy-server-77.37.62.240

Most recent sessions at the top.

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
- Media server running at `media.appofcag.space` → `/var/www/appmedia` (HTTP only, no SSL)
- Claude Code and GitHub Copilot are both installed on this server
- Nginx uses a modular `app-locations/` include pattern for path-based routing by IP

**Open items:**
- [ ] Investigate why `cagattendanceapi` is unhealthy
- [ ] Investigate why `manageserverapi` is unhealthy
- [ ] Confirm what WireGuard peer 10.0.0.1 is (Hostinger VPS?)
- [ ] Confirm local repo names for each Docker app
- [ ] Check if `media.appofcag.space` needs SSL added

**Decisions:**
- Keeping existing `vps-prod` alias (root, port 22) intact — new `home-proxy-server` alias is sysadmin, port 2222

---
