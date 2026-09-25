# Standard Publisher — update feed

This repository is the update feed for the Standard Publisher desktop app. It
contains **release metadata only** — no source code.

| File | What it is |
|---|---|
| `version.json` | The pointer the app fetches on every launch (~73 bytes). |
| `manifest.json` | Every shipped file with its SHA-256, and which release carries it. |

The files themselves are published as **release assets**: one zip per release
containing only what that release changed.

## How an update reaches a machine

1. `Start StandardPublisher.bat` runs `StandardPublisher.exe --update` before the
   app starts — the only moment the folder is not locked.
2. It fetches `version.json`. Same version as the local `version.txt` → it stops
   there and the app starts. That is the common case, and it costs one small request.
3. Otherwise it fetches `manifest.json`, compares SHA-256 against what is on disk,
   and downloads only the bundles carrying files that actually differ.
4. Every file is verified against the manifest hash **before** anything is
   installed. A failed or tampered download leaves the working version in place.

Config, logins and browser profiles live in `%LOCALAPPDATA%\StandardPublisher`
and are never touched by an update.
