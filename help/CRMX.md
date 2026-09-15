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
