# 7. Systemd & Services

Most modern distros use `systemd` to manage services. This is core day-to-day IT Ops territory.

## Checking service status

```bash
systemctl status nginx        # detailed status
systemctl is-active nginx       # active / inactive
systemctl is-enabled nginx        # enabled / disabled at boot
systemctl list-units --type=service     # all loaded services
```

## Starting, stopping, restarting

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx      # reload config without dropping connections
```

## Enabling / disabling at boot

```bash
sudo systemctl enable nginx        # start automatically on boot
sudo systemctl disable nginx         # don't start automatically
sudo systemctl enable --now nginx       # enable + start in one command
```

## Logs for a service

```bash
journalctl -u nginx            # all logs for a unit
journalctl -u nginx -f           # live-follow logs
journalctl -u nginx --since "1 hour ago"   # time-filtered logs
journalctl -xe                     # recent logs with extra context, useful after a failure
```

## Creating a simple custom service

```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My App
After=network.target

[Service]
ExecStart=/usr/bin/python3 /opt/myapp/app.py
Restart=always
User=emmanuel

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload      # reload unit files after editing
sudo systemctl enable --now myapp   # enable and start your new service
```

## Common gotchas

- After editing a `.service` file, you must run `daemon-reload` or systemd keeps using the old definition.
- `restart` vs `reload`: restart drops connections; reload (if the service supports it) applies new config gracefully.
- A failed service won't always show in `systemctl status` clearly — check `journalctl -u <service> -xe` for the real error.
