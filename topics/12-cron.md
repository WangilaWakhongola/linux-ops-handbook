# 12. Cron & Scheduled Tasks

## Editing your crontab

```bash
crontab -e          # edit your own crontab
crontab -l             # list current cron jobs
crontab -r                # remove all cron jobs (careful)
sudo crontab -u www-data -e  # edit another user's crontab
```

## Crontab syntax

```
* * * * * command-to-run
| | | | |
| | | | +---- day of week (0-6, Sunday=0)
| | | +------ month (1-12)
| | +-------- day of month (1-31)
| +---------- hour (0-23)
+------------ minute (0-59)
```

## Common examples

```bash
0 2 * * * /opt/scripts/backup.sh        # every day at 2:00 AM
*/15 * * * * /opt/scripts/healthcheck.sh   # every 15 minutes
0 0 * * 0 /opt/scripts/weekly-cleanup.sh      # every Sunday at midnight
0 9 1 * * /opt/scripts/monthly-report.sh        # 9 AM on the 1st of every month
```

## System-wide cron locations

```bash
/etc/crontab               # system crontab
/etc/cron.d/                  # drop-in cron job files
/etc/cron.daily/                # scripts run once a day
/etc/cron.hourly/                  # scripts run once an hour
/etc/cron.weekly/                     # scripts run once a week
```

## Logging cron output

```bash
0 2 * * * /opt/scripts/backup.sh >> /var/log/backup.log 2>&1
```

## Alternative: systemd timers

```ini
# /etc/systemd/system/backup.timer
[Unit]
Description=Run backup daily

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

```bash
sudo systemctl enable --now backup.timer
systemctl list-timers          # see all active timers
```

## Common gotchas

- Cron jobs run with a minimal environment — no full `$PATH`, no shell profile loaded. Use absolute paths to binaries and scripts.
- Silent failures are common — always redirect output to a log file, or you'll never know a job failed.
- `crontab -r` deletes the whole crontab instantly with no confirmation — easy to type by accident instead of `-e`.
