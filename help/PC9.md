[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# PC9 — Unvalidated interprocess input

## Threat

Another app can modify or expose sensitive data when IPC input is not validated
against a strict schema.

## Android-specific scenario

The exported app receiver trusts questions, authorization, and approval-token extras (added parameters).
The exported investigation provider accepts a caller's raw SQL. Those
values reach an investigation or database query without caller verification or
field-level validation.

## Example attack

Send an `INVESTIGATE` broadcast with an injected question or query the provider
with `1=1` as its selection. The app accepts the foreign input as its own.

## Mitigations

Keep components private where possible, require signature permissions, validate
every IPC (Inter-Process Communication) field, reject unknown fields,
and build parameterized queries.

## References

- [OWASP MASTG](https://mas.owasp.org/MASTG/)
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

URL parameters are accepted without a strict schema and flow into model generation, SQL execution, local-state changes, and file access.
