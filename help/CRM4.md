[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRM4 — Guessable key material

## Threat

Keys can be recovered when their origin has low entropy or known input values.

## Android-specific scenario

The AES key is the readable ASCII value `PwnedNextKey`. It is neither
randomly generated nor derived with a password-based key derivation function.

## Example attack

Read the key from decompiled bytecode and decrypt copied ciphertext offline.

## Mitigations

Generate random keys with a cryptographic generator, store them in Android
Keystore, and use a reviewed KDF when deriving keys from user secrets.

## References

- [OWASP MASTG cryptography testing](https://mas.owasp.org/MASTG/tests/android/MASVS-CRYPTO/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
