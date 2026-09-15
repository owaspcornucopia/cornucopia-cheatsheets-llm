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
