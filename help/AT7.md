[← Back to Cheat Sheet](/README.md)

# AT7 — No Protection Against Brute Force Attacks

## Threat

An attacker can use brute force or dictionary attacks against authentication without limit.

## How This Applies

The application's token validation is a simple list membership check with no protective mechanisms:

- No rate limiting on failed authentication attempts
- No account lockout after repeated failures
- No progressive delays between attempts
- No CAPTCHA or proof-of-work requirement
- No alerting on repeated failed attempts

The tokens are UUIDs (128-bit), making pure brute force impractical. However, an attacker with partial knowledge (e.g., they know the format is UUID v4) combined with information leaks (e.g., knowing some token characters from logs or error messages) could significantly reduce the search space. More practically, an attacker could try common or default API keys without any throttling.

## Example Attack

An attacker writes a script that sends requests with sequential or random UUIDs in the token header. The application processes each request, performs LLM inference (consuming resources), and only checks the token at query execution time. There is no lockout, no alerting, and no slowdown regardless of how many invalid tokens are tried.

## Mitigations

1. **Implement rate limiting** on failed authentication attempts — block or delay after N failures per IP.
2. **Alert on repeated failures** — notify operations when a threshold of failed attempts is reached from a single source.
3. **Move authentication to the entry point** so failed attempts are rejected immediately without consuming inference resources.
4. **Consider IP-based blocking** for sources generating excessive failed authentication requests.
5. **Log all authentication failures** with source IP and timestamp for forensic analysis.
