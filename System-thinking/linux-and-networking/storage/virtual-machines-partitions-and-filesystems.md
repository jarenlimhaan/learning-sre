# Virtual machines, partitions, and filesystems

## Manage libvirt virtual machines

```bash
# List running virtual machines
virsh list

# Include stopped and shut-down virtual machines
virsh list --all

# Show a virtual machine's configuration and runtime details
virsh dominfo <vm-name>

# Start a defined virtual machine
virsh start <vm-name>

# Request a graceful guest shutdown
virsh shutdown <vm-name>

# Force a guest off only when graceful shutdown fails
virsh destroy <vm-name>

# Configure the guest to start when the host boots
virsh autostart <vm-name>

# Open the guest's configured text console
virsh console <vm-name>

# Export the persistent XML definition before changing it
virsh dumpxml <vm-name> > <vm-name>.xml

# Edit and validate the persistent domain definition
virsh edit <vm-name>
```

`virsh destroy` stops a guest like removing power; it does not delete its definition or disks. Keep the guest definition and storage lifecycle as separate concerns.

## Inspect and partition a disk

```bash
# Show disks, partitions, filesystem types, UUIDs, and mount points
lsblk --fs

# Show partition tables without modifying them
sudo fdisk --list

# Open an interactive partition editor for exactly one disk
sudo fdisk <disk-device>

# Ask the kernel to reread a changed partition table
sudo partprobe <disk-device>

# Verify the resulting device layout
lsblk
```

In `fdisk`, use `p` to inspect, `n` to prepare a new partition, `d` to prepare deletion, and `w` to commit changes. Nothing is durable until `w`, but once written, partition changes can make existing data inaccessible. Confirm the whole-disk device, never a partition, before editing.

## Create and identify filesystems

```bash
# Create an ext4 filesystem on an empty partition
sudo mkfs.ext4 <partition>

# Create an XFS filesystem on an empty partition
sudo mkfs.xfs <partition>

# Set or change an ext4 filesystem label
sudo e2label <partition> <label>

# Set or change an XFS filesystem label
sudo xfs_admin -L <label> <partition>

# Confirm filesystem type, label, and UUID
sudo blkid <partition>
```

Formatting is destructive. Resolve and inspect the exact partition with `lsblk --fs` immediately before running `mkfs`, and ensure it is not mounted or holding needed data.
