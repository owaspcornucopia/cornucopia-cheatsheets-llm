[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AZ9 — Unsecured Administrative Interfaces

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The AZ9 card describes bypassing the application to gain access via unsecured administrative tools or interfaces.

This application does not expose administrative interfaces:

- No admin panel or dashboard
- No database management UI (phpMyAdmin, Adminer, etc.)
- No management API endpoints
- No debugging interfaces (Flask debug mode is not enabled)
- No monitoring dashboards exposed to the network

The only interface is the single `/api/fraud` endpoint. There are no alternative entry points through administrative tooling. The infrastructure concern (container security) is covered by C8.
