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
