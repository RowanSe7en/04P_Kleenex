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
./Magic_Cleaner --help
```

Run it:

```bash
./Magic_Cleaner
```

## Options

| Flag | Short | Description |
|---|---|---|
| `--help` | `-h` | Show usage |
| `--dry-run` | `-n` | Show what would be deleted, deletes nothing |
| `--print` | `-p` | Print each target as it's processed |
| `--analyze` | | Show biggest space consumers in `$HOME` before cleaning |
| `--no-purge` | | Skip the full-app purge stage |
| `--purge-unused-apps` | | Run **only** the purge stage |
| `--only CATEGORY` | | Run a single category: `general`, `python`, `vscode`, `chrome`, `purge` |
| `--yes` | `-y` | Auto-confirm purge prompts (no `[y/N]` asked) |

### Examples

```bash
./Magic_Cleaner                      # default: general + python + vscode + chrome + purge
./Magic_Cleaner --dry-run            # preview only, nothing deleted
./Magic_Cleaner --analyze            # show biggest space hogs first
./Magic_Cleaner --only vscode        # clean just VS Code
./Magic_Cleaner --only purge         # run just the purge stage
./Magic_Cleaner --purge-unused-apps --yes   # purge stage only, no prompts
./Magic_Cleaner --no-purge           # everything except purge
```

## Modes

- **Default run** — runs all five categories in order: `general → python → vscode → chrome → purge`.
- **`--only CATEGORY`** — runs exactly one category, skips the rest.
- **`--purge-unused-apps`** — shortcut to run only the purge stage (same as `--only purge`).
- **`--dry-run`** — runs the full selected scope but never calls `rm`; just reports what *would* be freed. Also auto-confirms any purge prompts (since nothing is actually deleted).
- **`--analyze`** — runs before cleaning starts. Dumps the largest items under `~/.var/app`, `~/.cache`, `~/.config`, `~/.local/share`, and top-level `$HOME`, sorted by size. Good for deciding what's worth purging.

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

### `purge` — full app removal (Flatpak data dirs), **with confirmation per app**
Fully deletes the entire Flatpak data directory for:
- Firefox (`org.mozilla.firefox`)
- Brave (`com.brave.Browser`)
- Discord (`com.discordapp.Discord`)
- Spotify (`com.spotify.Client`)

This wipes **all data** for that app (profiles, settings, saved logins, history — everything), not just caches. Each one is skipped automatically if not installed, and you get a `[y/N]` prompt with the size shown before anything is removed — unless `--yes` or `--dry-run` is set.

> **Docker is never touched.** No Docker cleanup is performed by this script.

## Safety notes

- Every deletion path is size-checked first; nothing with 0 bytes or a non-existent path is touched/counted.
- `--dry-run` walks the entire selected scope and reports sizes without calling `rm` anywhere.
- The `purge` stage is the only destructive, no-undo, whole-app-wipe stage — it always asks for confirmation unless you pass `--yes`.
- A summary table (found vs. freed, per category) and available storage before/after are printed at the end of every run.
