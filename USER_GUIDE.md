# P5 Archive Overview — User Guide

Asks an Archiware P5 server what it has archived, and keeps the answers.

This guide and the one inside the app (**Help → P5 Archive Overview User Guide**, or `⌘?`) cover
the same ground. Change one, change the other.

---

## Getting started

1. **Add a server** — Manage Servers → Add. Give it an alias, the P5 server's address, the port, a
   username and that user's password. The password is stored in your macOS Keychain and never
   leaves this Mac except as the login P5 itself asks for.
2. **Run a query** — select the server in the sidebar and click Run Query. One request goes to P5;
   results appear in the table and progress in the log below it.
3. **Read the results** — each row is one archive job: which plan ran, which media pool it wrote
   to, when it started and finished, how big it was, which client the data came from, and the
   directories it archived. Select any row and choose **Job Details** to read P5's detailed result.
4. **Keep them** — every query is saved. Switch Current to History in the toolbar to see everything
   this app has ever recorded for a server, including jobs P5 itself no longer lists.

---

## Demo Mode

Manage Servers → Add, then turn on Demo Mode and pick a scenario. A demo server answers with
made-up jobs and pool usage instead of talking to a real P5 server — no address, credentials or
network connection needed. Useful for a presentation or a screenshot; a demo server never leaves
this Mac and connects nowhere.

---

## HTTP or HTTPS

P5 answers the same requests two ways: in the clear on port 8000, and over TLS on port 8443. Which
one a server offers is decided when P5 is installed, so **each server here has its own Protocol
setting** rather than there being one switch for the whole app.

| | |
|---|---|
| **Leave it on HTTP** | What P5 uses unless TLS was deliberately enabled, and what every server added before 2.2 is set to. If you reach your servers over a VPN such as Tailscale or WireGuard, that connection is already encrypted before P5 sees it. |
| **Switching to HTTPS** | Moves the port to 8443. A port you typed yourself is left alone. The server has to actually offer TLS there — not all do. |
| **Check Certificate** | P5 ships a self-signed certificate macOS will not accept on its own, so HTTPS needs one decision from you. This shows the fingerprint and subject of the certificate the server really presents, and whether macOS trusts it. |
| **Trusting it once** | Records that exact certificate for that server. If the server later presents a different one the connection is refused rather than quietly accepted — which is the difference between this and turning checking off. |
| **A real certificate** | Needs nothing. Ordinary verification succeeds, nothing is pinned, and renewals keep working without asking again. |

---

## What a row means

- **Plan and Pool are different things.** A plan is the configuration that ran; a pool is the media
  it wrote to. A plan of `0` means P5 attributed the job to no plan at all, which is what an
  archive submitted straight through the REST API looks like.
- **An empty Pool** means the job never wrote to one. With a plan of `0` that is an unattributed
  job; with a real plan it is a job cancelled or failed before it wrote anything.
- **"In progress"** — P5 reports a running job with its finish time set to its start time, as a
  placeholder. Rather than show a job that appears to have finished instantly, the column says
  this until the next query picks up the real finish time.
- **Directories** — everything the job archived. The column shows the first path; hover for the
  rest. One job can carry dozens.
- **Job Details** — the overview status is only a summary. This action reads the matching P5 job
  and its completed protocol or current report, including information about affected files, drive
  or volume when P5 provides it.

### Reading job details

Select any row in Current or History and choose **Job Details**.

1. The app asks P5 for recent job IDs covering that row's date.
2. It reads candidate job states and completed-job protocols when available.
3. It prefers the completed protocol's actual start/end times, then checks archived size, status,
   plan, client, pool and directories. P5's scheduled time is only a lower-confidence fallback.
4. A uniquely strong match opens directly only when time and separate corroborating evidence agree.
   Otherwise the app shows scored candidates and their match reasons instead of guessing.
5. The detail sheet shows P5's job fields and completed-job protocol. For an active job without a
   protocol yet, it shows P5's current report instead. Every request is read-only.

The main Log reports how many IDs, state records and completion protocols P5 returned. If no match
is found, the failure sheet repeats that selectable diagnostic and lists up to five nearest jobs
that correlation rejected. Full report contents and credentials are never written to this log.

P5 can return protocol times as integer seconds or `HH:mm:ss`; both are accepted. Duplicate job
references are read once. If a server supplies no comparable time but a job strongly matches the
row's archive metadata, the app offers it for manual review and never selects it automatically.
Internal P5 jobs named **Archive database backup**, including labels followed by a database name,
are not content-archive candidates; their job IDs and exclusion reason appear in the troubleshooting log.

After a successful overview query, this enrichment starts in the background. A small spinner shows
while it runs, but the overview itself is already available. Results are cached in memory for that
server and replaced on the next query or when its connection settings change.

P5 does not include a job ID in `/archive/overview`. Matching is therefore evidence-based, not a
guaranteed database join. A job too old for P5's retained job list may no longer be available
through the API even though its overview row remains in this app's History.

---

## History

History is **one row per job**, not one row per time it was seen. A job is identified by its
server, its client and its start time — the things that do not change while it runs — so a job
recorded while running is updated when it finishes rather than stored again.

- **Filter by server, and by status** — All, Finished, Warning, Error, Cancelled or Running, each showing
  how many rows it would give, counted within whichever server is selected.
- **Both filters follow the export.** The CSV contains what you are looking at, and the status
  appears in the filename so an errors-only file cannot later be mistaken for a full one.
- **Delete ignores the status filter.** It removes the selected server's whole history. Deleting
  only what happens to be on screen is not what a trash button beside a filter should mean.
- **What a refresh changes** — a job already in history has its status, finish time and size
  updated by a later query. Plan, pool and directories are only filled in when they were empty, so
  a short or interrupted response cannot blank something P5 already answered.

---

## Files it writes

| What | Where |
|---|---|
| Query output | `~/Documents/P5ArchiveOverview/<server>_<timestamp>/` — P5's raw JSON exactly as sent, plus an Archive Overview CSV and a Pool Usage CSV |
| History database | `~/Library/Application Support/P5ArchiveOverview/` |
| History CSV export | `~/Documents/P5ArchiveOverview/` |

Sizes are written unquoted so a spreadsheet adds them up rather than treating them as text.

---

## Sharing a server list

Manage Servers can export the list as `P5Servers.json` and import one, and the same format is
shared with the other P5 apps. Put a copy in `/Users/Shared` or your Documents folder and this app
offers the new connections for review at launch.

- **Passwords are never included.** Add one for each imported server by editing it.
- **Nor are trusted certificates.** The protocol travels with the list; a certificate you trusted
  does not. That is a decision made on one machine about one server, and inheriting someone else's
  silently would defeat the point of making it.

---

## When something goes wrong

| Message | What it means |
|---|---|
| The server did not answer within 8 seconds | Nothing is listening at that address and port. A healthy server that is simply slow is not cut off — once connected, a query has five minutes |
| This server's certificate is not one macOS trusts | Normal for P5's own certificate. Edit the server, click Check Certificate, trust it once |
| The server presented a different certificate | It changed. A reinstall or renewal does that. If nothing changed on the server, do not trust the new one |
| No password found | Edit the server and enter it again |
| An empty Plan on an old row | Rows stored before 2.1 never read that field. Querying the server again fills it in, for jobs P5 still reports |
| Job Details shows several candidates | P5 gives the overview no job ID. Choose the candidate whose time, plan and description belong to the row |
| Job Details finds nothing | Read the troubleshooting log in the sheet: it shows whether P5 listed no IDs, state/protocol reads failed, or returned jobs were more than 24 hours away. The old overview row is still preserved locally |

---

## Help menu

- **P5 Archive Overview User Guide** (`⌘?`) — this guide, inside the app
- **What's New…** — what changed in each version
- **About** — version and build
