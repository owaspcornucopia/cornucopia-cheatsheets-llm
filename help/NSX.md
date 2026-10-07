[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# NSX — Weak certificate pinning

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

NSX requires an application network path with certificate pinning that an attacker can bypass. LLM Inference, SQL execution, and the transaction store run on the device. The app has no extneral model-service or backend TLS connection where certificate pinning is configured.
