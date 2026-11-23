---
name: diff
category: Utilities & Shell
description: Compare two files or streams line by line — spot the changed credential, config drift or tampered file.
tags: [diff, compare, files, config, baseline, blue-team]
---

# diff

**diff** reports the lines that differ between two files. In security work it isolates the one changed value in a pair of dumps, detects config drift from a known-good baseline, and shows what an attacker touched. `comm` and `cmp` are close cousins.

> coreutils/diffutils on every Unix · `diff OLD NEW` reads left-to-right: `<` = only in the first file, `>` = only in the second

## Basic usage

```bash
diff a.txt b.txt                 # default (ed-style) output
diff -u a.txt b.txt              # unified diff (the familiar +/- patch format)
diff -y a.txt b.txt              # side-by-side columns
diff -r dir1/ dir2/              # recurse directories
diff -q a.txt b.txt              # quiet — just say if they differ
```

## Reading the output

```
42c42                    # line 42 Changed
< OQxXZjELndr90zuhOTDYBEomI0SZITXI    # '<' = the FIRST file (a.txt)
---
> z7vPcsOPfFxnloUxixqYv4aWHUrx49K6    # '>' = the SECOND file (b.txt)
```

> The side matters: `diff new old` puts the new value on the `<` side. Pick the argument order deliberately so you know which line is which. `a` = added, `d` = deleted, `c` = changed.

## Common options

| Option | Purpose |
|--------|---------|
| `-u` | unified format (patch-style, `+`/`-`) |
| `-y` | side-by-side |
| `-r` | recurse into directories |
| `-q` | report only whether files differ |
| `-i` | ignore case |
| `-w` / `-b` | ignore all / trailing whitespace |
| `--suppress-common-lines` | with `-y`, show only differences |

## Recipes

```bash
# Find the single changed credential between two password dumps (writeup idiom)
diff passwords.new passwords.old        # the '<' line is the new password

# Compare command output without temp files (process substitution)
diff <(ssh host1 'rpm -qa' | sort) <(ssh host2 'rpm -qa' | sort)
diff <(sort a.txt) <(sort b.txt)        # ignore ordering

# Config drift from a known-good baseline
diff -u /etc/ssh/sshd_config /root/baseline/sshd_config

# Only lines unique to one side (set difference)
comm -23 <(sort a.txt) <(sort b.txt)    # in a.txt but not b.txt
```

> For line sets prefer `comm` (needs sorted input); `sort file | uniq -u` finds the lone unique line in a single file. Byte-level comparison is `cmp`. Related: [grep](#/tool/grep), [git](#/tool/git). Stay in scope.
