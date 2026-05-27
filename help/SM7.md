[← Back to Cheat Sheet](/README.md)

# SM7 — No Logout or Session Termination Capability

## Threat

An attacker can continue using a session after the legitimate user has finished because there is no logout function or way to terminate sessions.

## How This Applies

The application provides no mechanism to end a session or invalidate a token:

- There is no logout endpoint
- There is no "deactivate my token" function
- There is no administrative "revoke token" capability
- Once issued (hardcoded), a token is valid until the source code is changed

If a customer no longer needs access — because their contract ended, their staff changed, or they detected suspicious activity — they cannot terminate their own access. The only option is to contact the development team and wait for a code change and redeployment.

## Example Attack

A customer's employee who had access to the API token leaves the company. The company wants to revoke that person's access, but there is no way to invalidate the token without involving the application's development team. The former employee retains functional access until a new version is deployed.

## Mitigations

1. **Implement a token revocation endpoint** — allow authorized parties to invalidate specific tokens.
2. **Provide self-service token management** — let customers rotate or deactivate their own tokens.
3. **Add administrative controls** — give operators the ability to revoke any token immediately.
4. **Implement token lifecycle management** with clear creation, rotation, and termination phases.
