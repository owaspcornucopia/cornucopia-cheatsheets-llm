[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# RSQ — Runtime patching and hook bypass

## Threat

An attacker can patch or hook critical functions when runtime tampering is not
detected or handled.

## Android-specific scenario

No control protects authorization helpers, model output, SQL execution, or fraud investigations from replacement.
The app continues normally after runtime behavior is changed.

## Example attack

Hook the approval helper to always return true or replace the generated SQL before execution.

## Mitigations

Use layered runtime-integrity signals and keep authorization decisions outside
the client. Stop sensitive actions when tampering evidence is present.

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

No runtime integrity response protects model output, authorization state, report updates, or fraud decisions from hooks or patching.
