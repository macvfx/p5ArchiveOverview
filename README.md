# P5 Archive Overview

A native macOS application for querying and managing Archiware P5 Archive server data with secure credential storage and historical tracking. [Based on the script archive-overview-example.sh](https://github.com/macvfx/Archiware)

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
- **CSV not created**: Install `jq` with `brew install jq` in macOS 14
- **Note:** jq is included by default in macOS 15 (Sequoia) and later

## License

MIT License
