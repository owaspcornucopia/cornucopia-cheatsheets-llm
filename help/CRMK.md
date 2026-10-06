[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRMK — Cryptographic operations can be altered

## Threat

An attacker can influence or replace a cryptographic operation or its result and bypass the protection that operation is meant to provide.

## Android-specific scenario

`SuperSecureCrypto` performs encryption and decryption inside ordinary application code. However, the app does not detect runtime debugging, dynamic instrumentation, or hooks around `Cipher.getInstance`, `Cipher.init`,
`Cipher.doFinal`, or `SuperSecureCrypto.decrypt`.

Through an instrumented or patched process, an attacker can replace the cipher operation, capture the plaintext or key material, return altered plaintext, or skip decryption checks. The app then has no runtime-integrity signal with which to distinguish the original operation from the modified one.

## Example attack

1. Run the debug APK under a dynamic instrumentation framework.
2. Hook `Cipher.doFinal` or `SuperSecureCrypto.decrypt`.
3. Capture the decrypted memo or model prompt, or return attacker-controlled plaintext to the caller.
4. Let the application continue because it has no hook-detection response at the cryptographic boundary.

The iOS implementation has the equivalent exposure at `CCCrypt` and `InsecureTrainingCrypto.encrypt`.
Its non-CommonCrypto branch is a separate compile-time fallback that is using a repeated-key together with XOR. It's not a runtime hook, but it demonstrates that changing the selected implementation can remove the cryptographic protection entirely.

## Mitigations

Do not rely on client-side hooks as the only protection. Keep keys non-exportable in Android Keystore or iOS Keychain/Secure Enclave-backed facilities, use authenticated encryption, and validate authentication results before using plaintext.
Keep security-sensitive decisions and high-value keys outside of client processes.

Add layered runtime-integrity and instrumentation detection, fail closed when the process is debugged or modified, and verify that cryptographic providers, inputs, outputs, and code paths have not been replaced. Test the response to hooks rather than only testing the unmodified encryption path.

## References

- [OWASP MASTG cryptography testing](https://mas.owasp.org/MASTG/tests/android/MASVS-CRYPTO/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
- [MASTG-TEST-0341 — Runtime Use of Hook Detection Techniques for Android](https://mas.owasp.org/MASTG/tests/android/MASVS-RESILIENCE/MASTG-TEST-0341/)
- [MASTG-TEST-0354 — Runtime Use of Hook Detection Techniques for iOS](https://mas.owasp.org/MASTG/tests/ios/MASVS-RESILIENCE/MASTG-TEST-0354/)
- MASTG best practices: [0041](https://mas.owasp.org/MASTG-BEST-0041) for
  hardening against runtime hooking and [0048](https://mas.owasp.org/MASTG-BEST-0048)
  for hardening against reverse-engineering tools.
- MASTG knowledge: [0030](https://mas.owasp.org/MASTG-KNOW-0030) on reverse
  engineering tool detection, [0032](https://mas.owasp.org/MASTG-KNOW-0032)
  on runtime integrity verification, [0118](https://mas.owasp.org/MASTG-KNOW-0118)
  on RASP, and [0087](https://mas.owasp.org/MASTG-KNOW-0087) on reverse
  engineering tool detection.
- MASWE: [0058](https://mas.owasp.org/MASWE-0058) for runtime code integrity
  not being verified.

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

The app calls `CCCrypt` without detecting function interposition or runtime instrumentation.
A hooked operation can expose plaintext, replace ciphertext, or return a successful result without performing the intended encryption.
The same applies to `InsecureTrainingCrypto.encrypt` when the helper is patched.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above.
Test the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and verify that instrumentation, altered cryptographic calls, modified outputs, and protected key use are detected or rejected.

### IOS details

`InsecureTrainingCrypto` performs encryption and decryption using Apple's standard `CCCrypt` function without monitoring whether security tools are intercepting it in real time. The fallback implementation uses a repeated key with XOR. An altered or selected implementation can bypass encryption.
