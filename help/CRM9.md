[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRM9 — Unsafe cryptographic configuration

## Threat

Protected data can be exposed when an algorithm, mode, IV, nonce, or provider is
used incorrectly.

## Android-specific scenario

The app combines AES-CBC with one fixed IV and no authentication tag. Equal
plaintext prefixes create repeatable ciphertext patterns, and tampering is not
detected.

## Example attack

Compare encrypted records to identify repeated memo prefixes without decrypting
them.

## Mitigations

Use a reviewed AEAD mode, a unique nonce for every message, secure key storage,
and explicit versioning for the encryption format.

## References

- [OWASP MASTG cryptography testing](https://mas.owasp.org/MASTG/tests/android/MASVS-CRYPTO/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
