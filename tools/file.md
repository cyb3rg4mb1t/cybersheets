---
name: file
category: Utilities & Shell
description: Identify a file's type by content (magic bytes), not by extension — triage unknown files and blobs.
tags: [file, magic-bytes, identification, forensics, triage, mime]
---

# file

**file** guesses what a file actually is by reading its content (magic numbers), ignoring the extension. First move when you pull an unknown blob out of a dump, a CTF, or a forensic image and need to know whether it's text, an archive, an executable or an image.

> ships with every Unix (`libmagic`) · `apt install file` · database in `/usr/share/misc/magic`

## Basic usage

```bash
file unknown.bin                 # unknown.bin: PNG image data, 800 x 600
file -b unknown.bin              # brief — type only, no filename prefix
file -i unknown.bin              # MIME type: image/png; charset=binary
file *                           # type every file in the dir
file -z archive.gz               # look inside compressed files
file -s /dev/sda1                # -s: read special/block files (partitions)
file -k unknown.bin              # keep going — show all matches, not just first
```

## Common options

| Option | Purpose |
|--------|---------|
| `-b` | brief: omit the filename, print only the type |
| `-i` / `--mime` | print MIME type + charset instead of prose |
| `-s` | read block/char special files (disks, partitions) |
| `-z` | try to look inside compressed files |
| `-k` | don't stop at first match — list all |
| `-L` | follow symlinks |
| `-f LIST` | read the list of filenames to test from LIST |

## Recipes

```bash
# Decompression chain: identify, rename, extract, repeat
file data                        # data: gzip compressed data
mv data data.gz && gunzip data.gz
file data                        # data: POSIX tar archive  -> tar xf data ...

# Classify every file in a tree, group by type
find . -type f -exec file {} + | sed 's/.*: //' | sort | uniq -c | sort -rn

# Keep only the text files in a dir (writeup idiom)
find . -type f -exec sh -c 'file -b "$0" | grep -q "text"' {} \; -print

# MIME sweep (useful for upload filters / web roots)
find /var/www -type f -exec file --mime-type {} + | grep -v "text/"
```

> `file` is a heuristic — a crafted file can lie about its type, and a correct magic match isn't proof of safe content. Confirm with [xxd](#/tool/xxd), [strings](#/tool/strings), [binwalk](#/tool/binwalk) or [exiftool](#/tool/exiftool). Stay in scope.
