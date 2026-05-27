[← Back to Cheat Sheet](/README.md)

# VE5 — Bypass Centralized Encoding Routines

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The VE5 card describes bypassing encoding routines that prevent output-based attacks such as cross-site scripting (XSS). This assumes the application produces HTML or other rendered content where output encoding is required.

This application is a REST API that returns JSON responses. It does not:

- Render HTML pages
- Produce output consumed by a browser rendering engine
- Use output encoding routines (because none are needed for JSON API responses)

Since there is no browser rendering context, output encoding for XSS prevention is not applicable. The API output is consumed programmatically by other systems.
