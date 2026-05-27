[← Back to Cheat Sheet](/README.md)

# VEQ — Client-Side Injection

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The VEQ card describes injecting data into client-side interpreters (browser JavaScript, DOM manipulation, HTML rendering). This requires the application to produce content that is rendered in a browser.

This application is a backend REST API that:

- Returns JSON responses consumed by other systems
- Does not render HTML
- Does not serve JavaScript
- Does not produce content displayed in a browser
- Has no web frontend

Client-side injection (XSS, DOM-based attacks) is not applicable to a server-side JSON API. The server-side injection equivalent (SQL injection via LLM output) is covered by VEK.
