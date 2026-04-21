# Session Log — vps-prod-77.37.62.240 (Public VPS / Gateway)

Most recent sessions at the top.

---

## 2026-04-21 — Media server proxy setup

**Goal:** Configure VPS nginx to proxy `media.cagutility.click` to the home server.

**What we did:**
- Discovered VPS nginx pattern: all apps proxy to home server at `http://10.0.0.3/` via WireGuard
- Added `/etc/nginx/sites-available/media-cagutility.conf` (proxy `media.cagutility.click → http://10.0.0.3/`)
- Symlinked to sites-enabled and reloaded nginx
- Verified: `http://media.cagutility.click/icons/icon.png` → 200 ✅

**Findings:**
- VPS WireGuard IP: `10.0.0.1`; home server: `10.0.0.3`; unknown peer: `10.0.0.2` (unreachable)
- VPS has TWO reverse tunnel ports bound: 2222 and 2223 (both → home server SSH)
- Port 9092 also tunnelled from home server
- Existing nginx pattern: domain configs in `sites-available/`, path configs in `app-locations/`

**Open items:**
- [ ] Add SSL (Certbot) for `media.cagutility.click`
- [ ] Investigate unknown WireGuard peer at 10.0.0.2
- [ ] Confirm what port 9092 tunnel is used for

**Decisions:**
- SSL/Certbot for new domains runs on this VPS (not home server) — it's the public TLS termination point

---
