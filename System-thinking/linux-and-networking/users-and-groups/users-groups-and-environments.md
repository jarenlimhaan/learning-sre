# Users, groups, and environments

## User accounts

```bash
# Create an interactive user with a home directory, primary group, and login shell
sudo adduser <user>

# Set or change a user's password
sudo passwd <user>

# Create a system account without a home directory; use this for a daemon, not a person
sudo adduser --system --no-create-home <service-user>

# Show the current user's numeric identity and group memberships
id

# Show another user's identity and group memberships
id <user>

# Print only the current username
whoami

# Inspect account records such as UID, primary GID, home directory, and login shell
getent passwd <user>

# Move a user's home directory while updating the account record
sudo usermod --home <new-home> --move-home <user>

# Rename a user account
sudo usermod --login <new-name> <old-name>

# Change a user's login shell
sudo usermod --shell <shell-path> <user>

# Lock password authentication for an account; SSH keys may still permit access
sudo usermod --lock <user>

# Unlock the account's password
sudo usermod --unlock <user>

# Set an account expiration date in YYYY-MM-DD format
sudo usermod --expiredate <date> <user>

# Remove the account expiration date
sudo usermod --expiredate '' <user>

# Force a password change at the next login
sudo chage --lastday 0 <user>

# Require a password change every 30 days
sudo chage --maxdays 30 <user>

# Display password-aging information
sudo chage --list <user>

# Delete an account but retain its home directory
sudo deluser <user>

# Delete an account together with its home directory and mail spool
sudo deluser --remove-home <user>
```

Account names are labels; access checks ultimately use numeric UIDs and GIDs. Review ownership of retained files before reusing an old UID.

## Groups and memberships

```bash
# Create a group
sudo groupadd <group>

# Show a user's primary and supplementary groups
groups <user>

# Add a user to a supplementary group
sudo gpasswd --add <user> <group>

# Remove a user from a supplementary group
sudo gpasswd --delete <user> <group>

# Change a user's primary group
sudo usermod --gid <group> <user>

# Rename a group
sudo groupmod --new-name <new-name> <old-name>

# Delete a group that is not any user's primary group
sudo groupdel <group>

# Inspect the system's group record for one group
getent group <group>
```

A new login session is normally required before changed supplementary memberships take effect. Membership in privileged groups such as `sudo` or `docker` should be treated as administrative access.

## Login environments and templates

```bash
# Print the current process environment
printenv

# Print one environment variable
printenv <VARIABLE>

# Set a shell variable in the current shell
<VARIABLE>=<value>

# Export it so child processes inherit it
export <VARIABLE>

# Edit system-wide login environment assignments
sudoedit /etc/environment

# Add a system-wide login script; files here must end in .sh
sudoedit /etc/profile.d/<name>.sh

# Inspect the template copied into newly created home directories
ls -la /etc/skel/

# Edit the Bash template that future users will receive
sudoedit /etc/skel/.bashrc

# Edit one existing user's Bash configuration
sudoedit /home/<user>/.bashrc
```

`/etc/environment` contains assignments rather than shell logic. Put shell commands in `/etc/profile.d/*.sh`; changes to `/etc/skel` affect future users, not existing home directories.
