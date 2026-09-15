[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# RS9 — Insufficient obfuscation

## Threat

Readable code and bundled resources make security-sensitive behavior easy to reverse engineer.

## Android-specific scenario

Release minification is disabled. Class names, strings, SQL construction,
hard-coded crypto material, JNI entry points, and model assets remain easy to
identify in the APK (app binary).

## Example attack

Decompile the APK, search for `PwnedNextKey`, and locate the exported provider and raw-query path without dynamic analysis.

## Mitigations

Enable R8 (code optimizer and shrinker) for release builds, remove secrets from the client,
minimize sensitive strings, and rely on enforceable controls rather than security by obscurity.

## References

- [OWASP MASTG resilience testing](https://mas.owasp.org/MASTG/tests/android/MASVS-RESILIENCE/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
