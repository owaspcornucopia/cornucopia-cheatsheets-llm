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

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

An attacker can use the matching iOS entry point, local storage, process state,
or on-device model flow to expose data or change the fraud investigation.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above. Test
the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and
verify that input validation, authorization, data minimization, integrity, and
protected storage are enforced.

### IOS details

The app trusts both downloaded GGUFs, UserDefaults state, and the persistent SQLite database after presence/size checks without authenticity verification. The native path records but does not load the TypeScript adapter.
