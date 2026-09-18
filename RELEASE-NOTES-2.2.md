# P5 Archive Overview 2.2 (Build 16)

Released 2026-09-18.

Each server now chooses whether to reach P5 in the clear or over TLS, and the app finally has a
guide rather than an About box under a menu item marked Help.

## Each server chooses HTTP or HTTPS

P5 serves the same REST API two ways: in the clear on port 8000, and over TLS on port 8443.
Measured against a live server, the TLS port returns the identical archive entries and pools as the
plain one. Which one a server offers is decided when P5 is installed, so this is a setting on each
server rather than one switch for the app — of three servers checked, 8443 answered on one and not
on another.

- Choosing HTTPS moves the port to 8443. A port you typed yourself is left alone.
- Existing servers are untouched and stay on HTTP.

## Certificate checking

P5 ships a self-signed certificate whose subject identifies nothing, and macOS will not accept it.
Choosing HTTPS without settling that would not give a secure connection — it would give no
connection.

- **Check Certificate** shows the SHA-256 fingerprint and the subject of the certificate the server
  actually presents, and whether macOS trusts it.
- Trusting it once records that exact certificate. If the server later presents a **different** one
  the connection is refused rather than quietly accepted, and the sheet says so before you can
  accept the new one.
- A server whose administrator installed a real certificate needs no trusting at all. Ordinary
  verification succeeds and renewals keep working without asking again.

Sharing a server list carries the protocol but never a trusted certificate: that is a decision made
on one machine about one server, and inheriting someone else's silently would defeat the point of
making it.

## A guide, in the app

**Help → P5 Archive Overview User Guide** (`⌘?`) covers the workflow, what a row actually means,
how history identifies a job, the files the app writes, and the errors worth recognising.
**Help → What's New** lists each version's changes. Previously the Help menu opened the About
window.

## Fixed

- A certificate that did not match the one trusted reported itself as "cancelled", which read as
  though you had stopped the query yourself. It now says the certificate did not match.

## How private the connection is

Over a VPN such as Tailscale or WireGuard, the connection is already encrypted and the peer already
authenticated before P5 sees any of it, whichever protocol P5 itself is given. TLS earns its place
on a plain LAN, on someone else's network, or where an administrator has installed a real
certificate.

---

Requires macOS 14 or later. Universal, signed and notarized.
