# Power, systemd, and logs

## Reboot and shutdown

```bash
# Reboot through systemd, allowing services to stop cleanly
sudo systemctl reboot

# Power off through systemd
sudo systemctl poweroff

# Schedule a shutdown at a 24-hour clock time
sudo shutdown <HH:MM>

# Schedule a reboot a number of minutes from now and notify logged-in users
sudo shutdown -r +<minutes> '<message>'

# Cancel a pending shutdown or reboot
sudo shutdown -c

# Force a reboot when normal service shutdown is stuck
sudo systemctl reboot --force
```

Using `--force` twice is comparable to a hardware reset and risks data loss or filesystem corruption. Reserve it for emergencies.

## Manage services

```bash
# Display a service's unit definition and drop-ins
systemctl cat <name>.service

# Show runtime state, recent logs, PID, and enablement information
systemctl status <name>.service

# Check whether a service is configured to start at boot
systemctl is-enabled <name>.service

# Start or stop a service now
sudo systemctl start <name>.service
sudo systemctl stop <name>.service

# Restart a service, interrupting the current process
sudo systemctl restart <name>.service

# Reload configuration without a full restart when the service supports it
sudo systemctl reload <name>.service

# Prefer a reload and fall back to restart when reload is unsupported
sudo systemctl reload-or-restart <name>.service

# Enable a service at boot and start it immediately
sudo systemctl enable --now <name>.service

# Stop a service and disable its boot-time start
sudo systemctl disable --now <name>.service

# Prevent all manual and dependency-based starts
sudo systemctl mask <name>.service

# Remove that hard block
sudo systemctl unmask <name>.service

# List every loaded service, including inactive units
systemctl list-units --type=service --all

# Restore a vendor unit after a full local edit
sudo systemctl revert <name>.service
```

Disabling controls boot-time enablement; it does not necessarily stop a running service or prevent another unit from starting it. Masking is the stronger block.

## Create a service

```ini
# /etc/systemd/system/<name>.service
[Unit]
Description=<short description>
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=<service-user>
ExecStart=<absolute-command-and-arguments>
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```bash
# Make systemd reread changed unit files
sudo systemctl daemon-reload

# Validate a unit file for common errors
systemd-analyze verify /etc/systemd/system/<name>.service

# Enable and start the new service
sudo systemctl enable --now <name>.service
```

Run services as a dedicated unprivileged user unless they genuinely require root. `ExecStart` is not interpreted by a shell unless a shell is explicitly invoked.

## Query system logs

```bash
# Show all journal entries visible to the current user
journalctl

# Show logs for one systemd unit
journalctl -u <name>.service

# Show entries emitted by one executable
journalctl <absolute-executable-path>

# Jump to the newest entries
journalctl -e

# Follow new log entries continuously
journalctl -f

# Show error-priority messages and anything more severe
journalctl -p err

# Limit results to a time range
journalctl --since '<time-or-date>' --until '<time-or-date>'

# Show the current boot, then the previous boot
journalctl -b 0
journalctl -b -1

# Write a test message into the system log
logger '<message>'

# Show traditional text logs maintained under /var/log
sudo ls -alh /var/log/

# Show each account's most recent login
lastlog
```

Use `journalctl -u` and `systemctl status` together: status gives a quick snapshot, while the journal supplies the fuller timeline.
