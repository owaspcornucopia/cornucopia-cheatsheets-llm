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

AES-CBC with a fixed IV is used for training memos and prompts, preserving predictable ciphertext patterns.
