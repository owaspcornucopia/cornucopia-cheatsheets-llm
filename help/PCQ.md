[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# PCQ — Manipulated interprocess communication

## Threat

Another app can alter data or behavior by influencing messages exchanged between Android components.

## Android-specific scenario

The activity, receiver, transaction provider, and file provider are exported
without permissions. Callers can supply questions, approval state,
SQL queries, sql projections, and file paths.

## Example attack

A second app broadcasts an investigation with `authorized=true`, or queries the provider for every transaction column.

## Mitigations

Set `exported=false` unless sharing is required. Otherwise require signature
permissions, authenticate & authorize the caller, and validate all message fields.

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

A third-party app can supply questions, SQL predicates, authorization state, replay tokens, fraud overrides, and paths through the custom URL entry point.
