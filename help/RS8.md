[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# RS8 — Missing instrumentation detection

## Threat

Runtime hooks can expose sensitive data when the app neither detects nor responds
to instrumentation.

## Android-specific scenario

The Java investigation flow and native llama.cpp bridge run without Frida,
debugger, hook, or loaded-library checks. Prompts, generated SQL, database rows,
and fraud investigations can be observed or changed at runtime.

## Example attack

Hook the SQL executor and log or replace the query immediately before SQLite runs it.

## Mitigations

Combine runtime integrity checks, hook detection, server-side authorization, and
minimal local secrets. Treat detection as a signal, not the only control.

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

The Swift, C++, SQLite, and model operations run without hook or runtime-instrumentation detection.
