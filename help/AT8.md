[← Back to Cheat Sheet](/README.md)

# AT8 — Authentication Defaults to Allowing Access

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The AT8 card describes authentication that does not fail secure — it defaults to allowing unauthenticated access when the auth check fails.

In this application, when the token check executes, it correctly denies access with HTTP 401 if the token is invalid or missing. The check itself does fail secure. The problem is not that the check defaults to allowing — it's that the check is placed too late in the request pipeline (covered by ATJ).

When `request.headers.get('token') not in allowed_tokens` evaluates to True, the request is properly aborted. The authentication logic, while inadequate in many other ways (static tokens, no framework), does correctly deny access when it fires.
