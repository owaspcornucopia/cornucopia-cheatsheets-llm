[← Back to Cheat Sheet](/README.md)

# AZJ — Access Security Configuration or Access Control Lists

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The AZJ card describes accessing security configuration information or access control lists through the application.

The access control "list" (the allowed_tokens array) is hardcoded in Python source code. It is not accessible through the application's API or any database query. An attacker cannot:

- Query the token list through the `/api/fraud` endpoint
- Retrieve authorization configuration via SQL injection (the tokens are in Python memory, not in the database)
- Access an admin endpoint that exposes security settings (no admin endpoints exist)

The exposure of tokens through source code (repository access, container inspection) is a separate concern covered by CRJ and AT5. But the application itself does not provide a path to read its own security configuration.
