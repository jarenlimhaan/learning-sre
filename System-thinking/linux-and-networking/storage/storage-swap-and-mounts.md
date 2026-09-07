# Storage, swap, and filesystem mounts

## Discover storage

```bash
# Display block devices, partitions, filesystem types, UUIDs, and mount points
lsblk --fs

# Show filesystem signatures and stable UUIDs
sudo blkid

# Show mounted filesystems in a tree
findmnt

# Limit the mount tree to selected filesystem types
findmnt -t xfs,ext4

# Display filesystem capacity
df -hT
```

Prefer UUIDs in persistent mount configuration because names such as `/dev/sdb` can change when hardware discovery order changes.

## Configure swap

```bash
# List active swap areas
swapon --show

# Initialize an unused partition as swap
sudo mkswap <partition>

# Enable the swap area immediately
sudo swapon --verbose <partition>

# Disable one swap area
sudo swapoff <partition-or-file>

# Allocate a swap file efficiently when the filesystem supports it
sudo fallocate -l <size> /swapfile

# Restrict the file before it can contain process memory
sudo chmod 600 /swapfile

# Initialize and enable the swap file
sudo mkswap /swapfile
sudo swapon /swapfile
```

Persistent swap entry in `/etc/fstab`:

```fstab
/swapfile none swap defaults 0 0
```

## Mount filesystems

```bash
# Attach a filesystem to an existing directory
sudo mount <device> <mount-point>

# Detach it after processes stop using it
sudo umount <mount-point>

# Mount read-only
sudo mount -o ro <device> <mount-point>

# Mount with common restrictions for data-only storage
sudo mount -o rw,noexec,nosuid,nodev <device> <mount-point>

# Change independent options on an existing mount
sudo mount -o remount,rw,noexec,nosuid,nodev <mount-point>

# Test every fstab entry without rebooting
sudo mount -a

# Reread generated systemd mount units after editing fstab
sudo systemctl daemon-reload
```

An `/etc/fstab` line has `source target type options dump pass`, for example:

```fstab
UUID=<filesystem-uuid> /srv/data ext4 defaults 0 2
```

Use pass `1` only for the root filesystem, `2` for other filesystems that should be checked, and `0` for mounts that should not be checked. Keep a root session open while testing fstab changes; a bad entry can disrupt the next boot.
