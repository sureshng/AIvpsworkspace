# Troubleshooting

Generic diagnostic patterns that apply across servers. Server-specific
issues should go into that server's `session-log.md` or a runbook.

## Service won't start

1. Check status: `systemctl status <service>`
2. Read recent logs: `journalctl -u <service> -n 100 --no-pager`
3. Check config syntax (if applicable to the service)
4. Check port conflicts: `sudo ss -tlnp | grep <port>`
5. Check file permissions on config/data dirs
6. Check disk space: `df -h`

## API endpoint returning 500

1. Curl from the VPS itself first (rules out networking):
   `curl -v http://localhost:<port>/<path>`
2. Tail the app log while hitting the endpoint
3. Look for stack traces; map line numbers back to the local repo
4. Check recent deploys — what changed?
5. Check downstream dependencies (DB, cache, external APIs)

## Connection timeouts

1. Is the service actually listening? `ss -tlnp | grep <port>`
2. Firewall rules: `sudo ufw status` or `sudo iptables -L -n`
3. Cloud provider's firewall / security group (check their web UI)
4. DNS resolving correctly? `dig <hostname>`

## High CPU / memory

1. `top` or `htop` to find the process
2. `ps aux --sort=-%cpu | head` or `--sort=-%mem`
3. For JVM apps: check heap, thread count, GC logs
4. Recent traffic spike? Check access logs

## Disk full

1. `df -h` to find which mount is full
2. `du -sh /var/log/* /var/lib/* /tmp/* 2>/dev/null | sort -h`
3. Common culprits: old logs, old Docker images, old kernels,
   journal files, orphaned tmp files

## SSH connection issues

1. `ssh -v <alias>` for verbose output
2. Check `~/.ssh/config` has the entry
3. Check key permissions: `ls -la ~/.ssh/` — keys should be `600`
4. Has the server's host key changed? Warning would say so — if
   legitimate (reinstall/migration), update `~/.ssh/known_hosts`
