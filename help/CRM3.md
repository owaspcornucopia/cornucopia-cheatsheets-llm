[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRM3 — Predictable cryptographic values

## Threat

Predictable keys, initialization vectors, or nonces weaken protected data.

## Android-specific scenario

`SuperSecureCrypto` uses the same readable 16-byte IV,
`trainingIV` is used for every AES-CBC encryption.

## Example attack

Encrypt equal memo prefixes twice and compare the repeated ciphertext blocks.

## Mitigations

Generate a fresh unpredictable IV for every encryption, store it with the
ciphertext, and prefer an authenticated mode such as AES-GCM.

## References

- [OWASP MASTG cryptography testing](https://mas.owasp.org/MASTG/tests/android/MASVS-CRYPTO/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
