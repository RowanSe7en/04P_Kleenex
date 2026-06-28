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

It clears out caches, junk, and trash across your `$HOME`, with category-based cleaning that's **safe by default** — the normal run only ever deletes regenerable cache/junk, never your browser logins, chat tokens, or history. A separate, explicitly opt-in `--extreme` mode exists for anyone who actually wants to wipe an app's entire profile data.

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
| `--default` | `-d` | Run the full default SAFE clean: `general + python + vscode + chrome + firefox + brave + discord + spotify` (caches only, plus misc caches under `general` — see below) |
| `--help` | `-h` | Show usage |
| `--dry-run` | `-dr` | Show what would be deleted, deletes nothing |
| `--print` | `-p` | Print each target as it's processed |
| `--analyze` | `-a` | Report-only: show biggest space consumers in `$HOME`. Deletes nothing. |
| `--only CATEGORY` | `-o CATEGORY` | Run a single category: `general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify` |
| `--extreme` | `-e` | **DANGEROUS.** Fully wipe one or more apps' entire profile data (history, passwords, sessions, saved logins — everything). Never runs as part of the default clean. See [`--extreme` mode](#--extreme-mode---e-what-it-actually-is) below. |
| `--firefox` | | With `--extreme`: target Firefox |
| `--brave` | | With `--extreme`: target Brave |
| `--discord` | | With `--extreme`: target Discord |
| `--spotify` | | With `--extreme`: target Spotify |
| `--yes` | `-y` | Auto-confirm extreme-wipe prompts (no `[y/N]` asked) |

Every flag above works **standalone** — none of them need `--default`/`-d` to do something. `-d` is only there if you want to be explicit about running the full default clean.

`--extreme`/`-e` also accepts the typo-tolerant aliases `--extreem`, `-extreme`, and `-extreem`, in case you fat-finger it — all four behave identically.

### Examples

```bash
./Magic_Cleaner                      # no args: shows usage/help, does nothing else
./Magic_Cleaner --default            # default: general + python + vscode + chrome + firefox/brave/discord/spotify caches
./Magic_Cleaner --dry-run            # preview the default run, nothing deleted
./Magic_Cleaner -dr                  # same as --dry-run
./Magic_Cleaner --analyze            # report biggest space hogs, then exit — nothing deleted
./Magic_Cleaner -a -d                # report biggest space hogs, then run the default clean
./Magic_Cleaner --only vscode        # clean just VS Code
./Magic_Cleaner -o firefox           # clean just Firefox's cache
./Magic_Cleaner --extreme --brave    # DANGEROUS: fully wipe Brave's profile data
./Magic_Cleaner -e --discord         # DANGEROUS: fully wipe Discord's profile data
./Magic_Cleaner --extreme --brave --discord   # wipe both Brave and Discord
./Magic_Cleaner --extreme --yes      # DANGEROUS: wipes ALL FOUR supported apps, no prompts
```

## Modes

- **No arguments** — shows the same usage/help text as `--help`/`-h` and exits. Nothing is cleaned.
- **`--default` / `-d`** — runs all eight categories in order: `general → python → vscode → chrome → firefox → brave → discord → spotify`. Every one of these is cache/junk-only; nothing destructive runs here.
- **`--only` / `-o` `CATEGORY`** — runs exactly one category, skips the rest. Standalone, no `--default` needed. Valid categories: `general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify`.
- **`--dry-run` / `-dr`** — runs the full selected scope but never calls `rm`; just reports what *would* be freed. Also auto-confirms any `--extreme` prompts (since nothing is actually deleted). Standalone — running `./Magic_Cleaner --dry-run` on its own previews the **default** clean.
- **`--print` / `-p`** / **`--yes` / `-y`** — same standalone rule: used alone with no other flag, each runs the **default** clean (printing every target, or auto-confirming extreme prompts if extreme also happens to be running, respectively).
- **`--analyze` / `-a`** — **read-only**, and the only flag that does *not* fall back to the default clean on its own. Used by itself it dumps the largest items under `~/.var/app`, `~/.cache`, `~/.config`, `~/.local/share`, and top-level `$HOME`, sorted by size, then **exits immediately — nothing is ever deleted just because `--analyze` was passed**. Combine it with any other action (e.g. `-a -d`, `--analyze --only chrome`) to see the report first and then have that action run right after.
- **`--extreme` / `-e`** — **the only destructive, whole-profile-wipe mode in the script.** Never runs as part of the default clean, and never runs at all unless you pass it explicitly. See the dedicated section below.
- **No flag does anything destructive except `--extreme`** — every category in the default clean (`general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify`) only ever removes caches and junk that the respective app or tool regenerates on its own.

### Mixing flags

Flags combine the way you'd expect — each one tweaks *how* the run behaves, and you can stack as many as apply:

```bash
./Magic_Cleaner -d -p                       # default clean, print every target
./Magic_Cleaner --only firefox --dry-run    # preview only what Firefox's cache cleanup would remove
./Magic_Cleaner --extreme --spotify -y -dr  # preview an extreme Spotify wipe with no prompts (dry-run already skips prompts)
./Magic_Cleaner --analyze --only vscode     # report space hogs, then clean only vscode
./Magic_Cleaner -d --extreme --brave        # run the full safe default clean, then ALSO extreme-wipe Brave
```

A few rules of thumb when mixing:
- `--only`/`-o` takes priority over everything else if present: if you pass `--only CATEGORY` together with `--extreme` (and any app flags), only that one safe category runs — `--extreme` is silently ignored in that case. To run an extreme wipe, don't combine it with `--only`.
- `--firefox` / `--brave` / `--discord` / `--spotify` **only make sense together with `--extreme`/`-e`**. Using any of them without `--extreme` is a hard error — the script prints `--firefox/--brave/--discord/--spotify require --extreme` and exits.
- `--extreme` on its own (no `--default`/`-d`) runs **only** the extreme wipe — nothing else gets touched, not even the safe categories.
- `--extreme` combined with `--default`/`-d` runs the **full safe default clean first**, then **also** runs the extreme wipe afterward. Both happen.
- If `--extreme` is given with **none** of `--firefox`/`--brave`/`--discord`/`--spotify`, it wipes **all four** supported apps — the script prints a warning (`No app flag given with --extreme: targeting all supported apps...`) and proceeds. Be careful with `--extreme --yes` on its own for exactly this reason.
- `--yes`/`-y` only matters for the extreme-wipe confirmation prompt — it's silently irrelevant if `--extreme` isn't running.
- `--dry-run`/`-dr` makes `--yes`/`-y` moot for extreme mode: dry-run already never prompts and never deletes, with or without `--yes`.
- `--analyze`/`-a` stacked with anything else still runs that other thing afterward; it's only "report and stop" when it's the *only* flag given.

## What it deletes, by category

Every single path-based target below is **safe to run even if it doesn't exist** on your machine — the script checks first and silently skips anything missing or empty. Nothing here errors out just because an app isn't installed.

### `general`
Cross-cutting junk, trash, and miscellaneous app caches:
- `~/.42*` and `~/.zcompdump*` (42 / shell leftovers)
- `~/.local/share/Trash/files/*` and `Trash/info/*`
- `~/.cache/*` (general cache, includes `~/.cache/thumbnails/*`)
- `~/.thumbnails/*` (old-style thumbnail cache)
- Desktop junk: stray `.DS_Store` files, `Piscine Rules *.mp4`, `PLAY_ME.webloc`
- **Obsidian** (`~/.config/obsidian`): `Cache`, `Code Cache`, `GPUCache`, `DawnGraphiteCache`, `DawnWebGPUCache`, `Shared Dictionary/cache` — your vault content is never touched
- **Prism Launcher** (`~/.local/share/PrismLauncher`): `cache/*`, `logs/*` — your instances are never touched
- **GNOME Extension Manager** (`~/.var/app/com.mattjakeman.ExtensionManager`): `cache/*`
- **Clipboard history** (`~/.cache/clipboard-history@alexsaveau.dev`): cleared entirely
- Every stray `*.log` and `LOG.old` file found anywhere under `$HOME`
- Any file/dir in `/tmp` that you own

### `python`
Dev/package caches:
- `pip`, `pytest`, `mypy`, `ruff`, `uv` caches
- `npm`, `yarn`, `pnpm` caches
- `cargo` registry/git cache, `gem` cache
- Every `__pycache__` and `.pytest_cache` folder under `$HOME` — **except** any path inside a `.venv` (those are never touched)
- Every stray `*.pyc` and `*.pyo` file found anywhere under `$HOME` — **except** inside a `.venv`

### `vscode` (Flatpak install only — `~/.var/app/com.visualstudio.code`)
- Flatpak cache, Code Cache, CachedData, **CachedProfilesData**, GPUCache, logs, Crashpad, Service Worker (and its `CacheStorage` subfolder specifically), Local/Session Storage, WebStorage, Dawn caches (`DawnWebGPUCache`/`DawnGraphiteCache`), `Shared Dictionary/cache`
- `CachedExtensionVSIXs` (usually the biggest one)
- `User/History` and `User/workspaceStorage`
- **Keeps**: settings, keybindings, snippets, extensions themselves

### `chrome` (Flatpak install only — `~/.var/app/com.google.Chrome`)
- Flatpak cache, `BrowserMetrics` (+ `BrowserMetrics-spare.pma`), `component_crx_cache`, `optimization_guide_model_store`, `extensions_crx_cache`, `ShaderCache`/`GrShaderCache`, `Crash Reports` (+ `CrashpadMetrics-active.pma`), `segmentation_platform`, `OnDeviceHeadSuggestModel`, `GraphiteDawnCache`, `GPUPersistentCache`, `Safe Browsing`, `Crashpad`, `WasmTtsEngine`, `CertificateRevocation`, `ZxcvbnData`, `ActorSafetyLists`, `OptimizationHints`, `PKIMetadata`, `Crowd Deny`, `SafetyTips`, `hyphen-data`
- Per-profile (every `Default` and `Profile*` folder): Service Worker (+ its `CacheStorage` and `ScriptCache` subfolders specifically), GPUCache, Dawn caches, Code Cache, Cache, Application Cache, File System, blob_storage, `Shared Dictionary/cache`
- Cache only — **bookmarks, history, passwords, logins are never touched**

### `firefox` (Flatpak install only — `~/.var/app/org.mozilla.firefox`)
- Flatpak-level cache
- Per-profile (every `*.default*` profile folder): `cache2`, `startupCache`, `shader-cache`, `thumbnails`, `OfflineCache`
- Cache only — **`places.sqlite` (history/bookmarks), `logins.json` (saved passwords), cookies, and `sessionstore` are never touched**

### `brave` (Flatpak install only — `~/.var/app/com.brave.Browser`)
- Flatpak-level cache
- `ShaderCache`, `GrShaderCache`, `Crash Reports`
- Per-profile (every `Default` and `Profile*` folder, Chromium-style): Service Worker, GPUCache, Dawn caches, Code Cache, Cache, Application Cache, File System, blob_storage
- Cache only — **`Login Data`, `History`, `Cookies`, and `Sessions` are never touched**

### `discord` (Flatpak install only — `~/.var/app/com.discordapp.Discord`)
- Flatpak-level cache
- `Cache`, `Code Cache`, `GPUCache`, `DawnWebGPUCache`, `DawnGraphiteCache`, `Service Worker`, `blob_storage`, `Crash Reports`
- Cache/render junk only — **your login token and account data are never touched**

### `spotify` (Flatpak install only — `~/.var/app/com.spotify.Client`)
- Flatpak-level cache
- `Cache`, `Code Cache`, `GPUCache`, `Service Worker`, `blob_storage`, `Browser/Cache`
- Cache/render junk only — **your saved credentials/local prefs are never touched**

## `--extreme` mode (`-e`): what it actually is

`--extreme` is the odd one out: every category above only ever deletes **caches and junk** that an app regenerates on its own. `--extreme` instead **fully and permanently deletes an entire app's local data directory**, including things that are *not* regenerated: saved logins, profiles, settings, history, extensions, local message/chat data, downloaded files inside the app sandbox — everything that app has ever stored under its Flatpak data dir.

It targets exactly these four apps, each skipped automatically if not installed:
- Firefox (`~/.var/app/org.mozilla.firefox`)
- Brave (`~/.var/app/com.brave.Browser`)
- Discord (`~/.var/app/com.discordapp.Discord`)
- Spotify (`~/.var/app/com.spotify.Client`)

`--extreme` **never** runs on its own — you must pass it explicitly, and it **never** runs as part of `--default`/`-d` unless you also add `--extreme` to that same command.

You pick which app(s) to wipe with `--firefox` / `--brave` / `--discord` / `--spotify` (combine as many as you like). If you pass `--extreme` with **none** of those app flags, it wipes **all four** apps — the script will warn you (`No app flag given with --extreme: targeting all supported apps...`) before doing it.

Because this is destructive in a way nothing else in the script is, every app gets its own interactive prompt before it's touched:

```
[EXTREME] About to fully wipe Brave (history, logins, sessions, everything):
  /home/you/.var/app/com.brave.Browser
  Size: 1.2G
Continue? [y/N]
```

Flags that change how `--extreme` behaves:

| Flag | Effect on `--extreme` |
|---|---|
| `--extreme` alone (no app flags, no `-d`) | Wipes **all four** supported apps, with a `[y/N]` prompt per app, and nothing else runs |
| `--extreme --firefox` / `--brave` / `--discord` / `--spotify` | Wipes only the app(s) named; combine multiple flags to wipe more than one |
| `--extreme -d` (or `--default`) | Runs the full safe default clean **and then** the extreme wipe afterward |
| `--yes` / `-y` | Auto-confirms every extreme prompt — no `[y/N]` asked, apps are wiped immediately |
| `--dry-run` / `-dr` | Reports each targeted app's size as if it would be wiped, asks no prompt, deletes nothing |
| `--firefox`/`--brave`/`--discord`/`--spotify` used **without** `--extreme` | **Error** — script prints `--firefox/--brave/--discord/--spotify require --extreme` and exits |

> **Docker is never touched.** No Docker cleanup is performed by this script, in any mode.

## Safety notes

- Every deletion path is size-checked first; nothing with 0 bytes or a non-existent path is touched/counted.
- The entire default clean (`general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify`) is cache/junk-only by design — no saved logins, passwords, browser history, or chat/account data is ever touched by `--default`/`-d`, or by any `--only CATEGORY` run.
- `--extreme`/`-e` is the **only** destructive, no-undo, whole-app-wipe mode in the script — it always asks for confirmation per app unless you pass `--yes` or `--dry-run`, and it never runs unless explicitly requested.
- `--dry-run`/`-dr` walks the entire selected scope and reports sizes without calling `rm` anywhere, and never prompts for an extreme-mode confirmation.
- `--analyze`/`-a` never deletes anything; used alone it reports and exits before any cleaning stage would start.
- A summary table (found vs. freed, per category — `general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify`, `extreme`) is printed at the end of every run, along with available storage before and after, and — on real (non-dry-run) runs — the actual storage freed, measured as the disk-space delta before vs. after cleaning.
