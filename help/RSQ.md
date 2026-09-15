[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# RSQ — Runtime patching and hook bypass

## Threat

An attacker can patch or hook critical functions when runtime tampering is not
detected or handled.

## Android-specific scenario

No control protects authorization helpers, model output, SQL execution, or fraud investigations from replacement.
The app continues normally after runtime behavior is changed.

## Example attack

Hook the approval helper to always return true or replace the generated SQL before execution.

## Mitigations

Use layered runtime-integrity signals and keep authorization decisions outside
the client. Stop sensitive actions when tampering evidence is present.

## References

- [OWASP MASTG resilience testing](https://mas.owasp.org/MASTG/tests/android/MASVS-RESILIENCE/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
