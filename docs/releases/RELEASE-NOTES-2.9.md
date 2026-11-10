# P5 Archive Overview 2.9 (Build 23)

P5 Archive Overview can now open read-only details for an overview row, including jobs that finished, warned, errored, or were cancelled.

## Job Details

- Select a row and choose **Job Details** to view the matched P5 job state, completion protocol, current report, and correlation evidence.
- A background cache fetches recent P5 job states and protocols with the overview, then performs a focused live lookup when needed.
- Matching corroborates completion times, size, status, plan, client, pool, and archived directories instead of relying on a timestamp alone.
- Ambiguous results remain manual choices; the app never changes the P5 server.

## Reliability and diagnostics

- Supports the `HH:mm:ss` start and finish values returned by P5 completion protocols.
- Removes duplicate job IDs before requesting state and protocol records.
- Provides a copyable troubleshooting log with request counts, endpoint failures, nearest candidates, time distance, score, and matching evidence.
- Excludes P5 internal `Archive Database Backup` maintenance jobs, including colon-suffixed database names, from archive-content candidates.

## Safety boundary

Job Details uses read-only P5 REST requests. It does not start, cancel, retry, delete, or modify jobs, plans, pools, or archive data.

Requires macOS 14 or later and an Archiware P5 server with REST API access.
