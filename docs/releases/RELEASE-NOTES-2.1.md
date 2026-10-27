# P5 Archive Overview 2.1 (Build 14)

Released 2026-09-17.

Security, one row per job, and the CSVs no longer need anything installed.

## No P5 password reaches a process's arguments

Every query used to run as a shell script with the password written into the script's own text,
which then ran `curl` with the password on its command line. Saving and reading passwords went the
same way. All of those put the credential on an argument list that any other user of this Mac
could read while it ran.

Requests now go over the network in-process and the Keychain is read and written through macOS's
own interface. **The app launches no subprocesses at all.** Saved passwords are found exactly where
they were and nothing needs re-entering.

## One row per job

A job seen while running used to be stored a second time when it finished, because P5 reports a
running job with its finish time set to its start time — a placeholder, not a finish, and part of
what identified the row. History is now keyed on the server, client and start time, which do not
change while a job runs.

- Existing history is collapsed to one row per job the first time this version starts, keeping the
  finished sighting.
- A job already in history is refreshed, so its status, finish time and size follow what P5 now
  reports.
- A running job says "In progress" rather than showing a finish time that never happened.

## Status filter in History

All, Finished, Error, Cancelled or Running, each showing how many rows it would give. It narrows
the table, the record count and the CSV export, whose filename gains the status. It deliberately
does **not** narrow Delete, which still removes the selected server's whole history.

## Plan column

P5 reports the plan that ran and the pool it wrote to as two different things. Earlier versions
showed only the pool, under a column headed "Archive Plan", so the plan could not be seen at all.
A plan of `0` means P5 attributed the job to nothing, which is what an archive submitted straight
through the REST API looks like.

## jq is no longer required

Both CSVs are written by the app. They used to be produced by `jq`, searched for across eight
install locations and skipped quietly when it was not found, so a Mac without Homebrew got only the
raw JSON.

## Fixed

- The Size column in exported CSVs was quoted, so a spreadsheet read it as text and would not add
  it up. Numbers are now written unquoted.
- A value containing a quotation mark could break the row it was in, in the History export.

---

Requires macOS 14 or later. Universal, signed and notarized.
