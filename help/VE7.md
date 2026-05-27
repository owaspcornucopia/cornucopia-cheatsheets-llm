[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# VE7 — Craft Payloads to Foil Input Validation

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The VE7 card describes crafting special payloads that bypass existing input validation through encoding tricks, canonicalization issues, or type confusion. This assumes input validation exists but can be tricked.

In this application, no input validation exists to foil. There is no character set restriction, no type checking, no canonicalization step, and no encoding normalization. An attacker does not need to craft special payloads to evade validation — they can simply send malicious input directly because nothing checks it.

The more fundamental issue (total absence of validation) is covered by VE3 and VEJ.
