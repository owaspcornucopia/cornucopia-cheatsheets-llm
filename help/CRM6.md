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

The AES-CBC training ciphertext has no MAC or authenticated-encryption tag, so tampering is not detected.
