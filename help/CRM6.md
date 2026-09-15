[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRM6 — Missing cryptographic integrity

## Threat

Encrypted data can be modified when confidentiality is provided without an
integrity check.

## Android-specific scenario

`SuperSecureCrypto` uses AES-CBC and stores only Base64 ciphertext. There is no
MAC or authenticated-encryption tag to prove that ciphertext was not altered.

## Example attack

Modify a ciphertext block and let the app attempt to decrypt the unauthenticated
value.

## Mitigations

Use AES-GCM or another reviewed authenticated encryption function.
Reject data before use when tag verification fails.

## References

- [OWASP MASTG cryptography testing](https://mas.owasp.org/MASTG/tests/android/MASVS-CRYPTO/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
