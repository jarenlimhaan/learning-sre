# Text processing and search

## View, transform, and compare text

```bash
# Print an entire file to standard output
cat <file>

# Print a file in reverse line order
tac <file>

# Print the first 10 lines of a file
head <file>

# Print the first 20 lines; -n selects the line count
head -n 20 <file>

# Print the last 10 lines of a file
tail <file>

# Print the last 20 lines; -n selects the line count
tail -n 20 <file>

# Keep watching a log for appended lines; -f means follow
tail -f <log-file>

# Preview a global substitution; s means substitute and g replaces every match per line
sed 's/<old>/<new>/g' <file>

# Edit in place (-i) while saving the original with a .bak suffix
sed -i.bak 's/<old>/<new>/g' <file>

# Split on spaces (-d delimiter) and print the first field (-f 1)
cut -d ' ' -f 1 <file>

# Split on commas and print the third field
cut -d ',' -f 3 <file>

# Sort a file's lines alphanumerically
sort <file>

# Sort first, then remove adjacent duplicate lines with uniq
sort <file> | uniq

# Sort and remove duplicates in one command; -u means unique
sort -u <file>

# Show the line-by-line differences between two files
diff <file-1> <file-2>

# Show differences with surrounding context; -c means context format
diff -c <file-1> <file-2>

# Show both files in parallel columns; -y means side-by-side
diff -y <file-1> <file-2>

# Show a side-by-side comparison using the dedicated sdiff command
sdiff <file-1> <file-2>
```

## Search text with grep

```bash
# Print lines containing a case-sensitive pattern
grep '<pattern>' <file>

# Ignore letter case; -i means ignore case
grep -i '<pattern>' <file>

# Match only a complete word; -w means word-regexp
grep -w '<word>' <file>

# Print nonmatching lines; -v inverts the match
grep -v '<excluded-pattern>' <file>

# Print only the matched portion instead of the entire line; -o means only matching
grep -o '<pattern>' <file>

# Prefix matching lines with their line numbers; -n means line number
grep -n '<pattern>' <file>

# Search all files below a directory; -r means recursive
grep -r '<pattern>' <directory>/

# Search recursively (-r), ignore case (-i), and highlight matches when output supports color
grep -ri --color=auto '<pattern>' <directory>/

# Match only lines beginning with the specified text; ^ anchors the start
grep '^<starts-with>' <file>

# Match only lines ending with the specified text; $ anchors the end
grep '<ends-with>$' <file>

# Use extended regular expressions; -E enables operators such as |, +, ?, and {}
grep -E '<pattern>' <file>

# Recursively (-r) use extended regex (-E) to match either word with |
grep -Er 'enabled|disabled' <directory>/

# Match between three and five consecutive zeroes with {3,5}
grep -Er '0{3,5}' <directory>/

# Match "disable" or "disabled" because ? makes the preceding d optional
grep -Er 'disabled?' <directory>/

# Match "cat" or "cut" because [au] accepts either character
grep -Er 'c[au]t' <directory>/

# Match /dev/ followed by lowercase letters and an optional final digit
grep -Er '/dev/[a-z]*[0-9]?' <directory>/

# Match "https" only when its next character is not a colon; [^:] is a negated set
grep -Er 'https[^:]' <directory>/
```

Quote glob and regular-expression patterns so the shell does not expand them before the command receives them.
