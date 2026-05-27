[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CRQ — Access or Predict Master Cryptographic Secrets

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The CRQ card describes accessing or predicting master cryptographic secrets (encryption keys, signing keys, key derivation secrets).

This application has no cryptographic secrets:

- No encryption keys (nothing is encrypted)
- No signing keys (nothing is signed)
- No HMAC secrets
- No key derivation functions
- No TLS private keys (no TLS configured)

The bearer tokens (covered by CRJ, AT5) are authentication credentials, not cryptographic keys. They are not used for encryption, signing, or any cryptographic operation. CRQ would apply if the application used encryption and the master keys could be predicted or accessed.
