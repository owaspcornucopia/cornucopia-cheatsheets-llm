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

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

An attacker can use the matching iOS entry point, local storage, process state,
or on-device model flow to expose data or change the fraud investigation.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above. Test
the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and
verify that input validation, authorization, data minimization, integrity, and
protected storage are enforced at the native boundary.

### IOS details

Every training encryption operation reuses fixed-trainingIV, making equal plaintext produce repeatable ciphertext.
