# P5 Archive Overview 2.16 (Build 33)

P5's archive overview has no job ID, so the first time you open **Job Details** on a row the app
has to find the job among the server's recent jobs, which can take a while. 2.16 remembers the
answer.

## Job Details opens at once the second time

- Once a finished job has been matched to a row, the app stores it. Open the same row again and
  the job appears immediately, with nothing fetched from P5. A line at the top says it is a
  remembered match.
- **Refresh from P5** re-reads just that one job if you want its current state.
- **Choose a Different Candidate** runs the full search again and remembers whichever job you pick.
- A job that is still running is not remembered; it is looked up each time until it finishes.

## Double-click a row

Double-click any row in Current or History to open Job Details. It is also in the right-click menu.

## Fewer repeated searches

When the app does have to search a server's jobs, it keeps what it read for the rest of the
session, so the next row on the same server does not start over. Before, every row repeated the
whole search.

## Good to know

- The first time you open a row is as slow as before; it is the repeat visits that are instant.
- Remembered matches are stored only in this app, with the protocol text P5 returned. Deleting a
  server's whole history deletes them; deleting selected rows does not.
- On a busy server, a row older than the newest 500 jobs can still fail to find its job.

Requires macOS 14 or later and an Archiware P5 server with REST API access.
