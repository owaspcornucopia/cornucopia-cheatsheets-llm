[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# SM6 — No Session Timeout or Lifetime Limits

## Threat

An attacker can take over a user's session because there is no inactivity timeout or overall session time limit.

## How This Applies

The tokens in this application never expire. There is:

- No inactivity timeout — a token unused for years remains valid
- No maximum session duration — tokens work from deployment until the code is changed
- No automatic rotation after a period of time
- No requirement to re-authenticate periodically

This means a compromised token provides indefinite access. Even if the intended customer stops using the API, their token remains a valid entry point for an attacker.

## Example Attack

"NFT trader 5000" stops using the fraud investigation service after their business closes. Their token remains valid in the application. Years later, the token is discovered in an old configuration file backup and used by an attacker who now has full access to the system.

## Mitigations

1. **Set maximum token lifetime** — tokens should expire after a defined period (e.g., 30-90 days) and require renewal.
2. **Implement inactivity timeout** — automatically invalidate tokens that have not been used within a defined window.
3. **Force periodic re-authentication** — require token holders to obtain fresh credentials at regular intervals.
4. **Audit and remove unused tokens** — periodically review which tokens are active and remove those not in use.
