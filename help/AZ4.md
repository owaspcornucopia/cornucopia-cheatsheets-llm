[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AZ4 — Authorization Does Not Fail Securely

## Threat

An attacker can bypass authorization controls because the system defaults to allowing access when an error occurs.

## How This Applies

The authorization logic in this application has multiple failure modes that could result in access being granted when it should be denied:

1. If the `token` header is missing entirely, `request.headers.get('token')` returns `None`. The check `None in allowed_tokens` evaluates to `False` and correctly aborts — but this relies on `None` not being in the list rather than an explicit check for token presence.
2. If the `investigation_fraud()` function were to throw an exception before the token check (due to code changes or reordering), the auth check would never execute.
3. In the fallback mode (when the model fails to load), the application still generates tool call responses and attempts to execute queries — the authorization check still runs in this path, but the lack of explicit "deny by default" design means a subtle code change could bypass it.

The design does not follow a "deny by default" pattern where access is explicitly blocked unless all checks pass.

## Example Attack

A developer refactors the `investigation_fraud()` function and accidentally moves the `conn.execute(query)` line above the token check. Since the code doesn't enforce a "check auth first, then proceed" pattern at the architectural level, the change passes review and unauthenticated queries reach the database.

## Mitigations

1. **Adopt a deny-by-default architecture.** Structure the code so that requests are denied unless they explicitly pass all authorization checks.
2. **Separate authorization from business logic.** The token check should not be inside the same function that executes database queries.
3. **Use a decorator or middleware** that enforces authorization before the request handler body executes.
4. **Add integration tests** that verify unauthenticated and unauthorized requests are rejected regardless of code path.
