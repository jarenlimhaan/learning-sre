# Files, links, and permissions

## Create, copy, move, and remove

```bash
# Create an empty file or update an existing file's timestamps
touch <file>

# Create one directory
mkdir <directory>

# Create a directory path including missing parents; -p also ignores existing directories
mkdir -p <parent>/<child>

# Copy one file to a new path
cp <source-file> <destination>

# Copy a directory and everything below it recursively; -r means recursive
cp -r <source-directory>/ <destination-directory>/

# Move an item to another path, or rename it when the destination is a new name
mv <source> <destination>

# Prompt before removing a file; -i means interactive
rm -i <file>

# Remove a directory tree; -r is recursive and -I asks once before a large removal
rm -rI <directory>/
```

> `rm` does not use a recycle bin. Confirm the resolved path before using recursive removal.

## Links and file metadata

```bash
# Show file metadata, including inode, permissions, ownership, size, and timestamps
stat <path>

# Create a hard link; both names point to the same inode and data
ln <target-file> <link-name>

# Create a symbolic link; -s makes link-name store the target path
ln -s <target-path> <link-name>

# Print the target path stored in a symbolic link
readlink <symbolic-link>

# Resolve every link component; -f prints the final canonical absolute path
readlink -f <symbolic-link>

# Long-list both paths and show inode numbers; -l is long format and -i shows inodes
ls -li <path> <link-name>
```

Hard links cannot cross filesystems and normally cannot target directories. Prefer an absolute target when a symbolic link may be accessed from different locations.

## Ownership and permissions

```bash
# Long-list a path to show its type, permissions, owner, and group
ls -l <path>

# Show detailed metadata, including symbolic and octal permissions
stat <path>

# List the groups to which the current user belongs
groups

# Change a path's group owner
chgrp <group> <path>

# Change a path's user owner; sudo runs chown with root privileges
sudo chown <user> <path>

# Change both user and group ownership in one operation
sudo chown <user>:<group> <path>

# Change ownership throughout a directory tree; -R means recursive
sudo chown -R <user>:<group> <directory>/

# Add write permission for the owning user; u=user, +=add, w=write
chmod u+w <path>

# Remove read permission from others; o=other, -=remove, r=read
chmod o-r <path>

# Set the group's permissions to read-only; = replaces the current group bits
chmod g=r <path>

# Remove every group permission by assigning an empty permission set
chmod g= <path>

# Add user read/write, set group read-only, and remove all permissions from others
chmod u+rw,g=r,o= <path>

# Set file permissions to user read/write, group read, others none (640)
chmod 640 <file>

# Set directory permissions to user rwx and group/others read plus traverse (755)
chmod 755 <directory>
```

For directories, `r` lists entries, `w` creates/removes entries, and `x` permits traversal. Recursive `chown` and `chmod` can change an entire tree; verify the target first.
