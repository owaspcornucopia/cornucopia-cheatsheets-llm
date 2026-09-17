[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRMX — Hard-coded cryptographic key

## Threat

Attackers can extract keys or protected data when key material ships inside the
application package.

## Android-specific scenario

`PwnedNextKey` is a static byte array in `SuperSecureCrypto`. Every
installation receives the same key, and decompilation reveals it.

## Example attack

Search the APK strings (app package strings) for the key and use it with the fixed IV to decrypt local memo ciphertext.

## Mitigations

Never ship shared secrets in a client. Generate per-installation keys in Android Keystore and rotate keys through a controlled migration.

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

The reusable AES key and fixed IV are recoverable from the app binary and source-level training surface.
