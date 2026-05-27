[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# SMQ — Session Management Not Applied Comprehensively

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The SMQ card describes bypassing session management because it is not applied consistently across the application.

This application has only one endpoint (`/api/fraud`) and one code path. The token check (such as it is) applies to every request that reaches the database query step. There is no second endpoint, no alternative API version, no websocket interface, and no admin path where session management could be inconsistently applied.

The issues with session management (that it is self-built and weak) are already covered by SMK. The issue of it being applied inconsistently does not arise because there is only one path through the application.
