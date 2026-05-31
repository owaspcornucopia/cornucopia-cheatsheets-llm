[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AT7 — No Protection Against Brute Force Attacks

## Threat

An attacker can use brute force or dictionary attacks against authentication without limit.

## Non-Applicable

The application's token validation is a simple list membership check with no protective mechanisms:

- No rate limiting on failed authentication attempts
- No account lockout after repeated failures
- No progressive delays between attempts
- No CAPTCHA or proof-of-work requirement
- No alerting on repeated failed attempts

But the tokens are UUIDs (128-bit), making pure brute force impractical. An attacker with partial knowledge (e.g., they know the format is UUID v4) combined with information leaks (e.g., knowing some token characters from logs or error messages) could significantly reduce the search space, but it's more likely, in this case, that the token would be revealed in its entirety. In this case, it's more practical to just try common or default API keys.

## Example Attack

An attacker writes a script that sends requests with sequential or random UUIDs in the token header. The attacker would need to know certain token characters from logs or error messages for the attack to be effective.

## Mitigations

1. **Implement rate limiting** on failed authentication attempts — block or delay after N failures per IP.
2. **Alert on repeated failures** — notify operations when a threshold of failed attempts is reached from a single source.
3. **Move authentication to the entry point** so failed attempts are rejected immediately without consuming inference resources.
4. **Consider IP-based blocking** for sources generating excessive failed authentication requests.
5. **Log all authentication failures** with source IP and timestamp for forensic analysis.
6. **No default keys** Ensure the system doesn't use default or test credentials that could lead to unauthorised access.
