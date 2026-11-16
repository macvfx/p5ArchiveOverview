# P5 Archive Overview 2.13 (Build 30)

History can now answer a practical archive question: a folder was probably archived, but which P5
server handled it, and when?

## Search archived folders

- Enter any literal part of a folder name or path in History's **Search archived folders** box.
- Search covers all saved History, including records older than the normal 1,000-row display.
- The matching row shows its server and start/finish time.
- If one archive job contains several paths, the Directories column shows the path that matched.
- Server and status filters can narrow the search, and CSV export follows the visible results.
- A clear no-results state suggests trying part of the path or choosing All Servers.

## Safety boundary

Search is read-only and uses the app's local History database; it does not submit a job or change
anything on a P5 server. The red whole-server trash action still deletes all History for the
selected server, not only the matching rows, and continues to require confirmation.

Requires macOS 14 or later and an Archiware P5 server with REST API access.
