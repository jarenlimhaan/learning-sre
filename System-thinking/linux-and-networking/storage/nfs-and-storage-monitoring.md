# NFS and storage monitoring

## Export an NFS filesystem

```bash
# Install the Ubuntu NFS server
sudo apt install nfs-kernel-server

# Define exported directories and allowed clients
sudoedit /etc/exports

# Re-export entries and remove exports deleted from the configuration
sudo exportfs -r

# Show active exports and their effective options
sudo exportfs -v

# Read export syntax and options
man 5 exports
```

Example `/etc/exports` entries:

```exports
# Read/write access for one subnet, with completed writes committed before acknowledgement
/srv/data 10.0.16.0/24(rw,sync,no_subtree_check)

# Read-only access for one client
/srv/data <client-ip>(ro,sync,no_subtree_check)
```

Do not insert whitespace between a client and its opening parenthesis. Keep root squashing enabled unless remote client root genuinely needs root-equivalent access to the export.

## Mount an NFS share

```bash
# Install Ubuntu's NFS client utilities
sudo apt install nfs-common

# Create the local mount point
sudo mkdir -p <mount-point>

# Mount a remote export for the current session
sudo mount -t nfs <server>:/<export-path> <mount-point>

# Verify the mounted NFS filesystem and its options
findmnt -t nfs,nfs4

# Unmount the remote filesystem
sudo umount <mount-point>
```

Persistent NFS entry in `/etc/fstab`:

```fstab
<server>:/<export-path> <mount-point> nfs defaults,_netdev 0 0
```

`_netdev` identifies a network-dependent mount so startup logic does not treat it like local storage.

## Investigate storage performance

```bash
# Install iostat and pidstat on Ubuntu
sudo apt install sysstat

# Show device I/O averages since boot
iostat

# Show extended device statistics every two seconds
iostat -xz 2

# Show per-process read and write rates every two seconds
pidstat -d 2

# Show processes currently performing I/O
sudo iotop

# Show processes with open files below a mount point
sudo lsof +f -- <mount-point>

# Show processes preventing a filesystem from being unmounted
sudo fuser -vm <mount-point>
```

High throughput and high IOPS are different saturation modes. In extended `iostat` output, evaluate latency and utilization with queue depth and device capabilities rather than relying on one metric alone.
