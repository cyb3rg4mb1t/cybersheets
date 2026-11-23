---
name: grep
category: Utilities & Shell
description: Search text with patterns and regex — the default tool for hunting strings, secrets and matches in files and output.
tags: [grep, regex, search, text, secrets, pentest, blue-team]
---

# grep

**grep** prints lines matching a pattern. It's the backbone of text hunting — finding a string in a file, filtering command output in a pipe, or sweeping a tree for credentials. `egrep`/`fgrep` are deprecated aliases for `grep -E`/`grep -F`.

> ships with every system (GNU grep on Linux, BSD grep on macOS) · [gnu.org/software/grep](https://www.gnu.org/software/grep/)

## Basic usage

```bash
grep "pattern" file.txt           # lines containing pattern
grep "pattern" *.log              # across many files (prints filename)
command | grep "pattern"          # filter a pipe
grep -i "error" file              # case-insensitive
grep -w "root" /etc/passwd        # whole word only (not "chroot")
grep -x "exact line" file         # whole line must match
```

## Core options

| Option | Purpose |
|--------|---------|
| `-i` | case-insensitive |
| `-v` | invert — print lines that do NOT match |
| `-c` | count matching lines |
| `-n` | prefix line number |
| `-l` / `-L` | list filenames with / without a match |
| `-o` | print only the matched part, one per line |
| `-r` / `-R` | recurse into directories (`-R` follows symlinks) |
| `-w` / `-x` | match whole word / whole line |
| `-q` | quiet — no output, exit 0 on first match (for scripts) |
| `-a` | treat binary files as text |
| `-E` / `-P` | extended regex / Perl regex (PCRE) |
| `-F` | fixed strings, no regex (fast, literal) |
| `-A N` / `-B N` / `-C N` | N lines After / Before / Context around match |
| `--include=GLOB` / `--exclude=GLOB` | limit recursion by filename |
| `--exclude-dir=GLOB` | skip directories (e.g. `.git`, `node_modules`) |

## Regex quick reference

```bash
grep -E "cat|dog" file                 # alternation
grep -E "colou?r" file                 # optional char
grep -E "[0-9]{1,3}(\.[0-9]{1,3}){3}" f # IPv4-ish
grep -P "(?<=user=)\w+" file           # PCRE lookbehind (GNU -P only)
grep -oE "[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}" file   # extract emails
```

> Basic regex (`grep` default) needs `\(` `\{` for grouping/quantifiers; `-E` makes them literal. Prefer `-E` or `-P` to stay sane. `-F` when the pattern has regex metacharacters you want taken literally.

## Recipes — hunting & filtering

```bash
# Secrets sweep across a source tree (skip noise)
grep -rInE "password|passwd|secret|api[_-]?key|token" . \
  --exclude-dir={.git,node_modules} 2>/dev/null

# Private keys and AWS creds in a dump
grep -rE "BEGIN (RSA|OPENSSH) PRIVATE KEY|AKIA[0-9A-Z]{16}" .

# Only the matching tokens, not whole lines
ss -tlnp | grep -oE ':[0-9]+' | sort -u          # listening ports

# Use -q in a test / inside find -exec
find . -type f -exec sh -c 'file -b "$0" | grep -q "text"' {} \; -print
grep -q "Debian" /etc/os-release && echo "debian-like"

# Filter out the noise line in a brute/scan loop
./bruteforce | grep -v "Wrong!"

# Context around a hit (logs / configs)
grep -n -C2 "Failed password" /var/log/auth.log
```

> `grep` returns exit 0 if it matched, 1 if not, 2 on error — scripts branch on that. Pair with [find](#/tool/find) for tree sweeps, [strings](#/tool/strings) for binaries, [regex-ascii](#/tool/regex-ascii) for the pattern syntax. Stay in scope.
