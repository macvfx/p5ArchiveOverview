# P5 Archive Overview 2.15 (Build 32)

P5 lists an archive submitted through its REST API with no pool and no directories: you can see
how much was archived and when, but not what. The matched P5 job still knows more, and 2.15 lets
you keep that with the row.

## Add to Overview

Select a row, choose **Job Details**, and use the new **Add to Overview** section under the
matched job. Each field has its own tick box and can be edited before saving.

| Field | Where it comes from | What to check |
|---|---|---|
| Directories | The job description | It may be a path or a project name; edit it, one per line |
| Pool | The volume label | P5 does not link a volume to its pool, so confirm the suggestion or pick from **Known Pools** |
| Volume | Label and barcode in the completion protocol | Shown in the new **Volume** column |

Choose **Save to Overview** and the row shows the details from then on, in Current and History.

## How saved details behave

- A small pencil marks anything you added, so it is never mistaken for what P5 reported.
- They are stored only in this app. Nothing is written to the P5 server.
- A field P5 already reports cannot be added, and if P5 later fills in a field you added, P5's
  value is shown.
- History's folder search finds a job by its saved directories.
- The History CSV export gains four columns at the end: Volume Label, Volume Barcode, P5 Job ID
  and Added From Job Details. The first nine columns are unchanged.
- **Remove Added Details** in the same sheet returns the row to what P5 reports.
- Deleting a server's whole history also deletes its saved details. Deleting selected rows does
  not, because P5 reports the job again on the next query.

## Also in this release

- Column widths and order in Current and History reset once, because of the new Volume column.
- Fixed: deleting history for a server whose name contains an apostrophe did nothing.
- The README and user guide now also cover Query All and scheduled queries from 2.14.

Requires macOS 14 or later and an Archiware P5 server with REST API access.
