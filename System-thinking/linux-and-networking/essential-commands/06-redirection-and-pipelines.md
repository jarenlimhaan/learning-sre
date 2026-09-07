# Redirection and pipelines

```bash
# Redirect standard output (stdout) to a file, creating or truncating it
<command> > <output-file>

# Do the same explicitly using stdout's file descriptor, 1
<command> 1> <output-file>

# Append stdout to a file instead of truncating it
<command> >> <output-file>

# Read standard input (stdin) from a file instead of the keyboard
<command> < <input-file>

# Redirect standard error (stderr), file descriptor 2, to a file
<command> 2> <error-file>

# Discard stderr by redirecting it to the null device
<command> 2> /dev/null

# Write stdout and stderr to separate files
<command> > <output-file> 2> <error-file>

# Redirect stdout to a file, then point stderr at stdout's destination; order matters
<command> > <all-output-file> 2>&1

# Bash shorthand that redirects both stdout and stderr to the same file
<command> &> <all-output-file>

# Exclude comments, sort the remaining lines, and align them into a table through pipelines
grep -v '^#' /etc/login.defs | sort | column -t

# Pass one string to bc through stdin; <<< creates a here string
bc <<< '1+2+3+4'

# Pass multiple literal lines through stdin until EOF; quoted EOF disables shell expansion
<command> <<'EOF'
<multi-line-input>
EOF
```

`>` truncates an existing file; `>>` appends to it.
