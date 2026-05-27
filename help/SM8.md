[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# SM8 — No Re-Authentication After Context Changes

## Threat

An attacker can abuse sessions because the application does not require re-authentication when context changes (such as privilege changes, environmental shifts, or long elapsed time).

## How This Applies

The token check is a simple "is this token in the list" verification with no contextual awareness. The application does not re-evaluate authentication when:

- A request comes from a new IP address or geographic location
- The request pattern changes dramatically (e.g., from one request per day to hundreds per minute)
- A long time has passed since the token was issued
- Sensitive operations are requested (e.g., queries returning personal data)

The same static token grants the same access regardless of when, where, or how it is used. There is no step-up authentication for high-risk operations.

## Example Attack

A token is normally used from a corporate IP range during business hours. An attacker uses the same token from a foreign IP at 3 AM to bulk-extract investigation data. The application does not notice the contextual change and serves all requests identically.

## Mitigations

1. **Implement contextual authentication** — require additional verification when requests come from new locations or exhibit unusual patterns.
2. **Add step-up authentication** for sensitive operations (e.g., accessing personal data fields).
3. **Monitor and flag context changes** — alert when a token is used from a new IP, location, or device.
4. **Require re-authentication** after extended periods of inactivity or when access patterns change significantly.
