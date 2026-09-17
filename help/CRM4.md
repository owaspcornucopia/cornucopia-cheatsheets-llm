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

The AES key is a readable product string compiled into the app rather than randomly generated key material.
