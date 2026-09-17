[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRM7 — Inadequate platform data protection

## Threat

Sensitive stored data can be extracted or modified when platform-backed
protection is not used.

## Android-specific scenario

The app protects demo memos with a key embedded in the APK, not a non-exportable
Android Keystore key. Anyone who obtains the APK and ciphertext has both pieces.

## Example attack

Extract the database and APK (app binary), recover the key from bytecode,
and decrypt the stored memo on another machine.

## Mitigations

Use Android Keystore, bind sensitive keys to user authentication where suitable,
and minimize the data retained on the device.

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

Training data uses an app-bundled key instead of Keychain or Secure Enclave protection.
