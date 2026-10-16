---
name: find
category: Utilities & Shell
description: Linux find recipes for recon, privilege escalation, hunting, forensics and incident response — Red and Blue Team.
tags: [find, linux, privesc, enumeration, forensics, blue-team, red-team, suid]
---

# find

`find` walks a directory tree and matches files by name, type, permission, owner, size, time and more, then optionally runs a command on each match. It's one of the most useful primitives in both offensive enumeration and defensive hunting, so it appears constantly in privilege-escalation scripts, forensic triage and incident response.

> Ships with coreutils/findutils on every Linux box. BSD/macOS `find` differs (see the Portability note at the end). `-exec` runs commands — read what you run before using it on a system you don't own.

## Syntax and evaluation

```bash
find [PATHS...] [EXPRESSION]
# EXPRESSION = options (-maxdepth), tests (-name, -type), actions (-print, -exec)
```

Tests are ANDed by default. Order matters: tests are evaluated left to right and short-circuit, so put cheap/narrowing tests first.

```bash
find / -type f -name "*.conf"           # implicit AND
find / -name "*.bak" -o -name "*.old"   # -o = OR
find / -type f ! -name "*.log"          # ! or -not negates
find / \( -name "*.key" -o -name "*.pem" \) -type f   # group with \( \) to control precedence
```

> The parentheses and `;` are escaped (`\(`, `\)`, `\;`) because the shell would otherwise eat them. `2>/dev/null` hides the flood of "Permission denied" errors when scanning `/` as a normal user.

## Common tests and actions

| Expression | Matches / does |
|-----------|----------------|
| `-name "*.sh"` / `-iname` | name glob, case-insensitive with `-i` |
| `-path "*/.ssh/*"` / `-ipath` | match against the whole path |
| `-regex` / `-iregex` | match full path by regex |
| `-type f,d,l,s,p,b,c` | file, dir, symlink, socket, pipe, block, char |
| `-perm -4000` / `-perm /022` | all of these bits / any of these bits |
| `-user root` / `-group shadow` | owner / group |
| `-uid 0` / `-nouser` / `-nogroup` | by uid / orphaned by uid or gid |
| `-size +100M` / `-empty` | larger than / zero bytes |
| `-mtime -1` / `-mmin -10` | modified < 1 day / < 10 min ago |
| `-newer FILE` / `-newermt "2026-01-01"` | changed after a file / a date |
| `-maxdepth N` / `-mindepth N` | limit recursion depth |
| `-print0` | NUL-separated output (pipe to `xargs -0`) |
| `-delete` | delete matches (dangerous — dry-run with `-print` first) |
| `-exec CMD {} \;` | run CMD once per file (`{}` = path) |
| `-exec CMD {} +` | batch many paths into one CMD invocation (faster) |

## Red Team — enumeration & privilege escalation

```bash
# SUID / SGID binaries — classic privesc hunt (cross-check against gtfobins.github.io)
find / -perm -4000 -type f 2>/dev/null                 # SUID
find / -perm -u=s -type f 2>/dev/null                  # same, symbolic
find / -perm -2000 -type f 2>/dev/null                 # SGID
find / -perm -4000 -o -perm -2000 -type f 2>/dev/null  # either

# World-writable files and directories (tampering / persistence)
find / -perm -0002 -type f ! -path "/proc/*" 2>/dev/null
find / -perm -0002 -type d 2>/dev/null
find / -writable -type d 2>/dev/null                   # dirs this user can write to

# Files owned by root but writable by us (privesc)
find / -user root -perm -0002 -type f 2>/dev/null

# Capabilities-like hunt via owner, plus hunt for config/secrets
find / -type f \( -name "*.conf" -o -name "*.config" -o -name "*.cnf" \) 2>/dev/null
find / -type f \( -name "id_rsa" -o -name "id_dsa" -o -name "*.pem" -o -name "*.key" \) 2>/dev/null
find / -type f -name "authorized_keys" 2>/dev/null
find /home -type f -name ".bash_history" 2>/dev/null

# Credentials inside files
find / -type f -name "*.php" -exec grep -l "password" {} \; 2>/dev/null
find / -type f -name "*.txt" -exec grep -iE "pass|pwd|secret|token" {} + 2>/dev/null

# The multi-name + decode pattern from the prompt
find / -type f \( -name "file1" -o -name "file2" -o -name "file3" \) -exec base64 -d {} \; 2>/dev/null

# Find text files of an exact byte size, not executable, then dump them (CTF / recon)
find . -type f -size 1033c ! -perm /a+x -exec sh -c 'file -b "$0" | grep -qi "text"' {} \; -print | xargs -d '\n' cat
find . -type f -size 1033c ! -perm /a+x -exec sh -c 'file -b "$0" | grep -qi "text"' {} \; -exec cat {} \;

# Recently modified files (what did we/they just touch?)
find / -type f -mmin -10 2>/dev/null
find /var/www -type f -mtime -1 2>/dev/null

# Writable cron, systemd and PATH dirs (scheduled-task / service abuse)
find /etc/cron* /etc/systemd -writable -type f 2>/dev/null
find / -name "*.timer" -o -name "*.service" 2>/dev/null

# Interesting files by extension across the box
find / -type f \( -name "*.sql" -o -name "*.db" -o -name "*.bak" -o -name "*.old" \) 2>/dev/null
```

> `-exec grep -l ... {} +` batches paths into fewer `grep` calls — far faster than `\;` on large trees. Always validate SUID finds against GTFOBins before assuming exploitability.

## Blue Team — hunting, triage & IR

```bash
# Unexpected SUID/SGID — baseline then diff over time
find / -perm -4000 -type f 2>/dev/null | sort > /root/suid-$(date +%F).txt
# later: diff against the baseline to spot new SUID binaries

# Files with no valid owner/group (left by deleted attacker accounts)
find / -nouser -o -nogroup 2>/dev/null

# Recently modified system binaries / libs (tampering, rootkits)
find /bin /sbin /usr/bin /usr/sbin /lib /usr/lib -mtime -2 -type f 2>/dev/null

# Hidden files and dirs in odd places (staging / persistence)
find / -name ".*" -type f ! -path "/home/*" ! -path "/root/*" 2>/dev/null
find /tmp /var/tmp /dev/shm -type f 2>/dev/null           # common drop zones
find /dev/shm /tmp -type f -executable 2>/dev/null        # executables in tmp = suspicious

# Files changed within an incident window (scope the blast radius)
find / -newermt "2026-10-14 08:00" ! -newermt "2026-10-14 09:00" -type f 2>/dev/null

# Webshell hunt
find /var/www -type f -name "*.php" -mtime -7 2>/dev/null
find /var/www -type f -name "*.php" -exec grep -lE "eval\(|base64_decode|system\(|passthru|shell_exec" {} + 2>/dev/null

# Large / suspicious files (exfil staging, dumps)
find / -type f -size +500M 2>/dev/null
find /tmp -type f -size +50M 2>/dev/null

# Orphaned SUID + recently modified (strong IR signal)
find / -perm -4000 -type f -mtime -30 2>/dev/null

# Dangling symlinks and unexpected sockets/pipes
find / -xtype l 2>/dev/null          # broken symlinks
find / -type s 2>/dev/null           # unix sockets
```

> On a live host, large `find /` scans cost I/O and update access times. Prefer scoping to the relevant subtree, and preserve atimes during forensics by mounting read-only or using `-noatime`.

## Performance & safety

```bash
find . -name "*.log" -print0 | xargs -0 rm        # NUL-safe handling of odd filenames
find . -type f -mtime +30 -print                  # DRY RUN first
find . -type f -mtime +30 -delete                 # only after confirming the list
find . -xdev -name "*.core"                        # -xdev / -mount: stay on one filesystem
find / -path /proc -prune -o -type f -print        # -prune: skip a whole subtree
```

> `-delete` and `-exec rm` are irreversible. Run the same expression with `-print` first and read the output. `-delete` implies depth-first and can act on directories too. Prefer `-xdev` when you don't want to cross into `/proc`, `/sys` or network mounts.

## Portability (GNU vs BSD/macOS)

| GNU (Linux) | BSD / macOS |
|-------------|-------------|
| `find . -regex` (whole path) | `find -E . -regex` |
| `-printf` supported | not supported — use `-exec stat` |
| `-newermt "DATE"` | use `-newer FILE` or `find ... -mtime` |
| `-perm /022` (any bit) | `-perm +022` |

> Related: [grep](#/tool/grep) for content search, [Linux File Operations](#/tool/linux-file-ops) for manipulation, and GTFOBins for turning SUID finds into shells. Stay in scope on anything you don't own.
