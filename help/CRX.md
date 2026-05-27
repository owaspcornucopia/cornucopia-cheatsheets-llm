[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CRX — Cryptography Not Strong Enough

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The CRX card describes breaking cryptography because it is not strong enough for the degree of protection required.

This application does not use any cryptography. There is no encryption algorithm to break, no key length to evaluate, and no crypto configuration to assess. The issue is not "weak crypto" — it is "no crypto at all."

The complete absence of cryptographic protection is covered by CR6 (unencrypted transit), CR7 (no TLS), CR8 (unencrypted storage), and CRJ (plaintext credentials). CRX would become applicable if encryption were implemented using outdated algorithms (DES, MD5, SHA1 for hashing, short key lengths, etc.).
