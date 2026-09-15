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
