# Shell maintenance automation

## Build and run a Bash script

```bash
# Create a script file, then open it in an editor
touch <script>.sh
vim <script>.sh

# Make the script executable for its owner
chmod u+x <script>.sh

# Execute it from the current directory
./<script>.sh

# Ask Bash to check syntax without running the script
bash -n <script>.sh

# Trace expanded commands while troubleshooting
bash -x <script>.sh
```

A basic maintenance script:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

readonly LOG_FILE='/var/log/<task>.log'

main() {
  printf '%s Starting maintenance\n' "$(date --iso-8601=seconds)" >> "$LOG_FILE"
  <command> >> "$LOG_FILE" 2>&1
}

main "$@"
```

The shebang selects the interpreter when the file is executed directly. Quote variable expansions, use absolute paths in scheduled scripts, and send both normal output and errors to a deliberate destination.

## Variables, conditions, and loops

```bash
# Save a command-line argument; braces make the variable boundary explicit
target="${1:-}"

# Stop with a useful message when a required argument is missing
if [[ -z "$target" ]]; then
  echo "Usage: $0 <target>" >&2
  exit 2
fi

# Process every matching item safely, including names containing spaces
for file in /var/log/*.log; do
  [[ -e "$file" ]] || continue
  gzip -- "$file"
done

# Capture a command's success or failure
if systemctl is-active --quiet <name>.service; then
  echo 'service is active'
else
  echo 'service is inactive' >&2
fi
```

## Archive maintenance data

```bash
# Create a timestamped gzip-compressed archive
sudo tar -czf "/var/backups/<name>-$(date +%F).tar.gz" <source-path>

# List the archive before relying on it
tar -tzf <archive>.tar.gz

# Test a command sequence without changing files where supported
rsync -av --dry-run <source>/ <destination>/
```

A backup is only trustworthy after a restore test. Keep scripts idempotent where possible so rerunning after a partial failure does not compound damage.
