[← Back to Cheat Sheet](../README.md#fraud-investigation-llm-application--cheat-sheet)

# NSK — Weak TLS trust validation

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

NSK requires a TLS client that can be tricked into trusting an impostor server because certificate, hostname, or custom trust validation is weak.
The mobile investigation has no runtime TLS client, certificate pinning, or custom trust manager.
