# P5 Archive Overview 2.0.1 (Build 9)

Released 2026-08-10.

P5 Archive Overview no longer silently imports servers from a discovered `P5Servers.json` file. It now presents the source, connection details, and duplicate status before making changes.

Users can add new connections, defer the decision, or ignore that exact file revision. Accepted and ignored revisions are tracked by SHA-256 fingerprint. Connection matching excludes the editable alias, so renaming an imported server does not invalidate it or cause it to return as a new connection.

Passwords remain excluded from JSON and continue to be stored separately in macOS Keychain.
