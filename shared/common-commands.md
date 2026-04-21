# Common Commands

Reference of commands that tend to be useful across most VPS servers.
Server-specific commands belong in that server's `server.md` or a
runbook instead.

## System health

```bash
# Resource usage
top
htop
free -h
df -h
du -sh /var/log/*

# Who's logged in
w
last | head
```

## Services (systemd)

```bash
systemctl status <service>
sudo systemctl restart <service>
sudo systemctl stop <service>
sudo systemctl start <service>
sudo systemctl enable <service>
sudo systemctl disable <service>
journalctl -u <service> -n 100 --no-pager
journalctl -u <service> -f        # follow live
```

## Logs

```bash
tail -f <path>
tail -n 200 <path>
grep -i error <path> | tail -50
zgrep -i error <path>.gz          # search rotated logs
```

## Network

```bash
ss -tlnp                          # listening ports
ss -tnp                           # active connections
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:<port>/<path>
```

## Processes

```bash
ps aux | grep <name>
pgrep -af <name>
lsof -i :<port>
```

## Disk cleanup

```bash
sudo apt autoremove
sudo apt clean
sudo journalctl --vacuum-time=7d
```

## File transfer

```bash
# Use SSH aliases, not raw host addresses
scp file.txt <alias>:/remote/path/
scp <alias>:/remote/path/file.txt ./
rsync -avz ./localdir/ <alias>:/remote/dir/
```
