# Finding files

```bash
# Search below a path for an exact case-sensitive name; -name accepts shell patterns
find <search-path> -name '<name-or-pattern>'

# Search by name without case sensitivity; -iname means insensitive name
find <search-path> -iname '<case-insensitive-pattern>'

# Find regular files (-type f) whose names end in .jpg
find <search-path> -type f -name '*.jpg'

# Find directories (-type d) with the specified name
find <search-path> -type d -name '<directory-name>'

# Find entries whose contents changed less than five minutes ago; -mmin uses minutes
find <search-path> -mmin -5

# Find entries modified less than one 24-hour period ago; -mtime uses 24-hour units
find <search-path> -mtime -1

# Find entries whose metadata changed less than five minutes ago; -cmin uses minutes
find <search-path> -cmin -5

# Find regular files exactly 512 KiB in size; k means KiB
find <search-path> -type f -size 512k

# Find regular files larger than 10 MiB; + means greater than and M means MiB
find <search-path> -type f -size +10M

# Find regular files smaller than 512 KiB; - means less than
find <search-path> -type f -size -512k

# Match entries satisfying both conditions; adjacent expressions imply AND
find <search-path> -name 'f*' -size 512k

# Match either condition; -o means OR and escaped parentheses preserve grouping
find <search-path> \( -name 'f*' -o -size 512k \)

# Exclude names beginning with f; -not negates the following expression
find <search-path> -not -name 'f*'

# Perform the same negation with an escaped ! so the shell does not interpret it
find <search-path> \! -name 'f*'

# Match entries with exactly permission mode 664
find <search-path> -perm 664

# Match entries with at least every permission bit in 664; the leading - means all bits
find <search-path> -perm -664

# Match entries with any permission bit from 664; the leading / means any bit
find <search-path> -perm /664

# Match exact mode 664 using symbolic notation instead of octal
find <search-path> -perm 'u=rw,g=rw,o=r'

# Find entries for which the "other read" bit is not set
find <search-path> \! -perm -o=r
```
