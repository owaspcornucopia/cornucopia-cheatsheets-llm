[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# AA4 — Unused unlocked key

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

AA4 requires an application flow that unlocks a protected key but then performs the sensitive operation without using that unlocked key. These clients have no user-unlocked Android Keystore or iOS Keychain key in the investigation flow.
The embedded training key is addressed by the applicable cryptography cards.
