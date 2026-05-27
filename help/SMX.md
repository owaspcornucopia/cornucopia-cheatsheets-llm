[← Back to Cheat Sheet](/README.md)

# SMX — Missing CSRF Protection

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The SMX card describes forging requests because anti-CSRF tokens are not used for state-changing actions.

CSRF (Cross-Site Request Forgery) is a browser-based attack that exploits cookie-based authentication. It requires:

1. A victim's browser automatically attaching session cookies to requests
2. An attacker tricking the browser into making requests to the target application

This application uses a custom `token` header for authentication, not cookies. Browsers do not automatically attach custom headers to cross-origin requests. The Same-Origin Policy prevents JavaScript on attacker-controlled sites from setting custom headers on requests to other origins.

Since authentication is header-based (not cookie-based) and the API is consumed programmatically (not through a browser), CSRF is not applicable.
