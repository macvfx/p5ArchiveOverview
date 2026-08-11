# P5 Archive Overview

A native macOS application for querying and managing Archiware P5 Archive server data with secure credential storage and historical tracking. [Based on the script api-archive-overview.sh](https://github.com/macvfx/Archiware)
![P5 Archive Overview Set Up 3](https://github.com/user-attachments/assets/827f1d47-e9bf-4792-8356-197acba1adec)


![macOS](https://img.shields.io/badge/macOS-14.0+-blue) ![Swift](https://img.shields.io/badge/Swift-5.9-orange)

[Download the latest signed and notarized release](https://github.com/macvfx/p5ArchiveOverview/releases/latest)

## Requirements

- macOS 14.0 or later
- Archiware P5 server with REST API enabled
- CSV export during queries requires the `jq` command-line tool
  - macOS 14: install `jq` with Homebrew
    ```bash
    brew install jq
    ```
  - macOS 15 or later: `jq` is included with macOS; no separate installation is required

## Quick Start

1. **Add a Server**
   - Click `Manage Servers` in the sidebar, then choose `Add`
   - Enter server alias, IP address, port (default: 8000), username, and password
   - Click "Add"

2. **Run a Query**
   - Select a server from the sidebar
   - Click "Run Query"
   - View results in the table (top) and progress in the log (bottom)
   - If the selected server is unavailable, click **Cancel Query** or press `Esc`, then select another server

3. **View History**
   - Toggle from "Current" to "History" in the toolbar
   - Filter by server using the dropdown
   - Export to CSV using the export button (↑)

## File Locations

| File Type | Location |
|-----------|----------|
| Query Output (CSV/JSON) | `~/Documents/P5ArchiveOverview/[ServerAlias]_[timestamp]/` |
| SQLite Database | `~/Library/Application Support/P5ArchiveOverview/P5ArchiveHistory.sqlite` |
| History CSV Export | `~/Documents/P5ArchiveOverview/ArchiveHistory_[Server]_[timestamp].csv` |

## Server List Import & Export

Share server configurations with other P5 Archive apps using a common JSON file.

**Discovery and review at launch** — place `P5Servers.json` in either location and the app offers new server configurations for review on next launch:
- `/Users/Shared/P5Servers.json` — shared across all users
- `~/Documents/P5Servers.json` — current user only

**Import Servers JSON** and **Export Servers JSON** are available in the **Manage Servers** sheet. Export saves the current list without passwords.

- Launch discovery never changes saved servers until you confirm **Add New Servers**.
- Existing connections are matched by IP address, port, and username; changing an alias does not make a server new.
- **Not Now** defers the prompt; **Ignore This File Version** remembers that SHA-256 fingerprint.
- Manual import previews duplicates and lets you import only new entries or cancel.

Passwords remain local and are stored in macOS Keychain.

## Features

- **Archive Jobs**: Query recent jobs with pool, timing, status, size, client, and archive directories
- **Path-First Results**: Directories receives the flexible table space by default; manual column widths persist across launches
- **Bounded and Cancellable Queries**: Offline connections time out after eight seconds; active queries can be cancelled while the server list remains available
- **Pool Usage and History**: Review current pool use and SQLite-backed query history
- **Secure Credentials**: Passwords are stored in macOS Keychain
- **Automatic Deduplication**: Historical records are deduplicated by server, pool, start time, finish time, and client
- **CSV Export**: Export both current query results and historical data

## Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| Add Server | `Manage Servers` → `Add` |
| Run Query | Click "Run Query" |
| Cancel Query | `Esc` or click "Cancel Query" while running |
| Cancel Dialog | `Esc` |
| Save/Confirm | `Return` |

## Troubleshooting

- **"No password found"**: Edit the server and re-enter the password
- **Offline server**: Connection attempts stop after eight seconds. Use **Cancel Query** or `Esc` to stop sooner and select another server
- **Query times out after connecting**: Large connected queries have a five-minute ceiling; verify P5 responsiveness and try again
- **Query fails**: Check server IP, port, and credentials; ensure P5 REST API is enabled
- **CSV not created**: On macOS 14, install `jq` with `brew install jq`. macOS 15 or later already includes `jq`

## Changelog

### v2.0.2 (Build 10) — 2026-08-11

- **Path-first layout** — compact metadata columns are bounded so Directories receives the remaining width in Current and History.
- **Persistent sizing** — user-adjusted table column widths are restored on the next launch.
- **Offline timeout** — unreachable server connection attempts stop after eight seconds.
- **Cancel Query** — stop an active request from the toolbar or with `Esc`, then select another configured server.
- **Large-response allowance** — connected P5 overview requests retain a five-minute ceiling.

### v2.0.1 (Build 9) — 2026-08-10

- **Reviewed server discovery** — `P5Servers.json` is shown in a review sheet before any new connections are added.
- **Decision controls** — add the new servers, defer with **Not Now**, or ignore that exact file revision.
- **Stable identity** — matches host, port, and username without using the editable alias.
- **Revision memory** — accepted and ignored manifests are remembered by SHA-256 fingerprint, preventing repeated prompts and unwanted re-creation of deleted configurations.

### v2.0
### New Features

- **Manage Servers sheet** — new dedicated dialog for all server management: Add, Edit, Duplicate, Delete, Import JSON, and Export JSON; replaces scattered sidebar controls
- **Duplicate Server** — clone an existing server configuration including Keychain password in one click; the copy gets a unique alias suffix

### UI Changes

- **Cleaner sidebar** — now shows only the server list, a single Manage Servers button, and database stats
- **Add/Edit Server form** — IP Address and Port inputs now on separate rows for clearer spacing
- **Manual import warning** — importing JSON now warns before proceeding when duplicates are detected, then imports only new entries

### Import/Export Improvements

- **Duplicate detection rule** — a server is treated as duplicate when alias, IP address, and port all match an existing entry
- **Launch-time auto-import** skipped entries that already existed using the same alias + IP + port rule; v2.0.1 replaces automatic import with explicit review.

### v1.9

- **Sidebar header** now shows "P5 Archive Overview" and the current version number (read from app bundle) instead of plain "Servers" label

### v1.8

- Added **About window** — shows app name, version, build number, copyright, and link to code.matx.ca
- Added **Help menu item** — opens the About window from the Help menu

### v1.7

**Added**
- **Import Servers JSON** and **Export Servers JSON** buttons in the server sidebar.
- **Auto-detection of `P5Servers.json`** at launch from `/Users/Shared/` and `~/Documents/`.
  
## Known Issues

- *FIXED in 1.6* Sometimes jq will not be detected properly after installation
- *FIXED in 1.6* Issues reported on macOS 14 no csv created after 3rd party jq installed
- *FIXED in 1.6* Issues reported of csv created but jq still reporting as not installed
  
## License

Apache 2.0 License

## Acknowledgments

Built for use with [Archiware P5](https://www.archiware.com/)

## 2026 code.matx.ca - P5 Archive Tools for macOS & iOS
[For feedback, reach out via GitHub](https://github.com/macvfx) and [Support this project by optional donation](https://www.paypal.com/ncp/payment/ZX52VNS49SRZA)
