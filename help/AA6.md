[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# AA6 — Local authentication bypass through patching

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

AA6 requires a local authentication control that an attacker can remove or override through patching or instrumentation.
The clients do not implement a PIN, biometric, or other local authentication gate.
Their runtime tampering and instrumentation exposure is covered by the applicable resilience cards.