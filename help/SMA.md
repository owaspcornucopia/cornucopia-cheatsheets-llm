[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# SMA — Unable to Detect Novel Session Management Attacks

## Threat

Novel attacks against session management go undetected because there is no logging or monitoring of session-related events.

## How This Applies

With logging disabled and no session tracking, the application cannot detect:

- Token theft (no record of where tokens are used from)
- Session replay attacks (no request uniqueness verification)
- Concurrent use from multiple locations (no session state tracking)
- Abnormal session patterns (no baseline to compare against)
- Token enumeration attempts (no logging of invalid tokens tried)

If an attacker discovers a new way to abuse the token system — perhaps through timing attacks, replay attacks, or by exploiting the lack of session binding — there is no detection mechanism that would flag the activity.

## Example Attack

An attacker discovers that by sending requests with precise timing, they can determine whether a UUID is a valid token or not (timing side-channel). They methodically enumerate valid tokens. Because there is no logging of failed authentication attempts, this brute-force probing goes entirely unnoticed.

## Mitigations

1. **Log all session-related events** — token usage, source IPs, timestamps, success/failure.
2. **Implement session anomaly detection** — alert on tokens used from multiple IPs, at unusual times, or at unusual rates.
3. **Monitor for enumeration attempts** — detect and block repeated requests with invalid tokens.
4. **Create baselines** of normal token usage patterns so deviations can be identified.
5. **Ensure constant-time token comparison** to prevent timing side-channel attacks.
