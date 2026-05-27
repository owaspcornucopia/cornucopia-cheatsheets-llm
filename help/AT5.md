[← Back to Cheat Sheet](/README.md)

# AT5 — Default, Test, and Backdoor Credentials

## Threat

An attacker can authenticate using default, test, or easily guessable credentials that should not exist in a production system.

## How This Applies

The application uses a hardcoded list of bearer tokens for authentication. Among these tokens are:

- **"Jim's personal debug token"** — a token explicitly labelled for debugging, which should never exist in production
- **"Jon's backdoor token"** — a token explicitly labelled as a backdoor, indicating intentional unauthorized access capability

These tokens are hardcoded in the source code alongside legitimate customer tokens. Anyone with access to the source code (or anyone who obtains these token values) can authenticate as a valid user. The tokens are static UUIDs that never expire and cannot be revoked without a code change and redeployment.

## Example Attack

A former employee or a contractor who has seen the source code uses "Jon's backdoor token" to access the fraud investigation API long after their involvement with the project has ended. Because the tokens are static and embedded in code, there is no way to revoke access without changing the application.

## Mitigations

1. **Remove all debug and backdoor tokens immediately.** These have no place in any environment.
2. **Move token management out of source code** and into a secure credential store or identity provider.
3. **Implement token expiration and rotation** so that compromised tokens become useless after a defined period.
4. **Require individual accounts** for each API consumer with unique credentials tied to their identity.
5. **Implement token revocation** so that access can be removed without redeploying the application.
6. **Audit existing tokens** regularly to identify and remove unused or unnecessary credentials.
