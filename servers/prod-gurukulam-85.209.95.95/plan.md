# Plan — prod-gurukulam-85.209.95.95

## Completed plans

---

### PLAN-001 — Clone media server to home server ✅

**Status:** Done (2026-04-21)
**Goal:** Replicate `media.appofcag.site` static file server on the home server.

**What was done:**
- Backed up nginx config + all 58 media files (22MB) locally to `backups/media-server/`
- Rsynced media files to home server at `/home/mediaserver/appmedia/`
- Created `media-cagutility.conf` on home server nginx
- Added DNS A record: `media.cagutility.click → 77.37.62.240` (VPS)
- Added `media-cagutility.conf` on VPS nginx to proxy `media.cagutility.click → 10.0.0.3` (home server via WireGuard)
- Verified: `http://media.cagutility.click/icons/icon.png` → 200 ✅

**Remaining:**
- [ ] Add HTTPS — run Certbot on VPS for `media.cagutility.click`

---

## Active plans

*(none)*
