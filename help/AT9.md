[← Back to Cheat Sheet](/README.md)

# AT9 — Inconsistent Authentication Strength for Critical Functions

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The AT9 card describes being able to perform critical functions because authentication requirements are too weak or there is no requirement to re-authenticate for sensitive operations.

This application has only one function: querying fraud investigations. There are no tiered operations with different criticality levels. There is no distinction between:

- Read-only queries vs. data modification
- Low-sensitivity vs. high-sensitivity data access
- Regular operations vs. administrative functions

All requests go through the same single endpoint with the same (weak) authentication. Since there are no "more critical functions" requiring stronger auth, the concept of step-up authentication does not apply. The weakness of the overall authentication is already covered by other cards (AT5, ATX, AT6).
