# P5 Archive Overview 2.10 (Build 25)

P5 Archive Overview can now run entirely on made-up data, for a demo or a screenshot, with no P5 server or credentials at all.

## Demo Mode

- Flag a server as a demo server in **Manage Servers → Add**, then pick a scenario: Healthy Fleet, Drive Needs Cleaning, Busy Archive Week, or Index Discovery.
- The "Add Server" sheet suggests a name for the new server — a Canadian city paired with a production type, such as "Calgary VFX" or "Montreal Post Prod" — with a shuffle button for another.
- Job descriptions and archived folders in the demo data use the same style, such as "Halifax Film Location."
- A demo server answers the same query path, job-detail matching and history recording a real server does — nothing in the app behaves differently, only where the data comes from.

## Safety boundary

A demo server never connects to anything. It makes no network request and stores no real credentials.

Requires macOS 14 or later and an Archiware P5 server with REST API access (not required for a demo server).
