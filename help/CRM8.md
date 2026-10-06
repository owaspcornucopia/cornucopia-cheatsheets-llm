[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRM8 — Insufficient cryptographic strength

## Threat

Cryptography can be broken when its effective work factor is below recommendations or below the effort available to a realistic attacker.

## Android-specific scenario

`SuperSecureCrypto` uses AES-CBC with a 16-byte key made from the readable string `PwnedNextDemoKey`. The key is embedded in the APK instead of being randomly generated and protected by Android Keystore. Every operation also reuses `fixed-trainingIV`, and the ciphertext has no authentication tag.

The AES primitive is not the only measure of strength. Predictable key material, a reused IV, unauthenticated CBC, and the absence of key separation make the effective protection substantially weaker than the nominal AES key size.

## Example attack

1. Extract the APK and a copy of the local database or model-prompt state.
2. Read the key and IV from the APK or source.
3. Reproduce AES-CBC and decode the stored Base64 values without brute-forcing the cipher.
4. Compare repeated ciphertexts to identify repeated plaintexts, or alter a ciphertext before a decrypting component consumes it.

The attacker is not required to perform cryptanalysis against AES.
The implementation reduces the problem to recovering readable application data and reusing exposed parameters, and the fallback makes the app even weaker by repeating the key with XOR. Something that does not provide adequate encryption strength.

## Mitigations

Generate random, adequately sized keys with Android Keystore or iOS Keychain
and Secure Enclave-backed facilities where appropriate. Use a reviewed authenticated-encryption construction such as AES-GCM or ChaCha20-Poly1305 with a fresh random nonce for every message, and keep key purposes separate.
Remove the XOR fallback, rotate keys through a versioned migration, and reject records whose authentication tag which is not verifiable.

Weigh algorithms, key sizes, modes, and providers against the expected attacker effort. If hashing or password protection is needed, use an approved hash, HMAC, or password KDF for that purpose rather than a custom function.

## References

- [OWASP MASTG cryptography testing](https://mas.owasp.org/MASTG/tests/android/MASVS-CRYPTO/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
- MASTG tests: [0208](https://mas.owasp.org/MASTG-TEST-0208) and
  [0209](https://mas.owasp.org/MASTG-TEST-0209) for key strength on Android and
  iOS; [0210](https://mas.owasp.org/MASTG-TEST-0210) and
  [0211](https://mas.owasp.org/MASTG-TEST-0211) for iOS encryption and hashing;
  [0221](https://mas.owasp.org/MASTG-TEST-0221) and
  [0232](https://mas.owasp.org/MASTG-TEST-0232) for Android encryption and
  modes; and [0312](https://mas.owasp.org/MASTG-TEST-0312),
  [0317](https://mas.owasp.org/MASTG-TEST-0317), and
  [0350](https://mas.owasp.org/MASTG-TEST-0350) for provider and mode checks.
- MASTG best practices: [0005](https://mas.owasp.org/MASTG-BEST-0005) for
  secure encryption modes, [0009](https://mas.owasp.org/MASTG-BEST-0009) for
  secure encryption algorithms, and
  [0020](https://mas.owasp.org/MASTG-BEST-0020) for the GMS Security Provider.
- MASTG knowledge: [0011](https://mas.owasp.org/MASTG-KNOW-0011) on security
  providers and [0012](https://mas.owasp.org/MASTG-KNOW-0012) on key generation.
- MASWE: [0013](https://mas.owasp.org/MASWE-0013) for improper key generation,
  [0007](https://mas.owasp.org/MASWE-0007) for improper encryption, and
  [0008](https://mas.owasp.org/MASWE-0008) for improper hashing.

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

`InsecureTrainingCrypto` exposes `PwnedNextDemoKey` and `fixed-trainingIV` as static data in the app.
Its CommonCrypto path uses AES with those same parameters and does not produce an authentication tag. If CommonCrypto is unavailable, the fallback XORs plaintext with repeating key and IV bytes.
Extracting the app bundle and local data makes sure the attacker can avoid to do cryptanalytic work in order to steal sensitive information.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above.
Test the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and
verify that key generation, algorithm selection, nonce uniqueness, integrity verification, and protected storage meet the expected requirements.

### IOS details

Training memos and model prompts use AES with a readable app-bundled key and a fixed IV. The non-CommonCrypto branch uses a repeating-key XOR, so it provides no meaningful cryptographic protections.