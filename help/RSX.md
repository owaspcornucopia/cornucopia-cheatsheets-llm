[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# RSX — Unrestricted hostile-device access

## Threat

Rooted, infected, or instrumented devices can access full functionality when the
app does not detect or respond to hostile environments.

## Android-specific scenario

AI Anti Fraud 3.0 records a device fingerprint but never checks root state,
instrumentation, app integrity, or emulator state before revealing data and
running the AI model.

## Example attack

Run the app on a rooted emulator and inspect its private database and model
workflow with all functionality still enabled.

## Mitigations

Use device-integrity signals, step-up authentication, least-privilege storage,
and server-side authorization. Define a measured response to hostile states.

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

Simulator, jailbroken, instrumented, or otherwise hostile environments receive the complete investigation and report functionality.
