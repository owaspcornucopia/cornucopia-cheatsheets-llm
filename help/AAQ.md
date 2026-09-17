[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# AAQ — Authorization bypass across components

## Threat

An attacker can bypass authorization by moving commands or data through views,
processes, and exported components.

## Android-specific scenario

Intent extras (key-value data pairs that pass information between different components
of an app or between separate apps) and content provider arguments are sent across 
component boundaries without checking who is calling them.
Client-supplied `authorized` state from a malicious app can launch an automatic review,
while a malicious app's selection of content providers can reach the data in the
database directly.

## Example attack

An untrusted app sends an exported intent (Android app service request) that starts an
unattended investigation and marks its own request as authorized.

## Mitigations

Authorize at every component boundary, ignore client assertions, protect exported
components, and bind approvals to an authenticated user and transaction.

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
protected storage are enforced at the native boundary.

### IOS details

Deep-link values cross from the external URL boundary into the investigation database and report update without caller authorization.
