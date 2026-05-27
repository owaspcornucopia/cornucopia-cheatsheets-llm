[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# ATQ — Inconsistent Authentication Across Channels

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The ATQ card describes bypassing authentication because it is not enforced with equal rigor across all types of authentication functionality (registration, password change, recovery) or across all channels (mobile, web, API).

This application has:

- One endpoint (`/api/fraud`)
- One channel (HTTP API)
- One authentication mechanism (bearer token)
- No registration, password change, password recovery, or admin functions

There are no multiple channels or authentication-related functions where inconsistency could exist. The application is a single-purpose API with one path.
