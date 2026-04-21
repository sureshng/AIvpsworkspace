# vps-prod-77.37.62.240 (Public VPS / Gateway)

## Connection

- **SSH alias:** `ssh vps-prod`
  *(User root, default port 22 — configure in `~/.ssh/config`, see `SETUP.md`.)*
- **Host:** 77.37.62.240
- **User:** root
- **WireGuard IP:** 10.0.0.1

## Architecture — IMPORTANT

This is the **public-facing VPS and WireGuard gateway** for the home server.

```
Internet → This VPS (77.37.62.240) → WireGuard (10.0.0.x) → Home server (10.0.0.3)
```

- All public domains resolve to this server's IP
- Nginx proxies all app traffic to the home server via WireGuard at `http://10.0.0.3/`
- SSH access to home server: port 2222 on this VPS → reverse tunnel → home server port 22
- Port 2223 on this VPS → reverse tunnel → home server port 22 (autossh-managed)
- Port 9092 on this VPS → reverse tunnel → home server port 9092

## Environment

- **Type:** production
- **Provider:** unknown (public VPS)
- **OS:** Linux (Ubuntu)
- **Region / timezone:** unknown

## What runs here

- **Nginx** — public-facing reverse proxy for all domains
- **WireGuard** — VPN gateway (`wg0`, IP 10.0.0.1/24, port 51820)
- **SSH tunnels** — reverse tunnels from home server bound on ports 2222, 2223, 9092

## WireGuard peers

| Peer | WireGuard IP | Notes |
|---|---|---|
| Home server | `10.0.0.3` | Active, latency ~48ms |
| Unknown peer | `10.0.0.2` | Unreachable as of 2026-04-21 |

## Domains & nginx sites

| Domain | Config file | Proxies to |
|---|---|---|
| `appofcag.space` | `cagappnative.conf` | `http://10.0.0.3/` |
| `media.cagutility.click` | `media-cagutility.conf` | `http://10.0.0.3/` |
| `77.37.62.240` (IP) | `cag-ip-apps.conf` | `http://10.0.0.3/<path>/` via app-locations |

## Key paths (on this VPS)

- Nginx sites: `/etc/nginx/sites-available/`, `/etc/nginx/sites-enabled/`
- Nginx app locations: `/etc/nginx/app-locations/` (modular path-based proxy configs)

## Services (systemd)

- `nginx.service` — public-facing reverse proxy
- `wg-quick@wg0.service` — WireGuard VPN

## Common operations

- Reload nginx: `nginx -s reload`
- Test nginx config: `nginx -t`
- Check WireGuard: `wg show`
- Check tunnel ports: `ss -tlnp | grep -E '2222|2223|9092'`
- Add new domain: create config in `sites-available/`, symlink to `sites-enabled/`, reload nginx

## Warnings / notes

- **Gateway server** — disrupting nginx or WireGuard here cuts access to ALL home server apps.
- This VPS has root SSH access — be careful with destructive commands.
- SSL/Certbot for new domains should run here (not on the home server).

## Related local repo(s)

- unknown — to be confirmed
