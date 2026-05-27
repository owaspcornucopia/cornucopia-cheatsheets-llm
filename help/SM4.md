[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# SM4 — Session Cookies Scoped Incorrectly

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The SM4 card describes setting session cookies for another web application because the domain or path are not restricted sufficiently.

This application does not use cookies. Authentication is performed via a custom `token` HTTP header. There are no:

- Session cookies
- Cookie domain or path settings
- Cookie security flags (Secure, HttpOnly, SameSite)
- Cookie-based session identifiers

Since no cookies are used, incorrect cookie scoping cannot occur. The token-in-header approach has its own issues (covered by SM9, SMJ) but cookie-specific threats are not applicable.
