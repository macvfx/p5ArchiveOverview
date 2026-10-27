# P5 Archive Overview

A native macOS application for querying and managing Archiware P5 Archive server data with secure credential storage and historical tracking.

![P5 Archive Overview Set Up 3](https://github.com/user-attachments/assets/827f1d47-e9bf-4792-8356-197acba1adec)

![macOS](https://img.shields.io/badge/macOS-14.0+-blue) ![Swift](https://img.shields.io/badge/Swift-5.9-orange)

[Download the latest signed and notarized release](https://github.com/macvfx/p5ArchiveOverview/releases/latest)

## Requirements

- macOS 14.0 or later
- Archiware P5 server with REST API enabled

Nothing else. From 2.1 the app writes its CSVs itself and launches no subprocesses; `jq` is no
longer needed, and a Mac without Homebrew produces the same files as one with it.

## Quick Start

1. **Add a Server**
   - Click `Manage Servers` in the sidebar, then choose `Add`
   - Enter server alias, IP address, port (default: 8000), username, and password
   - Click "Add"

2. **Choose HTTP or HTTPS** (optional)
   - Each server has its own protocol setting. HTTP on port 8000 is the default and what P5 uses unless TLS was enabled
   - Choosing HTTPS moves the port to 8443, P5's TLS default; a port you typed yourself is left alone
   - P5 usually ships a self-signed certificate that macOS will not accept, so for HTTPS click **Check Certificate**, look at the fingerprint, and trust it once. A server that later presents a different certificate is refused
   - If an administrator installed a real certificate, nothing needs trusting — ordinary verification just works

3. **Run a Query**
   - Select a server from the sidebar
   - Click "Run Query"
   - View results in the table (top) and progress in the log (bottom)
   - Select any row and choose **Job Details** to read P5's job state, report or completion protocol
   - If the selected server is unavailable, click **Cancel Query** or press `Esc`, then select another server

4. **View History**
   - Toggle from "Current" to "History" in the toolbar
   - Filter by server, and by job status — All, Finished, Warning, Error, Cancelled or Running, each showing how many rows it would give
   - Export to CSV using the export button (↑). The export follows both filters, and the status appears in the filename
   - **Delete ignores the status filter** — it removes the selected server's whole history, not just what is on screen

5. **Try it without a server** (optional)
   - In **Manage Servers → Add**, turn on **Demo Mode** and pick a scenario
   - A demo server answers with made-up jobs and pool usage — no address, credentials or network connection needed

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

**Import Servers JSON** and **Export Servers JSON** are available in the **Manage Servers** sheet. Export saves the current list (passwords excluded) as `P5Servers.json`.

Duplicate handling:
- **Launch discovery** never changes the saved server list until you confirm **Add New Servers**.
- Existing connections are matched by IP address, port, and username; changing an alias does not make a server new.
- **Not Now** defers the prompt, while **Ignore This File Version** remembers the file's SHA-256 fingerprint.
- **Manual import** warns if duplicates are found, then lets you import only the new servers or cancel.

After importing, edit each new server once to enter its password (stored in Keychain).

## Features

- **Demo Mode** — flag a server as a demo server and pick a scenario (Healthy Fleet, Drive Needs Cleaning, Busy Archive Week, Index Discovery) to see the app work on made-up data, with no real server, credentials or network connection
- **Archive Jobs** — query and display recent archive jobs with plan, pool, status, size, client, and timing
- **Job Details** — correlate any selected overview row using actual job times, size, status and archive metadata, then show its fields and available report or completion protocol
- **Background detail cache** — after the overview appears, recent job states and likely protocols are read into a per-server in-memory cache without delaying the primary result
- **Plan and pool shown separately** — a plan is the configuration that ran, a pool is the media it wrote to; P5 returns both and they are not interchangeable
- **Path-first results** — the Directories column receives the flexible table space by default; manual column widths persist across launches
- **Pool Usage** — view storage pool utilization (used/total/free) per server
- **History** — SQLite-backed archive history persisted across sessions, filterable by server and by job status, with CSV export
- **One row per job** — a job seen while running is refreshed when it finishes, rather than stored a second time
- **Multi-Server** — shared server configuration format with Import/Export JSON support
- **Manage Servers workflow** — add, edit, duplicate, delete, import, and export server entries from one sheet without sidebar context menus
- **About window** — version display with sidebar header showing running version
- **Secure Credentials** — passwords stored in the macOS Keychain through the Security framework, never passed to a command line
- **Per-server HTTP or HTTPS** — with certificate pinning for P5's self-signed certificate; a server that changes certificate is refused, not silently accepted
- **Automatic Deduplication** — a job is identified by server, client and start time: the fields that do not change over its life
- **CSV Export** — export both current query results and historical data; numbers are written unquoted so size columns sum in a spreadsheet
- **Bounded and cancellable queries** — offline connections time out after eight seconds; active queries can be cancelled while the server list remains available

## Help menu

- **P5 Archive Overview User Guide** (`⌘?`) — the full guide, inside the app. `USER_GUIDE.md`
  carries the same material.
- **What's New…** — what changed in each version, rendered from the `RELEASES.md` bundled in the
  app, so it cannot drift from the notes in this repository.
- **About** — version and build.

## How it talks to P5

The normal query is one call, `GET /archive/overview`, made over `URLSession` through
[P5Kit](https://github.com/macvfx/P5Kit) — the shared Swift package these P5 apps use — with the
credential in an HTTP header, to whichever scheme and port each server is configured for.

**Job Details** performs read-only follow-up calls through P5Kit: `GET /general/jobs` with a
bounded `lastdays`, then `GET /general/jobs/{id}` and the closest candidates' completion protocols.
This begins in the background only after the overview has appeared and been saved. An active job
can fall back to its current `/report`. P5 does not put a job ID in `/archive/overview`, so the app
compares actual protocol start/end times, size, status, plan, client, pool and directories. It opens
only a uniquely strong, independently corroborated match; otherwise it shows scored candidates.
Both background and on-demand lookups write diagnostic counts and representative endpoint errors
to the Log. If no match is found, the Job Details sheet also shows the nearest rejected jobs and
their time distance, score and evidence.

P5Kit accepts both protocol-time forms found in P5: integer seconds since midnight and the
documented `HH:mm:ss` strings. Repeated references in P5's all-jobs response are deduplicated before
follow-up reads. A strong metadata-only candidate can be reviewed manually but never auto-selected.
P5's `Archive database backup(s)` maintenance jobs—including labels followed by a database name—are
excluded and identified in the diagnostic log rather than being suggested as content-archive matches.

Before 2.1 the same call was made by `curl` inside a `bash` script that carried the password in its
own text, and the keychain was read by running `/usr/bin/security`. Both put the password on a
process argument list, where `ps` could read it. The app now launches no subprocesses at all.

**P5 serves the same REST API over TLS on port 8443** — measured against a live server: TLS 1.3,
returning the identical entries and pools as port 8000. From 2.2 each server chooses. TLS is not
enabled on every installation, and the certificate P5 ships with is a self-signed placeholder that
macOS rejects, which is why HTTPS comes with a certificate check rather than being a port change.

How private a connection is depends as much on the network it runs over. Over a WireGuard or
Tailscale overlay it is already encrypted and the peer already authenticated, whatever scheme P5
itself is given — so HTTPS matters most on a plain LAN or someone else's network.

`RESTORE_FROM_OVERVIEW_DESIGN.md` covers what it would take to restore from this data, and what
would have to be proven against a live server first.

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
- **"This server's certificate is not one macOS trusts"**: normal for P5's own certificate. Edit the server, click **Check Certificate**, and trust it once
- **"The server presented a different certificate from the one you trusted"**: the certificate changed. A reinstall or renewal does that — check it again and re-trust. If nothing changed on the server, do not re-trust it
- **Check Certificate says the server did not answer**: TLS may not be enabled on that port. Not every P5 installation has it
- **Duplicate server import warning**: Manual import previews duplicates; launch discovery handles exact existing connections without adding them again
- **A history row shows an empty Plan**: the plan is filled in the next time that server is queried, provided P5 still reports the job. Jobs P5 has dropped from its overview keep an empty plan, as there is nothing left to read it from
- **A job says "In progress" under Finish Time**: P5 reports a running job with its finish time set to its start time, as a placeholder. The next query of that server replaces it with the real finish time
- **History shows fewer rows after upgrading to 2.1**: a job seen while running used to be stored a second time when it finished. The first launch of 2.1 collapses each job to one row, keeping the finished sighting
- **Job Details shows several candidates**: `/archive/overview` has no job ID. Choose the P5 job whose time, plan and description belong to the selected row; the app never guesses when the evidence is ambiguous
- **Job Details finds nothing**: read the selectable troubleshooting log in the sheet. It distinguishes no returned IDs, failed state/protocol reads, and jobs rejected as more than 24 hours away. The configured server may also differ from the one that produced an old History row
