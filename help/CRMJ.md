[← Back to Cheat Sheet](../README.md#fraud-investigation-llm-application--cheat-sheet)

# CRMJ — Cryptographic controls fail open

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

CRMJ requires a cryptographic failure path that defaults to unprotected operation.
The Android helper rejects blank input and throws when encryption or decryption fails.
The iOS helper does not fall back to plaintext.
The IOS XOR fallback is an inadequate alternate transformation covered by other cryptography cards, not a plaintext fail-open scenario.
