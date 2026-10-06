[← Back to Cheat Sheet](../README.md#fraud-investigation-llm-application--cheat-sheet)

# CRM5 — Obfuscation instead of cryptography

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

CRM5 applies when sensitive data is hidden with obfuscation instead of an approved cryptographic function.
The mobile crypto paths do use encryption helpers, but they are weakly configured. Their issues are covered by CRM2, CRM6, CRM8, CRM9, CRMK, and CRMQ. No separate obfuscation-only protection is used.
