[← Back to Cheat Sheet](/README.md)

# SM2 — Control Over Session Identifier Generation

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The SM2 card describes an attacker having control over the generation of session identifiers or authorization tokens.

In this application, tokens are not generated at runtime. They are hardcoded UUID strings in the source code. There is no session creation flow, no token issuance endpoint, and no runtime token generation mechanism. An attacker cannot influence token generation because no generation occurs.

The relevant threats for these static tokens are covered elsewhere: AT5 (hardcoded tokens), CRJ (tokens in source code), and AT6 (tokens never expire).
