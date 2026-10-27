# P5 Archive Overview 2.12 (Build 27)

Two additions to how History and the server list are managed.

## History row/range deletion

- Select a single row, a shift-click range, or a scattered cmd-click selection, then delete just those — by Delete/Backspace with the table focused, right-click, or a toolbar trash button that only appears while something is selected.
- The existing red trash button (delete everything for the current server) is unchanged.

## Sidebar server right-click

- Right-click a server in the sidebar for **Edit…**, which opens Manage Servers with that server already selected.
- **Delete…** asks whether to also delete that server's saved history — a plain server delete has always left history behind, since it's kept by server alias rather than the server's own id.

## Safety boundary

Both deletions ask for confirmation first and only ever remove what you explicitly selected.

Requires macOS 14 or later and an Archiware P5 server with REST API access (not required for a demo server).
