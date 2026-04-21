# Plan — prod-gurukulam-85.209.95.95

## Active plans

---

### PLAN-001 — Clone media server to a new VPS

**Status:** In progress
**Goal:** Replicate `media.appofcag.site` (static file server) on a second VPS.

**What we have (backed up locally):**
- Nginx config: `backups/media-server/media.conf`
- All media files (58 files, 22MB): `backups/media-server/appmedia/`

**Steps to deploy on new VPS:**
- [ ] Onboard new VPS into this workspace
- [ ] Install nginx on new VPS
- [ ] `rsync` `appmedia/` to `/var/www/appmedia/` on new VPS
- [ ] Copy and adapt `media.conf` (update domain or keep same if using DNS failover)
- [ ] Point domain to new VPS IP (or use new subdomain)
- [ ] Run Certbot to issue SSL cert for the domain
- [ ] Enable nginx site and test

**Notes:**
- Media files are served with 30-day cache / `immutable` headers — no server-side state
- CORS is open (`*`) — safe to serve from any domain/IP
- SSL cert on source: `/etc/letsencrypt/live/media.appofcag.site/`

---
