# Help, login, and navigation

## Help and command discovery

```bash
# Show the command's built-in usage summary and available flags
<command> --help

# Open the command's full manual page
man <command>

# Open printf from manual section 1 (user commands)
man 1 printf

# Open printf from manual section 3 (library functions)
man 3 printf

# Build or refresh the manual-page search database; sudo runs it as root
sudo mandb

# Search all manual-page names and descriptions for a keyword
apropos <keyword>

# Search only sections 1 (user commands) and 8 (admin commands)
apropos -s 1,8 <keyword>
```

## Login and system identity

```bash
# Show all network interfaces and their IP addresses; "address" can be shortened to "a"
ip address

# Open an encrypted remote shell as the specified user
ssh <user>@<host-or-ip>

# Connect over a non-default SSH port; -p selects the port
ssh -p <port> <user>@<host-or-ip>

# End the current shell or SSH session
exit
```

## Filesystem navigation and listing

```bash
# Print the absolute path of the current working directory
pwd

# List visible entries in the current directory
ls

# List all entries (-a) in long format (-l) with human-readable sizes (-h)
ls -alh

# Apply the same detailed listing to a specific path
ls -alh <path>

# Change to a specified directory
cd <path>

# Change to the parent of the current directory
cd ..

# Return to the previous working directory
cd -

# Return to the current user's home directory
cd
```
