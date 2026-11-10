# P5 Archive Overview 2.13 (Build 30)

A documentation release. Nothing in how the app queries P5 or stores history has changed.

## The guide now covers testing against a local P5 lab

The User Guide (Help, or `⌘?`) has a new section, **Testing against a local P5 lab**. It explains
how to point this app at a disposable P5 server running in a container on your own Mac, set up
with [P5 Local Lab](https://github.com/macvfx/P5-local-lab-setup), so you can see real P5 answers
without touching a production server.

- Which address, protocol and port to enter for a lab instance: `127.0.0.1`, HTTP, and `20000` for
  `p5-lab-01`, `20001` for `p5-lab-02`, and so on. Each instance is added as its own server.
- How the lab mounts a folder of source media read-only at `/media`, so a test archive job cannot
  change or delete the originals.
- That the lab checks an instance's ports are free before creating it, and what that means for the
  port you enter here.
- What to check when a lab server does not answer, and why a recreated instance should get a new
  alias.

The same section is in [`USER_GUIDE.md`](USER_GUIDE.md#testing-against-a-local-p5-lab).

Requires macOS 14 or later and an Archiware P5 server with REST API access (not required for a demo server).
