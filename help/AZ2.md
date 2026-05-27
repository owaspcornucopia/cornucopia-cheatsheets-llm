[← Back to Cheat Sheet](/README.md)

# AZ2 — Influence Where Data Is Sent or Forwarded

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The AZ2 card describes an attacker influencing where data is sent or forwarded to (open redirects, SSRF, data exfiltration through forwarding).

This application does not forward or redirect data to external systems. It:

- Queries a local SQLite database
- Returns results directly to the caller via HTTP response
- Does not make outbound HTTP requests
- Does not send data to third-party services
- Does not have redirect functionality
- Does not fetch external resources based on user input

There is no mechanism for an attacker to redirect application output to a destination of their choosing. Data flows in one direction: database → API response to caller.
