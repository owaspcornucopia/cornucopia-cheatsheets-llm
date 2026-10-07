[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# CRMQ — Custom or inadequately implemented cryptography

## Threat

Simon can bypass hashing and encryption functions because they are custom and/or inadequately implemented.

## Android-specific scenario

`SuperSecureCrypto` is a hand-written helper around `AES/CBC/PKCS5Padding`. It embeds the readable key `PwnedNextDemoKey`,
reuses `fixed-trainingIV` for every transaction memo and model prompt, 
and stores only Base64 ciphertext. The ciphertext has no authentication tag.
AES is a standard primitive, but this wrapper is an inadequate implementation of protected storage.

## Example attack

1. Obtain the APK and a copy of the local database or prompt state.
2. Recover the key and IV from the APK or source.
3. Reproduce AES-CBC and decode the Base64 values to read memos or model
   prompts.
4. Modify a ciphertext value and observe that no authentication tag rejects the
   change.

The current scenario does not contain a custom hash implementation. CRMQ still
applies because its encryption implementation is inadequate; the hash-specific
references below are not evidence of a separate hash defect.

## Mitigations

- Use a reviewed platform or library AEAD API such as Android Keystore-backed
- AES-GCM. Generate a fresh random nonce for every record, store it with the ciphertext and authentication tag, keep keys non-exportable, separate key purposes, version the format, and reject failed authentication.
Remove any XOR or plaintext fallback. If the app needs hashing, use an approved hash, HMAC, or password KDF for its specific purpose rather than a custom function.

## References

- [OWASP MASTG cryptography testing](https://mas.owasp.org/MASTG/tests/android/MASVS-CRYPTO/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
- MASTG tests: [0210](https://mas.owasp.org/MASTG-TEST-0210) and
  [0211](https://mas.owasp.org/MASTG-TEST-0211) for iOS encryption and hashing,
  [0221](https://mas.owasp.org/MASTG-TEST-0221) for Android encryption, and
  [0232](https://mas.owasp.org/MASTG-TEST-0232) for Android encryption modes.
- MASTG best practices: [0005](https://mas.owasp.org/MASTG-BEST-0005) for
  secure encryption modes and [0009](https://mas.owasp.org/MASTG-BEST-0009) for
  secure encryption algorithms.
- MASTG knowledge: [0068](https://mas.owasp.org/MASTG-KNOW-0068) on
  cryptographic third-party libraries.
- MASWE: [0007](https://mas.owasp.org/MASWE-0007) for improper encryption and
  [0008](https://mas.owasp.org/MASWE-0008) for improper hashing.

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

The native helper exposes the same readable key and fixed IV to anyone who can inspect the app bundle or local data. Its non-CommonCrypto branch also falls back to repeating-key XOR, which does not provide cryptographic confidentiality.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above. Test
the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and
verify that input validation, authorization, data minimization, integrity, and protected storage are enforced.

### IOS details

`InsecureTrainingCrypto` uses CommonCrypto AES with `PwnedNextDemoKey` and `fixed-trainingIV` for training memos and model prompts.
If CommonCrypto is unavailable, it XORs the plaintext while repeating the key and IV bytes. No authentication tag is stored.
