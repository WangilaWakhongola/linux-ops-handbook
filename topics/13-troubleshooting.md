# 13. Troubleshooting Cheatsheet

A quick first-response checklist for common IT Ops scenarios.

## Server is slow

```bash
top                      # what's eating CPU/memory?
free -h                     # is memory exhausted?
df -h                          # is disk full?
vmstat 1                          # check I/O wait, swap activity
iostat -x 1                          # disk I/O bottlenecks
```

## Service won't start

```bash
systemctl status myservice          # check current state
journalctl -u myservice -xe            # check detailed error logs
sudo myservice-binary --test             # many services have a config-test flag
ss -tulpn | grep <port>                     # is the port already in use?
```

## Out of disk space

```bash
df -h                                       # which filesystem is full
du -sh /* 2>/dev/null | sort -rh | head -10    # find large directories
journalctl --disk-usage                          # check journal log size
find / -name "*.log" -size +100M                    # find oversized log files
```

## Can't connect to a server

```bash
ping host                  # basic reachability
traceroute host               # where does it break
nc -zv host port                 # is the specific port open
ss -tulpn | grep <port>             # is something actually listening locally
sudo ufw status                        # local firewall blocking it?
```

## User can't log in

```bash
sudo tail -f /var/log/auth.log     # watch login attempts live
id username                           # does the account exist / what groups
sudo chage -l username                   # is the password expired
sudo passwd -S username                     # account locked?
```

## High load average

```bash
uptime                  # see the load numbers
top                        # sort by CPU, see what's driving it
ps aux --sort=-%cpu | head     # top CPU consumers
vmstat 1                          # check if it's I/O wait, not CPU
```

## File permission errors

```bash
ls -l file                  # check current permissions/ownership
id                              # confirm your own uid/groups
sudo -l                            # what can you actually run as sudo
namei -l /path/to/file                # check permissions on every parent dir
```

## General first-response order

1. Reproduce the issue and note the exact error message.
2. Check the relevant service status (`systemctl status`).
3. Check logs (`journalctl -u`, `/var/log/...`).
4. Check resources (disk, memory, CPU) aren't the root cause.
5. Check network/firewall if it's a connectivity issue.
6. Make one change at a time and re-test — don't change five things at once.
