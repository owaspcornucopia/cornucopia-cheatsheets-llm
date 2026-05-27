[← Back to Cheat Sheet](/README.md)

# SM3 — Stolen Session Tokens Cannot Be Detected or Terminated

## Threat

An attacker can continue to use a stolen session token for its full lifetime because users cannot detect or terminate compromised sessions.

## How This Applies

The static bearer tokens used by this application have no session management at all:

- There is no way for a token owner to see if their token is being used by someone else
- There is no "revoke session" or "terminate active sessions" capability
- There is no mechanism to detect that a token is being used from multiple locations simultaneously
- Tokens remain valid indefinitely regardless of suspicious activity

If a token is stolen (from source code, network sniffing, or a compromised system), the attacker can use it for as long as the application is running. The legitimate owner has no tools to discover the theft or respond to it.

## Example Attack

The "Bankly customer" token is intercepted during an HTTP request (no TLS). The attacker uses it from a different country. Bankly has no dashboard, no notification, and no "revoke all sessions" button. The stolen token remains usable forever.

## Mitigations

1. **Provide session visibility** — let token owners see recent API activity (last used, source IP, request count).
2. **Implement token revocation** — allow token owners and administrators to invalidate tokens immediately.
3. **Detect concurrent use** — alert when the same token is used from multiple distinct IPs simultaneously.
4. **Add session binding** — tie tokens to specific client attributes (IP range, client certificate) so stolen tokens cannot be used from other locations.
5. **Implement maximum session lifetime** — force token rotation at regular intervals.
