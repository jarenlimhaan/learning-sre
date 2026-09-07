# Network block devices

An NBD server exposes a block device or disk image across the network. The client attaches it as a local-looking device such as `/dev/nbd0`, after which normal partitioning, filesystem, and mount tools can operate on it.

Unlike NFS, NBD exports blocks rather than a shared filesystem. A filesystem that is not designed for concurrent access must not be mounted read/write by multiple clients.

```bash
# Install NBD client utilities on Ubuntu
sudo apt install nbd-client

# Load the client kernel module and permit up to 16 partitions per NBD device
sudo modprobe nbd max_part=16

# Attach a named export from an NBD server
sudo nbd-client <server> <port> /dev/nbd0 -N <export-name>

# Inspect the newly attached remote block device
lsblk --fs /dev/nbd0

# Ask the kernel to discover its partitions
sudo partprobe /dev/nbd0

# Mount one discovered filesystem
sudo mount /dev/nbd0p1 <mount-point>

# Unmount before disconnecting the remote block device
sudo umount <mount-point>

# Disconnect the NBD device
sudo nbd-client -d /dev/nbd0
```

Treat network loss as storage loss: applications may hang or see I/O errors. Unmount every filesystem and stop all users of the device before disconnecting it.
