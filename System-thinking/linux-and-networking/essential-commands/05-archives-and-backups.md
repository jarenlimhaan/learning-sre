# Archives and backups

## Archives and compression

```bash
# List archive contents without extracting; -t lists and -f selects the archive file
tar -tf <archive.tar>

# Create (-c) an uncompressed archive and name it with -f
tar -cf <archive.tar> <file-or-directory>...

# Append files to an existing uncompressed archive; -r means append and -f selects it
tar -rf <archive.tar> <file-or-directory>...

# Extract (-x) an archive selected by -f into the current directory
tar -xf <archive.tar>

# Extract into another directory; -C changes directory before extraction
tar -xf <archive.tar> -C <destination-directory>/

# Compress a file with gzip, replacing it with a .gz file
gzip <file>

# Compress a file with bzip2, replacing it with a .bz2 file
bzip2 <file>

# Compress a file with xz, replacing it with a .xz file
xz <file>

# Decompress a .gz file
gunzip <file.gz>

# Decompress a .bz2 file
bunzip2 <file.bz2>

# Decompress a .xz file
unxz <file.xz>

# Compress with gzip while retaining the input; --keep prevents removal of the original
gzip --keep <file>

# Compress with bzip2 while retaining the input
bzip2 --keep <file>

# Compress with xz while retaining the input
xz --keep <file>

# Create or update a ZIP archive containing the listed files
zip <archive.zip> <file>...

# Add an entire directory tree to a ZIP archive; -r means recursive
zip -r <archive.zip> <directory>/

# List ZIP contents without extracting; -l means list
unzip -l <archive.zip>

# Extract a ZIP archive into the current directory
unzip <archive.zip>

# Create (-c) a gzip-compressed (-z) tar archive named by -f
tar -czf <archive.tar.gz> <file-or-directory>...

# Create (-c) a bzip2-compressed (-j) tar archive named by -f
tar -cjf <archive.tar.bz2> <file-or-directory>...

# Create (-c) an xz-compressed (-J) tar archive named by -f
tar -cJf <archive.tar.xz> <file-or-directory>...

# Create an archive and choose compression from its suffix; -a means auto-compress
tar -caf <archive.tar.gz> <file-or-directory>...

# Extract a compressed archive; tar detects the compression format automatically
tar -xf <archive.tar.gz>
```

Always list an untrusted archive before extracting it, then extract it into a dedicated directory.

## Copy and back up data

```bash
# Synchronize locally in archive mode; -a preserves metadata and the trailing slash copies contents
rsync -a <source-directory>/ <destination-directory>/

# Push to a remote host; -a preserves metadata, -v is verbose, and --progress reports transfers
rsync -av --progress <source-directory>/ <user>@<host>:<destination-directory>/

# Pull from a remote host using the same archive, verbose, and progress options
rsync -av --progress <user>@<host>:<source-directory>/ <destination-directory>/

# Preview (--dry-run) a mirror where --delete would remove destination-only files
rsync -av --delete --dry-run <source-directory>/ <destination-directory>/

# Read from if= into of= using 1 MiB blocks and display progress; unmount the source first
sudo dd if=<source-device> of=<image-file> bs=1M status=progress

# Restore an image to a device; this destructively overwrites the entire of= destination
sudo dd if=<image-file> of=<destination-device> bs=1M status=progress
```

Double-check `dd`'s `if=` and especially `of=`. Writing to the wrong device can destroy the filesystem immediately.
