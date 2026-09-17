[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# NS6 — Missing device access security

## Threat

Data can be recovered from a stolen or decommissioned device when the app does
not require basic device security.

## Android-specific scenario

AI Anti Fraud 3.0 never checks whether the device has a secure lock screen,
current platform, disabled USB debugging, encryption, or an acceptable root
state before showing transaction data or approving a review.

## Example attack

Open an unlocked recovered device and review, copy, or export transaction details
without any app authentication.

## Mitigations

Require a device to be locked by default for sensitive use, add step-up authentication, 
use hardware-backed keys, and restrict functionality in unsupported device environments or risky app states.

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

The app performs investigations and report updates without checking device lock state or another trusted-device signal.
