# Session Log — prod-gurukulam-85.209.95.95

Most recent sessions at the top.

---

## 2026-04-21 — Deep scan, media backup & service discovery

**Goal:** Understand full nginx/service layout and back up media server for replication.

**What we did:**
- Scanned all nginx sites-enabled: 4 domain configs + `app-locations/` modular includes
- Read all systemd service files including hidden `cag-api.service`
- Discovered shell-script-managed `cagformapi` (not systemd) — port 8092
- Backed up media server: nginx config + all 58 files (22MB) to `backups/media-server/`
- Created `plan.md` with PLAN-001 (media server replication)

**Findings:**
- 8 domains served: `caggurukulam.com`, `cagusa.org`, `cagapp.appofcag.site`, `caggurukulam.appofcag.site`, `appofcag.site`, `api.appofcag.site`, `api.gurukulam.appofcag.site`, `media.appofcag.site`
- `cag-api.service` exists but is **stopped** (attendance API, jar at `/home/deploy/backend/cagattendanceapi/attendanceapi.jar`)
- `cagformapi` (port 8092) managed via scripts in `/home/deploy/scripts/` — no systemd, won't survive reboot
- Source code on server: `/home/source/javapp/cagattendanceapi/` and `/home/source/nativeapp/cagnativeapp/`
- Media server config (SSL via Certbot): `/etc/nginx/sites-available/media.conf` → `/var/www/appmedia`

**Open items:**
- [ ] Confirm related local repo names
- [ ] Confirm what `api1` serves vs `prodcaggurukulamapi`
- [ ] Convert `cagformapi` script to a systemd service (won't survive reboots)
- [ ] Investigate stopped `cag-api.service`
- [ ] Confirm provider, region, timezone

**Decisions:**
- Media replication target: home server via `media.cagutility.click` (PLAN-001 complete)

---

## 2026-04-21 — Onboarding session

**Goal:** Set up this server in the workspace and discover what's running.

**What we did:**
- Added `prod-gurukulam` SSH alias to `~/.ssh/config` (macOS)
- Copied public key (`id_vps_cag.pub`) to server via `ssh-copy-id`
- Scanned home folder, `/var/www`, `/opt`, `/etc/nginx`, `/var/log`
- Read systemd service files for `api1.service` and `prodcaggurukulamapi.service`
- Created `server.md` with discovered facts

**Findings:**
- Two Java Spring Boot APIs running: `prodcaggurukulamapi` (port 8095) and `api1`
- App jars live under `/home/deploy/` (not `/var/www` or `/opt`)
- MySQL and Nginx also running
- Media storage at `/var/www/appmedia`
- Log files in both `/var/log/` and `/home/deploy/prod/backend/caggurukulamapi/logs/`
- `.expo` and `.npm` in root home — Node/Expo tooling present

**Open items:**
- [ ] Confirm related local repo names
- [ ] Discover nginx virtual host config (sites-enabled)
- [ ] Confirm what `api1` serves vs `prodcaggurukulamapi`
- [ ] Confirm provider, region, timezone
- [ ] Set up maintenance window info if applicable

**Decisions:**
- Using `prod-gurukulam` as the SSH alias going forward (not raw IP)

---
