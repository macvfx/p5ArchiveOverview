# P5 Archive Overview

A native macOS application for querying and managing Archiware P5 Archive server data with secure credential storage and historical tracking. [Based on the script api-archive-overview.sh](https://github.com/macvfx/Archiware)
![P5 Archive Overview Set Up 3](https://github.com/user-attachments/assets/827f1d47-e9bf-4792-8356-197acba1adec)


![macOS](https://img.shields.io/badge/macOS-14.0+-blue) ![Swift](https://img.shields.io/badge/Swift-5.9-orange)

## Requirements

- macOS 14.0 or later
- Archiware P5 server with REST API enabled
- `jq` command-line tool (optional, for CSV export during queries)
- **Note:** jq is included by default in macOS 15 (Sequoia) and later

## Quick Start

1. **Add a Server**
   - Click the `+` button in the sidebar
   - Enter server alias, IP address, port (default: 8000), username, and password
   - Click "Add"

2. **Run a Query**
   - Select a server from the sidebar
   - Click "Run Query"
   - View results in the table (top) and progress in the log (bottom)

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

## Features

- **Secure Credentials**: Passwords stored in macOS Keychain
- **Automatic Deduplication**: Historical records are deduplicated by server, pool, start time, finish time, and client
- **CSV Export**: Export both current query results and historical data

## Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| Add Server | Click `+` |
| Run Query | Click "Run Query" |
| Cancel Dialog | `Esc` |
| Save/Confirm | `Return` |

## Troubleshooting

- **"No password found"**: Edit the server and re-enter the password
- **Query fails**: Check server IP, port, and credentials; ensure P5 REST API is enabled
- **CSV not created**: Install `jq` with `brew install jq` in macOS 14 or earlier
- **Note:** jq is included by default in macOS 15 (Sequoia) and later

## Changelog

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
- **Launch-time auto-import** now skips entries that already exist using the same alias + IP + port rule

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
