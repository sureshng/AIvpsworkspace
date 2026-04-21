# Session Log — prod-gurukulam-85.209.95.95

Most recent sessions at the top.

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
