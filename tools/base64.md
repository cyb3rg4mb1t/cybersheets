---
name: base64
category: Utilities & Shell
description: Encode and decode Base64 — move binary through text channels, decode dumps, build and unpack payloads.
tags: [base64, encoding, decode, payload, exfiltration, data]
---

# base64

**base64** encodes binary as ASCII and decodes it back. It's everywhere in security work: config blobs, JWT parts, PowerShell `-EncodedCommand`, data smuggled through text-only channels, and CTF layers. It is **encoding, not encryption** — anyone can reverse it.

> coreutils on Linux (`base64`) · macOS/BSD also ships `base64` · GNU `basenc` adds base32/base16/base58

## Basic usage

```bash
echo -n "hello" | base64                 # aGVsbG8=
echo "aGVsbG8=" | base64 -d              # hello   (-d / --decode)
base64 file.bin > file.b64               # encode a file
base64 -d file.b64 > file.bin            # decode back to bytes
echo -n "data" | base64 -w0              # -w0: no line wrapping (one line)
```

> `-n` on `echo` matters — without it the trailing newline gets encoded too. By default GNU `base64` wraps output at 76 cols; use `-w0` when a tool needs it unbroken.

## Common options

| Option | Purpose |
|--------|---------|
| `-d` / `--decode` | decode instead of encode |
| `-w N` / `-w0` | wrap encoded output at N cols (`0` = never wrap) |
| `-i` | (decode) ignore non-alphabet characters |

## Recipes

```bash
# Decode a blob pulled from a file / response (writeup idiom)
base64 -d data.txt

# Decode the payload of a JWT (parts split on ".")
cut -d. -f2 token.jwt | base64 -d 2>/dev/null; echo

# Encode a script to paste through a text-only shell, then run it
base64 -w0 payload.sh                     # on attacker
echo <BLOB> | base64 -d | bash            # on target

# URL-safe variant (JWT, some APIs): +/ become -_ , padding stripped
echo -n "$DATA" | base64 -w0 | tr '+/' '-_' | tr -d '='      # encode url-safe
echo "$URLSAFE" | tr '_-' '/+' | base64 -d                    # decode url-safe

# Decode PowerShell -EncodedCommand (UTF-16LE)
echo "<BLOB>" | base64 -d | iconv -f UTF-16LE -t UTF-8
```

> Base64 hides nothing — treat decoded content as untrusted and read a payload before running it. For hex use [xxd](#/tool/xxd); for real crypto use [openssl](#/tool/openssl) or [gpg](#/tool/gpg). Stay in scope.
