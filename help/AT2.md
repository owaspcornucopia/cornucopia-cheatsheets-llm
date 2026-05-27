[← Back to Cheat Sheet](/README.md)

# AT2 — Authentication Actions Without User Awareness

## Threat

An attacker can undertake authentication functions (log in, use credentials) without the real user ever being aware this has occurred.

## How This Applies

The application uses static bearer tokens with no session tracking, login notifications, or activity logging. When a token is used:

- The legitimate token owner is not notified
- There is no login event recorded
- There is no way to see active sessions or recent access
- There is no mechanism to detect concurrent use from different locations

If an attacker obtains one of the hardcoded tokens (from source code, git history, or a compromised system), they can use it indefinitely. The legitimate customer will never know their token is being used by someone else because the system provides no visibility into authentication events.

## Example Attack

"Jon's backdoor token" is used by an unauthorized party to query fraud investigation data. The Bankly team (whose token is also in the system) has no way to know that someone else is accessing the same system with a different token, and Jon has no audit trail showing when "his" token was used.

## Mitigations

1. **Log all authentication events** — record when tokens are used, from which IP addresses, and at what frequency.
2. **Implement token usage notifications** — alert token owners of unusual access patterns.
3. **Provide visibility** — give API consumers a way to see when their token was last used.
4. **Add session uniqueness** — associate each API request with contextual metadata (IP, user-agent) to detect anomalous use.
5. **Implement token rotation** so that long-lived static tokens are replaced regularly.
