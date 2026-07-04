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

Clears out caches, junk, and trash across your `$HOME` with category-based cleaning that is **safe by default** — the normal run only ever deletes regenerable cache and junk, never your logins, history, cookies, passwords, sessions, or extension settings. A separate, explicitly opt-in `--wipe` mode exists for anyone who wants to fully wipe an app's entire profile data.

## Clone & setup

```bash
git clone https://github.com/RowanSe7en/004_PERSONAL_Magic_Cleaner
cd 004_PERSONAL_Magic_Cleaner
chmod +x Magic_Cleaner
```

Show usage:

```bash
./Magic_Cleaner
```

*(running with no arguments shows the same thing as `--help`)*

Run the full default safe clean:

```bash
./Magic_Cleaner --safe
```

## Options

| Flag | Short | Description |
|---|---|---|
| `--safe` | `-s` | Run the full default safe clean (see [what it cleans](#what--safe--s-cleans)) |
| `--safe-dry-run` | `-sdr` | Show what would be deleted, delete nothing |
| `--print` | `-p` | Print each target path before deleting it |
| `--analyze` | `-a` | Report-only: show biggest space consumers in `$HOME` plus disk usage. Deletes nothing. |
| `--only CATEGORY` | `-o CATEGORY` | Run exactly one category: `general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify`, `zen` |
| `--wipe` | `-w` | **DANGEROUS.** Fully wipe one or more apps' entire profile data. See [Wipe mode](#wipe-mode---wipe--w) below. |
| `--firefox` | | With `--wipe`: target Firefox |
| `--brave` | | With `--wipe`: target Brave |
| `--discord` | | With `--wipe`: target Discord |
| `--spotify` | | With `--wipe`: target Spotify |
| `--zen` | | With `--wipe`: target Zen Browser |
| `--chrome` | | With `--wipe`: target Chrome |
| `--vscode` | | With `--wipe`: target VS Code |
| `--yes` | `-y` | Auto-confirm wipe prompts (no `[y/N]` asked) |
| `--help` | `-h` | Show usage |

Every flag works **standalone** — none of them need `--safe`/`-s` to do something. `-s` is only there if you want to be explicit. Standalone flags that don't pick a specific scope fall back to the full default safe clean (same as `--safe`).

`--wipe`/`-w` also accepts the typo-tolerant aliases `--extreem`, `-wipe`, and `-extreem`.

### Examples

```bash
./Magic_Cleaner                        # no args: shows usage/help, does nothing
./Magic_Cleaner --safe                 # full safe clean
./Magic_Cleaner --safe-dry-run         # preview everything, delete nothing
./Magic_Cleaner -sdr                   # same as --safe-dry-run
./Magic_Cleaner --analyze              # report biggest space hogs, exit — nothing deleted
./Magic_Cleaner -a -s                  # report space hogs, then run the full safe clean
./Magic_Cleaner --only vscode          # clean only VS Code
./Magic_Cleaner -o zen                 # clean only Zen Browser's cache
./Magic_Cleaner --wipe --brave         # DANGEROUS: fully wipe Brave's entire profile
./Magic_Cleaner -w --discord           # DANGEROUS: fully wipe Discord's entire profile
./Magic_Cleaner --wipe --brave --discord --zen   # wipe all three
./Magic_Cleaner --wipe --chrome --vscode         # wipe Chrome and VS Code
./Magic_Cleaner --wipe --yes           # DANGEROUS: wipe ALL 7 supported apps, no prompts
```

## Modes

- **No arguments** — shows the same usage/help text as `--help`/`-h` and exits. Nothing is cleaned.
- **`--safe` / `-s`** — runs all nine categories in order: `general → python → vscode → chrome → firefox → brave → discord → spotify → zen`. Every category is cache/junk-only; nothing destructive runs here.
- **`--only` / `-o` `CATEGORY`** — runs exactly one category, skips the rest. Valid values: `general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify`, `zen`.
- **`--safe-dry-run` / `-sdr`** — runs the full selected scope but never calls `rm`; reports what *would* be freed. Also auto-skips wipe confirmation prompts (since nothing is actually deleted). Used alone it previews the **full default safe clean**.
- **`--print` / `-p`** — prints every target path as it's processed. Used alone it runs the **full default safe clean** with per-target output.
- **`--analyze` / `-a`** — **read-only**, the only flag that does *not* fall back to the default clean on its own. Used alone it reports the largest items under `~/.var/app`, `~/.cache`, `~/.config`, `~/.local/share`, and top-level `$HOME`, sorted by size, plus total/used/available disk space — then **exits immediately without deleting anything**. Combine with any other action (e.g. `-a -s`, `--analyze --only chrome`) to see the report first, then have that action run right after.
- **`--wipe` / `-w`** — the only destructive mode. Never runs as part of `--safe`. See [Wipe mode](#wipe-mode---wipe--w) below.

### Mixing flags

```bash
./Magic_Cleaner -s -p                          # safe clean, print every target
./Magic_Cleaner --only firefox --safe-dry-run  # preview Firefox cache cleanup only
./Magic_Cleaner -a --only chrome               # disk report, then clean Chrome only
./Magic_Cleaner -s --wipe --brave              # safe clean first, then wipe Brave
./Magic_Cleaner --wipe --spotify -sdr          # preview what a Spotify wipe would remove
```

Rules of thumb when mixing:

- **`--only` takes priority over everything.** If `--only CATEGORY` is present, only that one category runs — `--wipe` is silently ignored when combined with `--only`.
- **`--wipe` alone** (no `--safe`/`-s`) runs only the wipe — no safe categories are touched.
- **`--wipe` + `--safe`/`-s`** runs the full safe clean first, then the wipe afterward.
- **`--firefox`/`--brave`/`--discord`/`--spotify`/`--zen`/`--chrome`/`--vscode` require `--wipe`**. Using any of them without `--wipe` is a hard error: the script prints an error message and exits.
- **`--wipe` with no app flag** wipes all seven supported apps (prints a warning first). Be careful combining this with `--yes`.
- **`--yes`/`-y`** only matters for `--wipe` confirmation prompts. It's irrelevant if `--wipe` isn't running.
- **`--safe-dry-run`/`-sdr`** makes `--yes`/`-y` moot: dry-run never prompts and never deletes, with or without `--yes`.
- **`--analyze`/`-a`** combined with any other flag still runs that other flag; it only exits early when it's the *sole* flag given.

## What `--safe` / `-s` cleans

Every path is checked before anything is touched. Missing files and directories are silently skipped — no errors, no warnings.

### `general`

Cross-cutting junk, trash, and miscellaneous app caches. Runs first in every default clean.

- `~/.42*` and `~/.zcompdump*` — 42/shell leftovers
- `~/.local/share/Trash/files/*` and `Trash/info/*` — Trash
- `~/.cache/*` — general cache (includes `~/.cache/thumbnails/*`)
- `~/.thumbnails/*` — old-style thumbnail cache
- Desktop junk: stray `.DS_Store` files, `Piscine Rules *.mp4`, `PLAY_ME.webloc`
- `/tmp/*` — any file/dir in `/tmp` that you own
- Stray `*.log` and `LOG.old` files anywhere under `$HOME` — **except** inside `~/.config/*` and `~/.var/app/*/config/*`, because some databases (LevelDB, used by Chrome/Brave/Electron apps for extension storage and IndexedDB) name their live uncommitted data files `*.log`. Deleting those corrupts or resets the database. Real disposable log files (build output, app logs, project dirs) are almost always outside these config trees, so the exclusion is safe.
- **Obsidian** (`~/.config/obsidian`): `Cache`, `Code Cache`, `GPUCache`, `DawnGraphiteCache`, `DawnWebGPUCache`, `Shared Dictionary/cache` — vault content is never touched
- **Prism Launcher** (`~/.local/share/PrismLauncher`): `cache/*`, `logs/*` — instances are never touched
- **GNOME Extension Manager** (`~/.var/app/com.mattjakeman.ExtensionManager`): `cache/*`
- **Clipboard history** (`~/.cache/clipboard-history@alexsaveau.dev`): cleared entirely

### `python`

- `pip` cache (`~/.cache/pip`, `~/.local/share/pip-cache`)
- `pytest` cache (`~/.cache/pytest`)
- `mypy` cache (`~/.cache/mypy`)
- `ruff` cache (`~/.cache/ruff`)
- `uv` cache (`~/.cache/uv`)
- `npm` cache (`~/.npm/_cacache`)
- `yarn` cache (`~/.yarn/cache`)
- `pnpm` cache (`~/.cache/pnpm`, `~/.local/share/pnpm/store`)
- `cargo` registry and git cache (`~/.cargo/registry/cache`, `~/.cargo/git/db`)
- `gem` cache (`~/.gem/ruby/*/cache`)
- Every `__pycache__` directory under `$HOME` — **except** inside `.venv`
- Every `.pytest_cache` directory under `$HOME` — **except** inside `.venv`
- Every `*.pyc` and `*.pyo` file under `$HOME` — **except** inside `.venv`

### `vscode` (Flatpak — `~/.var/app/com.visualstudio.code`)

- Flatpak-level cache
- `Code/Cache`, `Code/CachedData`, `Code/CachedProfilesData`
- `Code/GPUCache`, `Code/DawnWebGPUCache`, `Code/DawnGraphiteCache`
- `Code/logs`, `Code/Crashpad`
- `Code/Service Worker/CacheStorage` and `Code/Service Worker/ScriptCache` — **only these two subdirs**, never the whole `Service Worker/` folder (`ServiceWorkerDatabase` inside it holds service worker registration metadata for extension background workers)
- `Code/Shared Dictionary/cache`
- `Code/CachedExtensionVSIXs` — usually the biggest item
- `Code/User/History`, `Code/User/workspaceStorage`
- **Never touched:** `Code/Local Storage/`, `Code/Session Storage/`, `Code/WebStorage/` (extension webviews can persist real settings there), user settings, keybindings, snippets, extensions themselves

### `chrome` (Flatpak — `~/.var/app/com.google.Chrome`)

Top-level:
- Flatpak-level cache
- `BrowserMetrics/`, `BrowserMetrics-spare.pma`
- `component_crx_cache/`, `optimization_guide_model_store/`
- `ShaderCache/`, `GrShaderCache/`
- `Crash Reports/`, `CrashpadMetrics-active.pma`
- `segmentation_platform/`, `OnDeviceHeadSuggestModel/`
- `GraphiteDawnCache/`, `GPUPersistentCache/`
- `Safe Browsing/`, `Crashpad/`, `WasmTtsEngine/`
- `CertificateRevocation/`, `ZxcvbnData/`, `ActorSafetyLists/`
- `OptimizationHints/`, `PKIMetadata/`, `Crowd Deny/`, `SafetyTips/`, `hyphen-data/`

Per-profile (`Default/` and every `Profile */`):
- `Service Worker/CacheStorage/` and `Service Worker/ScriptCache/` — **only these two subdirs** (`ServiceWorkerDatabase` is never touched)
- `GPUCache/`, `DawnWebGPUCache/`, `DawnGraphiteCache/`
- `Code Cache/`, `Cache/`, `Application Cache/`
- `blob_storage/`, `Shared Dictionary/cache/`
- **Never touched:** `File System/` (used by sandboxed Filesystem API), `Extension State/`, `Local Extension Settings/`, `Extension Scripts/`, history, bookmarks, cookies, passwords, login data, extensions themselves
- **`extensions_crx_cache/` is intentionally not cleaned** — it sits adjacent to extension install/update bookkeeping and was found to reset extension settings on some machines

### `firefox` (Flatpak — `~/.var/app/org.mozilla.firefox`)

- Flatpak-level cache
- Per-profile (`*.default*`): `cache2/`, `startupCache/`, `shader-cache/`, `thumbnails/`, `OfflineCache/`
- **Never touched:** `places.sqlite` (history/bookmarks), `logins.json` (saved passwords), cookies, sessionstore, preferences

### `brave` (Flatpak — `~/.var/app/com.brave.Browser`)

- Flatpak-level cache
- `ShaderCache/`, `GrShaderCache/`, `Crash Reports/`
- Per-profile (`Default/` and every `Profile */`):
  - `Service Worker/CacheStorage/` and `Service Worker/ScriptCache/` — only these two subdirs
  - `GPUCache/`, `DawnWebGPUCache/`, `DawnGraphiteCache/`
  - `Code Cache/`, `Cache/`, `Application Cache/`, `blob_storage/`
  - **`File System/` is intentionally not cleaned** (backs sandboxed Filesystem API, extensions/web apps can persist real data there)
- **Never touched:** Login Data, History, Cookies, Sessions, extension settings/storage

### `discord` (Flatpak — `~/.var/app/com.discordapp.Discord`)

- Flatpak-level cache
- `discord/Cache/`, `discord/Code Cache/`, `discord/GPUCache/`
- `discord/DawnWebGPUCache/`, `discord/DawnGraphiteCache/`
- `discord/Service Worker/CacheStorage/` and `discord/Service Worker/ScriptCache/` — only these two subdirs
- `discord/blob_storage/`, `discord/Crash Reports/`
- **Never touched:** login token, account data

### `spotify` (Flatpak — `~/.var/app/com.spotify.Client`)

- Flatpak-level cache
- `spotify/Cache/`, `spotify/Code Cache/`, `spotify/GPUCache/`
- `spotify/Service Worker/CacheStorage/` and `spotify/Service Worker/ScriptCache/` — only these two subdirs
- `spotify/blob_storage/`, `spotify/Browser/Cache/`
- **Never touched:** saved credentials, local preferences

### `zen` (Flatpak — `~/.var/app/app.zen_browser.zen`)

Zen Browser is Firefox-based (Gecko engine), so the layout mirrors Firefox exactly.

- Flatpak-level cache
- Per-profile (`*.default*` under `.zen/` or `zen/`): `cache2/`, `startupCache/`, `shader-cache/`, `thumbnails/`, `OfflineCache/`
- **Never touched:** `places.sqlite`, `logins.json`, cookies, sessionstore, `zen-themes.json`, preferences

## Wipe mode (`--wipe` / `-w`)

`--wipe` is the only destructive mode in the script. Unlike every safe category above (which only removes regenerable cache and junk), `--wipe` **permanently deletes an entire app's local Flatpak data directory**, including everything that is *not* regenerated: saved logins, history, settings, keybindings, snippets, installed extensions, bookmarks, cookies, sessions — everything the app has ever stored.

**`--wipe` never runs unless you pass it explicitly.** It never runs as part of `--safe`/`-s`. It never uninstalls the app itself — the app stays installed, only its data is gone.

### Supported apps for `--wipe`

| App flag | Directory wiped |
|---|---|
| `--firefox` | `~/.var/app/org.mozilla.firefox` |
| `--brave` | `~/.var/app/com.brave.Browser` |
| `--discord` | `~/.var/app/com.discordapp.Discord` |
| `--spotify` | `~/.var/app/com.spotify.Client` |
| `--zen` | `~/.var/app/app.zen_browser.zen` |
| `--chrome` | `~/.var/app/com.google.Chrome` |
| `--vscode` | `~/.var/app/com.visualstudio.code` |

Combine as many app flags as you like: `--wipe --brave --discord --zen`. If you pass `--wipe` with **no app flag**, it targets all seven apps — the script prints a warning before doing so.

Before wiping each app, the script shows its size and asks for confirmation:

```
[WIPE] About to fully wipe Brave (history, logins, sessions, everything):
  /home/you/.var/app/com.brave.Browser
  Size: 1.2G
Continue? [y/N]
```

### Flags that affect `--wipe`

| Flag | Effect |
|---|---|
| `--wipe` alone (no app flags, no `-s`) | Targets all 7 apps, `[y/N]` prompt per app, nothing else runs |
| `--wipe --firefox` (or any combo of app flags) | Targets only the named app(s) |
| `--wipe -s` (or `--safe`) | Runs the full safe clean first, then wipes the targeted app(s) |
| `--yes` / `-y` | Skips all `[y/N]` prompts — wipes immediately |
| `--safe-dry-run` / `-sdr` | Reports what would be wiped per app (size), asks no prompts, deletes nothing |
| any app flag without `--wipe` | **Error** — exits with a message |

## Safety guarantees

- **Extension data is never touched** by any safe category. `Service Worker/` is only ever cleaned via its `CacheStorage/` and `ScriptCache/` subdirs — never as a whole folder, because `ServiceWorkerDatabase` inside it holds MV3 extension background worker registrations. `File System/`, `Extension State/`, `Local Extension Settings/`, and `Extension Scripts/` are never touched anywhere in the script.
- **`*.log` exclusion inside config trees** — the stray-log sweep in `general` explicitly skips `~/.config/*` and `~/.var/app/*/config/*` because LevelDB (used by Chrome, Brave, Discord, Electron apps for IndexedDB and extension storage) names its live uncommitted data files `*.log`. Deleting them corrupts or silently resets those stores.
- **`.venv` exclusion** — `__pycache__`, `.pytest_cache`, `*.pyc`, and `*.pyo` cleanup never touches anything inside a `.venv` directory.
- **No app is ever uninstalled.** `--wipe` only deletes data under the Flatpak data directory. The app itself remains installed.
- **Docker is never touched** in any mode.
- Every deletion path is size-checked before anything is removed. Paths with 0 bytes or that don't exist are silently skipped.
- `--safe-dry-run`/`-sdr` walks the full selected scope and reports sizes without calling `rm` anywhere, and skips all wipe confirmation prompts.
- `--analyze`/`-a` never deletes anything — it only reads and reports.
- A per-category summary (found vs. freed for `general`, `python`, `vscode`, `chrome`, `firefox`, `brave`, `discord`, `spotify`, `zen`, and `wipe`) is printed at the end of every run, along with available storage before and after, and the actual disk-space delta on real (non-dry-run) runs.
