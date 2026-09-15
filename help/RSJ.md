[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# RSJ — Unverified security-relevant files

## Threat

Attackers can alter configuration, restored data, or downloaded content when the
app uses it without integrity- and authenticity checks.

## Android-specific scenario

The app loads its GGUF AI model, adapter, SQLite data, and restored
preferences without a trusted checksum, signature, or authenticated-data check.

## Example attack

Replace an AI model or restored preference in a test environment,
then observe the app trust the altered SQL or fraud result.

## Mitigations

Verify signed AI model manifests, authenticate stored state, validate restored data,
and fail closed when integrity checks fail.

## References

- [OWASP MASTG resilience testing](https://mas.owasp.org/MASTG/tests/android/MASVS-RESILIENCE/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
