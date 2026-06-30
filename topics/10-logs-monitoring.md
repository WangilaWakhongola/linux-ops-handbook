# 10. Logs & Monitoring

## Key log locations

```bash
/var/log/syslog          # general system log (Debian/Ubuntu)
/var/log/messages           # general system log (RHEL/CentOS)
/var/log/auth.log              # authentication attempts (Debian/Ubuntu)
/var/log/secure                  # authentication attempts (RHEL)
/var/log/dmesg                      # kernel ring buffer / boot messages
/var/log/apt/                          # package install history (Debian)
```

## Viewing logs

```bash
tail -f /var/log/syslog       # live-follow a log file
less /var/log/syslog              # paginated view, searchable with /
grep "failed" /var/log/auth.log      # search for specific events
journalctl                              # systemd's centralized log viewer
journalctl -f                              # live-follow all systemd logs
journalctl --since "10 min ago"               # time-filtered
journalctl -p err                                # only error-level and above
```

## Disk usage of logs

```bash
journalctl --disk-usage         # how much space journal logs use
sudo journalctl --vacuum-time=7d   # delete journal logs older than 7 days
du -sh /var/log/*                     # size of each log file/directory
```

## Basic monitoring tools

```bash
top / htop              # live CPU/memory/process view
vmstat 1                   # system performance snapshot every second
iostat -x 1                   # disk I/O stats
sar -u 1 5                       # CPU usage samples (needs sysstat)
uptime                              # load averages
```

## Common gotchas

- `journalctl` logs can balloon in size on busy systems — set up `--vacuum-time` or size limits in `/etc/systemd/journald.conf`.
- Log rotation (via `logrotate`, config in `/etc/logrotate.d/`) means old entries may be in `.gz` files, not the live log — check those too when investigating older issues.
- Timestamps in logs can be in different timezones from what you expect — confirm with `timedatectl` before drawing conclusions from "when" something happened.
