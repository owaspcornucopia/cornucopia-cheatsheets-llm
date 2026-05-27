[← Back to Cheat Sheet](/README.md)

# ATX — No Centralized Authentication Framework

## Threat

An attacker can bypass authentication because the application does not use a centralized, standard, tested authentication module.

## How This Applies

Authentication in this application is a single `in` operator check against a hardcoded Python list:

```
if not request.headers.get('token') in allowed_tokens:
    abort(401)
```

This is not a proper authentication framework. It provides:

- No token validation beyond list membership
- No cryptographic verification
- No integration with identity providers
- No standardized protocol (OAuth, OIDC, SAML)
- No audit capability
- No lifecycle management

The check is hand-written, not based on any security library, and placed inside a utility function rather than enforced architecturally. This ad-hoc approach is fragile and easy to bypass through code changes, path additions, or refactoring.

## Example Attack

A new endpoint is added to the application but the developer forgets to include the token check (since it's not enforced by a framework). The new endpoint is accessible without any authentication because there is no centralized mechanism that applies authentication to all routes automatically.

## Mitigations

1. **Adopt a standard authentication framework** — use Flask-Login, Flask-JWT-Extended, or an API gateway that handles authentication centrally.
2. **Enforce authentication at the middleware level** so all endpoints are protected by default and exceptions must be explicitly declared.
3. **Use a proven protocol** (OAuth 2.0, API keys via an API gateway) instead of hand-written token checking.
4. **Apply the same authentication standard** across all current and future endpoints.
5. **Test authentication enforcement** — automated tests should verify that all endpoints reject unauthenticated requests.
