[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CM8 — Insecure delegated Android actions

## Threat

An attacker can trigger malicious actions through insecure inter-app
communication or delegated operations.

## Android-specific scenario

The exported app functionality can recieve and start a fraud investigation through a 
question or approval controlled by a malicious app doing the request.
No permission or caller check protects the action.

## Example attack

A background app broadcasts a request to the model and supplies
`authorized=true` without opening the banking interface itself.

## Mitigations

Keep the reciever functionality private, require a signature permission, authenticate the
caller, and require visible user confirmation for sensitive delegated actions.

## References

- [OWASP MASTG platform testing](https://mas.owasp.org/MASTG/tests/android/MASVS-PLATFORM/)
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

The custom URL scheme acts as an unprotected delegated action: another app can start an investigation and provide report state.
