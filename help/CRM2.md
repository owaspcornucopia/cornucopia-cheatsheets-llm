[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CRM2 — Keys reused for multiple purposes

## Threat

Cryptographic keys and initialization vectors should have a single documented
purpose. Reusing a key or a fixed IV for unrelated storage and transport
operations can reveal relationships between plaintexts, enlarge the impact of
key recovery, and make rotation or access control impossible.

## How This Applies

`SuperSecureCrypto` contains a synthetic hard-coded AES key and fixed IV. The
same pair encrypts every transaction memo and is also reused for the model
transport header. Identical plaintexts therefore produce identical ciphertexts,
and extracting the APK (app binary) will reveal the key.

## Example Attack

1. Extract the debug APK or inspect the source.
2. Recover `PwnedNextKey` and the fixed IV.
3. Decrypt the `encrypted_memo` column from the local SQLite database.
4. Use the same key material to interpret the model transport header.

Even without source access, repeated ciphertexts reveal when two values are the
same because the IV never changes.

## Mitigations

1. Generate keys in Android Keystore and assign one documented purpose per key.
2. Use an authenticated encryption mode such as AES-GCM with a fresh nonce for
   every record.
3. Never hard-code production keys or IVs in the APK.
4. Separate storage, transport, and signing keys with distinct access policies.
5. Test key generation, storage, rotation, IV uniqueness, and failure behavior.

## MASTG and MASWE references

- MASTG tests: [0307](https://mas.owasp.org/MASTG-TEST-0307) and
  [0308](https://mas.owasp.org/MASTG-TEST-0308).
- MASTG knowledge: [0012](https://mas.owasp.org/MASTG-KNOW-0012).
- MASWE weakness: [0007](https://mas.owasp.org/MASWE-0007).
- The supplied mapping lists no MASTG best-practice entry for CRM2.

## Applicability

- [CRM2](https://cornucopia.owasp.org/cards/CRM2).

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

The fixed PwnedNextDemoKey and fixed-trainingIV are reused for transaction memos and model prompt material.
