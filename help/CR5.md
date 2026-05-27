[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CR5 — Cryptographic Controls Don't Fail Securely

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The CR5 card describes bypassing cryptographic controls because they do not fail securely (defaulting to unprotected when crypto fails).

This application has no cryptographic controls. There is no encryption, no signature verification, no hash validation, and no TLS termination within the application code. Since no cryptography is implemented, there is no crypto failure mode to exploit.

The total absence of cryptography is covered by CR6, CR7, CR8, and CRJ. CR5 would become applicable if encryption were added but the application fell back to plaintext when encryption failed (e.g., allowing unencrypted connections when TLS certificate validation fails).
