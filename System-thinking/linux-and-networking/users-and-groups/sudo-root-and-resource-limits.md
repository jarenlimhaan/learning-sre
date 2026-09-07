# Sudo, root access, and resource limits

## Delegated privileges

```bash
# Add a user to Ubuntu's broadly privileged sudo group
sudo gpasswd --add <user> sudo

# Remove a user from the sudo group
sudo gpasswd --delete <user> sudo

# Safely validate and edit the main sudo policy
sudo visudo

# Safely edit a dedicated policy fragment
sudo visudo -f /etc/sudoers.d/<policy-name>

# Check the current user's sudo permissions
sudo -l

# Run one command as another user
sudo --user <user> <command>

# Open a login shell as root through sudo
sudo --login

# Open a root login shell using root's password
su --login
```

Example sudoers rules:

```sudoers
# Allow one user to run any command as any user
<user> ALL=(ALL) ALL

# Apply the same rule to every member of a group
%<group> ALL=(ALL) ALL

# Permit only two commands; always use their absolute paths
<user> ALL=(ALL) /usr/bin/ls, /usr/bin/stat

# Permit commands without a password; use sparingly
<user> ALL=(ALL) NOPASSWD: ALL
```

Prefer narrowly scoped files in `/etc/sudoers.d/` and always edit them through `visudo`. A user allowed to edit or execute a sufficiently powerful program can often obtain unrestricted root access.

## Root password access

```bash
# Set the root password, enabling password-based root access where policy permits it
sudo passwd root

# Unlock root's password
sudo passwd --unlock root

# Lock password-based root login; sudo and configured SSH keys may still work
sudo passwd --lock root
```

Before locking root, confirm at least one tested account has working sudo access. Locking a password does not necessarily disable every authentication method.

## Per-user resource limits

```bash
# Read the limits.conf syntax and available limit items
man 5 limits.conf

# Edit PAM session limits
sudoedit /etc/security/limits.conf

# Show soft limits for the current shell
ulimit -aS

# Show hard limits for the current shell
ulimit -aH

# Start a login session as another user to test newly configured limits
sudo -iu <user>
```

Example limit entries use `domain type item value`:

```text
# Default soft CPU-time limit of five minutes
* soft cpu 5

# Group members start at 20 processes
@<group> soft nproc 20

# One user may never exceed 30 processes
<user> hard nproc 30

# Set both soft and hard maximum file size to 1024 KiB
<user> - fsize 1024
```

Soft limits are the session defaults; users may raise them only as far as the hard limits. Limits normally apply to new login sessions, so test them in a fresh session.
