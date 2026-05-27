[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AT6 — Static Tokens That Never Expire or Rotate

## Threat

An attacker can reuse authentication tokens indefinitely because they have no expiration, rotation mechanism, or revocation capability.

## How This Applies

The five hardcoded tokens in the application:

- Never expire — they are valid from deployment until the code is changed
- Cannot be rotated without a code change and redeployment
- Cannot be revoked independently (removing one token requires editing the source code list)
- Have no usage-based expiration (they don't become invalid after a period of non-use)
- Have no maximum lifetime

If a token is compromised, it remains valid until someone notices and pushes a code change. There is no "invalidate this token" operation available to administrators.

## Example Attack

A token belonging to "NFT trader 5000" is compromised in a data breach on their side. Because the token never expires and cannot be revoked remotely, the attacker has indefinite access to the fraud investigation API until the development team learns about the breach, updates the code, and redeploys.

## Mitigations

1. **Implement token expiration** — tokens should have a maximum lifetime (e.g., 90 days) after which they must be renewed.
2. **Provide a revocation mechanism** — administrators should be able to invalidate a specific token immediately without redeploying.
3. **Implement token rotation** — issue new tokens periodically and retire old ones.
4. **Add idle timeout** — tokens unused for a defined period should be automatically invalidated.
5. **Use an authentication service** (OAuth 2.0, API gateway) that handles token lifecycle management.
