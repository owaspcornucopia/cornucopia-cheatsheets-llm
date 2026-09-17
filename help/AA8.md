# AA8 - Fail-open client authentication

## Threat

Authentication can be bypassed when the app trusts client-side results or
defaults to allowing access when verification is missing or fails.

## How This Applies

The app uses `authorized=true` when the caller omits the extra. The
approval flow therefore accepts missing authentication state instead of
denying the operation.

## Example Attack

Send the exported investigation broadcast without an `authorized` parameter.
The app receives the default allowed value and the approval succeeds.

## Mitigations

Use explicit deny-by-default state, require a cryptographically verified
authentication result, handle errors as failures, and repeat the authorization step
for each operation.

## MASTG and MASWE references

- MASTG tests: [0266](https://mas.owasp.org/MASTG-TEST-0266),
  [0267](https://mas.owasp.org/MASTG-TEST-0267),
  [0327](https://mas.owasp.org/MASTG-TEST-0327).
- MASTG best practice: [0036](https://mas.owasp.org/MASTG-BEST-0036).
- MASWE: [0020](https://mas.owasp.org/MASWE-0020).

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

Missing authorized input defaults to true, preserving the Android scenario's fail-open behavior.
