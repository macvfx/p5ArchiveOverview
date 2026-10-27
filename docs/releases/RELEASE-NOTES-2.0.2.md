# P5 Archive Overview 2.0.2 (Build 10)

Released 2026-08-11.

This release makes archive paths easier to read and prevents an unavailable P5 server from holding up the workflow indefinitely.

## What changed

- The Directories column now receives the flexible table space by default in both Current and History views.
- Compact metadata columns have bounded widths, and manual column resizing is restored after relaunching the app.
- Offline server connection attempts stop after eight seconds.
- An active query can be stopped with **Cancel Query** or `Esc`; the server list remains available so another configured server can be selected.
- Connected P5 overview requests retain a five-minute ceiling for unusually large responses.
- Cancelled requests remove temporary response files and are reported separately from query failures.

Passwords remain stored in macOS Keychain and are never included in exported server JSON.
