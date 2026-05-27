[← Back to Cheat Sheet](/README.md)

# SMJ — Stolen Token Reuse Without Proof of Possession

## Threat

An attacker can reuse stolen session identifiers or tokens because there is no strong proof of possession binding the token to its legitimate holder.

## How This Applies

The application's tokens are simple bearer tokens — whoever presents the token is treated as authorized. There is no binding of tokens to:

- A specific client certificate
- A device fingerprint
- An IP address or IP range
- A user-agent string
- Any cryptographic proof of possession

This means a stolen token is fully reusable from any location, any device, at any time. There is no way to distinguish a legitimate use from a stolen one.

## Example Attack

An attacker intercepts the "Bankly customer" token from unencrypted network traffic. They use it from their own machine, a different IP, a different country. The application accepts it without question because there is no check beyond "is this string in the list."

## Mitigations

1. **Bind tokens to client attributes** — associate tokens with IP ranges, client certificates, or device fingerprints at issuance time.
2. **Implement mutual TLS (mTLS)** — require clients to present a certificate that is validated alongside the token.
3. **Use token binding** (RFC 8471) or DPoP (RFC 9449) to cryptographically bind tokens to the client's key.
4. **Detect reuse from new contexts** — flag and block when a token appears from an unrecognized device or location.
5. **Use short-lived tokens** so the window for reuse is minimized.
