[← Back to Cheat Sheet](/README.md)

# CR2 — Obfuscation Used Instead of Cryptography

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The CR2 card describes accessing data because it has been obfuscated rather than using an approved cryptographic function.

This application does not attempt to protect data through obfuscation. Data (tokens, database contents, API traffic) is stored and transmitted in complete plaintext with no protection at all. The problem is not "wrong kind of protection" — it is "no protection."

The absence of encryption is covered by CR6 (unencrypted transit), CR7 (no TLS), CR8 (unencrypted storage), and CRJ (tokens in plaintext). CR2 would apply if the application attempted to hide data through encoding (Base64, ROT13, etc.) instead of using proper encryption.
