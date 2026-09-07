# Scheduling, system health, kernel settings, and SELinux

## Cron, anacron, and at

```bash
# Edit or list the current user's recurring cron jobs
crontab -e
crontab -l

# Edit or list root's recurring cron jobs
sudo crontab -e
sudo crontab -l

# Edit another user's cron table
sudo crontab -e -u <user>

# Remove the current user's entire cron table; this does not prompt
crontab -r
```

A user crontab has five schedule fields followed by a command:

```cron
# minute hour day-of-month month day-of-week command
# Run every Sunday at 03:00; use absolute paths in scheduled jobs
0 3 * * 0 /absolute/path/to/command

# Run at minute 0 every four hours
0 */4 * * * /absolute/path/to/command
```

System `/etc/crontab` entries add a username between the five schedule fields and the command. Scripts placed in `/etc/cron.hourly`, `/etc/cron.daily`, `/etc/cron.weekly`, or `/etc/cron.monthly` must be executable and should not contain a filename extension.

```bash
# Edit anacron jobs, which can catch up after a machine was powered off
sudoedit /etc/anacrontab

# Validate anacrontab syntax; no output means success
anacron -T

# Schedule one interactive job
at '<time specification>'

# List pending one-time jobs
atq

# Display the commands stored in a job
at -c <job-id>

# Remove a pending job
atrm <job-id>
```

An anacron entry is `period-in-days delay-in-minutes job-id command`, for example `7 10 weekly_backup /absolute/path/to/script`.

## Check system capacity and filesystems

```bash
# Show filesystem capacity using human-readable units
df -h

# Summarize the space consumed by one directory tree
du -sh <directory>/

# Show RAM and swap totals; focus on the available column for usable memory
free -h

# Show uptime and 1-, 5-, and 15-minute load averages
uptime

# Show CPU architecture, cores, and capabilities
lscpu

# List PCI hardware
lspci

# Show important systemd dependencies as a tree
systemctl list-dependencies

# Check an unmounted XFS filesystem and repair detected metadata problems
sudo xfs_repair -v <device>

# Force-check an unmounted ext4 filesystem and automatically fix safe problems
sudo fsck.ext4 -v -f -p <device>
```

Never run repair tools against a mounted filesystem. Confirm the exact device and arrange a maintenance window or recovery environment first.

## Kernel runtime parameters

```bash
# List readable kernel runtime parameters
sudo sysctl -a

# Read one parameter
sysctl <parameter>

# Change a parameter until the next reboot
sudo sysctl -w <parameter>=<value>

# Read persistent-configuration rules
man 5 sysctl.d

# Edit a dedicated persistent configuration file
sudoedit /etc/sysctl.d/<name>.conf

# Apply one configuration file immediately
sudo sysctl -p /etc/sysctl.d/<name>.conf

# Reload all standard sysctl configuration files
sudo sysctl --system
```

Prefer narrowly named files under `/etc/sysctl.d/` to editing `/etc/sysctl.conf`. Record the old value and verify the effect after applying a change.

## Inspect SELinux state and contexts

```bash
# Report Enforcing, Permissive, or Disabled
getenforce

# Show the current user's SELinux context
id -Z

# Show file contexts; their fields are user:role:type:level
ls -Z <path>

# Show security contexts for all processes
ps axZ

# Display login-to-SELinux-user mappings
sudo semanage login -l

# Display SELinux users and the roles available to them
sudo semanage user -l
```

The type is usually the most important file label component; a process's equivalent restriction is called its domain. Permissive mode logs denials but does not enforce them.

## Inspect AppArmor state

```bash
# Show whether AppArmor is loaded and summarize enforced and complain-mode profiles
sudo aa-status

# Put one profile into enforce mode
sudo aa-enforce <profile-or-executable>

# Put one profile into complain mode for policy investigation
sudo aa-complain <profile-or-executable>

# Search kernel logs for AppArmor decisions
journalctl -k -g apparmor
```

Enforce mode blocks and logs disallowed actions. Complain mode logs policy violations without blocking them; use it temporarily while refining a profile, not as the desired steady state.
