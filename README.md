# P5 Archive Overview

A native macOS application for querying and managing Archiware P5 Archive server data with secure credential storage and historical tracking. [Based on the script api-archive-overview.sh](https://github.com/macvfx/Archiware)
![P5 Archive Overview Set Up 3](https://github.com/user-attachments/assets/827f1d47-e9bf-4792-8356-197acba1adec)


![macOS](https://img.shields.io/badge/macOS-14.0+-blue) ![Swift](https://img.shields.io/badge/Swift-5.9-orange)

[Download the latest signed and notarized release](https://github.com/macvfx/p5ArchiveOverview/releases/latest)

## Requirements

- macOS 14.0 or later
- Archiware P5 server with REST API enabled

Nothing else. Since 2.1 the app writes its CSVs itself and launches no subprocesses, so `jq` is no
longer required.

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

### Per-server HTTP or HTTPS
P5 serves the same REST API in the clear on port 8000 and over TLS on port 8443. Each server has
its own Protocol setting, since TLS is enabled per installation.

- **Check Certificate** shows the fingerprint and subject of the certificate the server presents,
  and whether macOS trusts it. P5 ships a self-signed certificate macOS will not accept on its own,
  so trusting it once records that exact certificate
- A server that later presents a different certificate is refused, not quietly accepted

### One row per job
A job is identified by its server, its client and its start time — the things that do not change
while it runs — so a job recorded while running is refreshed when it finishes rather than stored a
second time.

### In-app guide
Help → P5 Archive Overview User Guide (⌘?), and What's New for each version's changes.

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
- **A history row shows an empty Plan**: the plan is filled in the next time that server is queried, provided P5 still reports the job
- **A job says "In progress" under Finish Time**: P5 reports a running job with its finish time set to its start time, as a placeholder
- **A server's certificate is refused**: P5 ships a self-signed certificate macOS will not accept. Edit the server, click **Check Certificate**, and trust it once

## Changelog

### v2.2 (Build 16) — 2026-09-18
- Each server chooses HTTP or HTTPS, with certificate checking and pinning for the self-signed certificate P5 ships
- A real in-app user guide and What's New, replacing a Help menu that opened the About window
- A refused certificate no longer reports itself as "cancelled"

### v2.1 (Build 14) — 2026-09-17
- One row per job: a job seen while running is refreshed when it finishes, rather than stored twice
- Status filter in History — All, Finished, Error, Cancelled or Running, with counts
- Plan column, separate from Pool. P5 reports both and earlier versions showed only the pool
- No P5 password reaches a process's arguments; requests and Keychain access happen in-process
- `jq` is no longer required — the CSVs are written by the app
- Sizes in exported CSVs are unquoted, so a spreadsheet adds them up

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

- *Resolved in 2.1* Every issue below concerned detecting `jq`, which the app no longer uses —
  the CSVs are written by the app itself.
  
## License

Apache 2.0 License

## Acknowledgments

Built for use with [Archiware P5](https://www.archiware.com/)

## 2026 code.matx.ca - P5 Archive Tools for macOS & iOS
[For feedback, reach out via GitHub](https://github.com/macvfx) and [Support this project by optional donation](https://www.paypal.com/ncp/payment/ZX52VNS49SRZA)
