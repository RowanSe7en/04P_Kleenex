<div align="center">

```text id="cph6di"
███╗   ███╗  █████╗   ██████╗ ██╗  ██████╗      ██████╗██╗     ███████╗ █████╗ ███╗   ██╗███████╗██████╗
████╗ ████║ ██╔══██╗ ██╔════╝ ██║ ██╔════╝     ██╔════╝██║     ██╔════╝██╔══██╗████╗  ██║██╔════╝██╔══██╗
██╔████╔██║ ███████║ ██║  ███╗██║ ██║          ██║     ██║     █████╗  ███████║██╔██╗ ██║█████╗  ██████╔╝
██║╚██╔╝██║ ██╔══██║ ██║   ██║██║ ██║          ██║     ██║     ██╔══╝  ██╔══██║██║╚██╗██║██╔══╝  ██╔══██╗
██║ ╚═╝ ██║ ██║  ██║ ╚██████╔╝██║ ╚██████╗     ╚██████╗███████╗███████╗██║  ██║██║ ╚████║███████╗██║  ██║
╚═╝     ╚═╝ ╚═╝  ╚═╝  ╚═════╝ ╚═╝  ╚═════╝      ╚═════╝╚══════╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝
```

[![Shell](https://img.shields.io/badge/Shell-Bash-121011?style=for-the-badge\&logo=gnubash\&logoColor=white)](https://www.gnu.org/software/bash/)
[![Platform](https://img.shields.io/badge/Platform-1337_Ubuntu%20%2F%20Linux-E95420?style=for-the-badge\&logo=ubuntu\&logoColor=white)](.)
[![Type](https://img.shields.io/badge/Type-Cleanup%20Utility-8A2BE2?style=for-the-badge)](.)
[![Status](https://img.shields.io/badge/Status-Finished-success?style=for-the-badge)](.)
[![Mode](https://img.shields.io/badge/Mode-Dry--Run%20Support-gold?style=for-the-badge)](.)

</div>

# Magic_Cleaner

Disk-space cleanup script for **1337/42 Ubuntu PCs**. Made by Bilal Rouane (42 intra: `brouane`).

It clears out caches, junk, and trash across your `$HOME`, with category-based cleaning and an optional full-app purge stage for browsers and chat/media apps.

## Clone & setup

```bash
git clone <repo_url>
cd <repo_folder>
chmod +x Magic_Cleaner
```

Show usage:

```bash
./Magic_Cleaner
```

*(running with no arguments shows the same thing as `--help`)*

Run the full default clean:

```bash
./Magic_Cleaner --default
```

## Options

| Flag | Short | Description |
|---|---|---|
| `--default` | `-d` | Run the full default clean: `general + python + vscode + chrome + purge` |
| `--help` | `-h` | Show usage |
| `--dry-run` | `-n` | Show what would be deleted, deletes nothing |
| `--print` | `-p` | Print each target as it's processed |
| `--analyze` | | Report-only: show biggest space consumers in `$HOME`. Deletes nothing. |
| `--no-purge` | | Run a clean but skip the purge stage specifically |
| `--purge-unused-apps` | | Run **only** the purge stage |
| `--only CATEGORY` | | Run a single category: `general`, `python`, `vscode`, `chrome`, `purge` |
| `--yes` | `-y` | Auto-confirm purge prompts (no `[y/N]` asked) |

Every flag above works **standalone** — none of them need `--default`/`-d` to do something. `-d` is only there if you want to be explicit about running the full default clean.

### Examples

```bash
./Magic_Cleaner                      # no args: shows usage/help, does nothing else
./Magic_Cleaner --default            # default: general + python + vscode + chrome + purge
./Magic_Cleaner --dry-run            # preview the default run, nothing deleted
./Magic_Cleaner --analyze            # report biggest space hogs, then exit — nothing deleted
./Magic_Cleaner --analyze -d         # report biggest space hogs, then run the default clean
./Magic_Cleaner --only vscode        # clean just VS Code
./Magic_Cleaner --only purge         # run just the purge stage
./Magic_Cleaner --purge-unused-apps --yes   # purge stage only, no prompts
./Magic_Cleaner --no-purge           # default run except purge
```

## Modes

- **No arguments** — shows the same usage/help text as `--help`/`-h` and exits. Nothing is cleaned.
- **`--default` / `-d`** — runs all five categories in order: `general → python → vscode → chrome → purge`.
- **`--only CATEGORY`** — runs exactly one category, skips the rest. Standalone, no `--default` needed.
- **`--purge-unused-apps`** — shortcut to run only the purge stage (same as `--only purge`). Standalone, no `--default` needed.
- **`--no-purge`** — runs a clean (the default five categories, or whatever `--only` picks) but skips the purge stage specifically. Standalone, no `--default` needed.
- **`--dry-run`** — runs the full selected scope but never calls `rm`; just reports what *would* be freed. Also auto-confirms any purge prompts (since nothing is actually deleted). Standalone — running `./Magic_Cleaner --dry-run` on its own previews the **default** clean.
- **`--print`** / **`--yes`** — same standalone rule: used alone with no other flag, each previews/runs the **default** clean (printing every target, or skipping purge confirmations, respectively).
- **`--analyze`** — **read-only**, and the only flag that does *not* fall back to the default clean on its own. Used by itself it dumps the largest items under `~/.var/app`, `~/.cache`, `~/.config`, `~/.local/share`, and top-level `$HOME`, sorted by size, then **exits immediately — nothing is ever deleted just because `--analyze` was passed**. Combine it with any other action (e.g. `--analyze -d`, `--analyze --only chrome`) to see the report first and then have that action run right after.
- **No flag does anything destructive on its own except by running a clean stage** — `--only`/`--purge-unused-apps` pick one stage; everything else other than `--analyze` falls back to the full default clean if nothing more specific was picked.

### Mixing flags

Flags combine the way you'd expect — each one tweaks *how* the run behaves, and you can stack as many as apply:

```bash
./Magic_Cleaner -d --no-purge -p          # default clean, skip purge, print every target
./Magic_Cleaner --only purge --dry-run    # preview only what purge would remove
./Magic_Cleaner --purge-unused-apps -y -n # preview purge with no prompts (dry-run already skips prompts)
./Magic_Cleaner --analyze --only vscode   # report space hogs, then clean only vscode
./Magic_Cleaner -d --no-purge --yes       # --yes here does nothing, since purge is skipped anyway
```

A few rules of thumb when mixing:
- `--only CATEGORY` and `--purge-unused-apps` both pick a single stage; you can't combine `--only` with `--purge-unused-apps` to pick two scopes — `--only` wins if both are present.
- `--no-purge` only matters when purge is actually part of the run (default clean, or `--only purge`/`--purge-unused-apps`, where it would skip the only stage requested). It's a no-op stacked with `--only vscode`, for example.
- `--yes` only matters for the purge stage's confirmation prompt — it's silently ignored if purge isn't running.
- `--dry-run` makes `--yes` moot for purge: dry-run already never prompts and never deletes, with or without `--yes`.
- `--analyze` stacked with anything else still runs that other thing afterward; it's only "report and stop" when it's the *only* flag given.

## What it deletes, by category

### `general`
- `~/.42*` and `~/.zcompdump*` (42 / shell leftovers)
- `~/.local/share/Trash/files/*` and `Trash/info/*`
- `~/.cache/*` (general cache, includes `~/.cache/thumbnails/*`)
- `~/.thumbnails/*` (old-style thumbnail cache)
- Desktop junk: stray `.DS_Store` files, `Piscine Rules *.mp4`, `PLAY_ME.webloc`
- Any file/dir in `/tmp` that you own

### `python`
Dev/package caches:
- `pip`, `pytest`, `mypy`, `ruff`, `uv` caches
- `npm`, `yarn`, `pnpm` caches
- `cargo` registry/git cache, `gem` cache
- Every `__pycache__` and `.pytest_cache` folder under `$HOME` — **except** any path inside a `.venv` (those are never touched)

### `vscode` (Flatpak install only — `~/.var/app/com.visualstudio.code`)
- Flatpak cache, Code Cache, CachedData, GPUCache, logs, Crashpad, Service Worker, Local/Session Storage, WebStorage, Dawn caches, Shared Dictionary
- `CachedExtensionVSIXs` (usually the biggest one)
- `User/History` and `User/workspaceStorage`
- **Keeps**: settings, keybindings, snippets, extensions themselves

### `chrome` (Flatpak install only — `~/.var/app/com.google.Chrome`)
- Flatpak cache, BrowserMetrics, `component_crx_cache`, optimization model store, `extensions_crx_cache`, ShaderCache/GrShaderCache, Crash Reports, segmentation platform, OnDeviceHeadSuggestModel
- Per-profile (every `Default` and `Profile*` folder): Service Worker, GPUCache, Dawn caches, Code Cache, Cache, Application Cache, File System, blob_storage
- Cache only — **bookmarks, history, passwords, logins are never touched**

### `purge` — what it actually is

`purge` is the odd one out among the five categories: every other category only deletes **caches and junk** — things an app regenerates on its own. `purge` instead **fully and permanently deletes an entire app's local data directory**, including things that are *not* regenerated: saved logins, profiles, settings, history, extensions, local message/chat data, downloaded files inside the app sandbox — everything that app has ever stored under its Flatpak data dir.

It targets exactly these four apps, each skipped automatically if not installed:
- Firefox (`~/.var/app/org.mozilla.firefox`)
- Brave (`~/.var/app/com.brave.Browser`)
- Discord (`~/.var/app/com.discordapp.Discord`)
- Spotify (`~/.var/app/com.spotify.Client`)

Because this is destructive in a way nothing else in the script is, every app gets its own interactive prompt before it's touched:

```
About to fully remove Firefox:
  /home/you/.var/app/org.mozilla.firefox
  Size: 1.2G
Continue? [y/N]
```

Flags that change how `purge` behaves:

| Flag | Effect on purge |
|---|---|
| *(default clean, no extra flags)* | Purge runs as the last stage, with a `[y/N]` prompt per app |
| `--no-purge` | Clean runs, but the purge stage is skipped entirely — no prompts, nothing purged |
| `--purge-unused-apps` | Skips general/python/vscode/chrome — runs **only** purge, still with prompts unless `--yes`/`--dry-run` |
| `--only purge` | Same effect as `--purge-unused-apps` |
| `--yes` / `-y` | Auto-confirms every purge prompt — no `[y/N]` asked, apps are wiped immediately |
| `--dry-run` / `-n` | Reports each app's size as if it would be purged, asks no prompt, deletes nothing |

> **Docker is never touched.** No Docker cleanup is performed by this script, in any mode.

## Safety notes

- Every deletion path is size-checked first; nothing with 0 bytes or a non-existent path is touched/counted.
- `--dry-run` walks the entire selected scope and reports sizes without calling `rm` anywhere, and never prompts for purge confirmation.
- The `purge` stage is the only destructive, no-undo, whole-app-wipe stage — it always asks for confirmation unless you pass `--yes` or `--dry-run`.
- `--analyze` never deletes anything; used alone it reports and exits before any cleaning stage would start.
- A summary table (found vs. freed, per category) is printed at the end of every run, along with available storage before and after, and — on real (non-dry-run) runs — the actual storage freed, measured as the disk-space delta before vs. after cleaning.
