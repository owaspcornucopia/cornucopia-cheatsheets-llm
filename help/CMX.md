[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# CMX — Path traversal

## Threat

A caller can read or overwrite unintended files when a target path is not
validated and contained.

## Android-specific scenario

`TrainingFileProvider` appends a URI path to the app files
directory controllable by a request from a malicous app. It never compares canonical paths, so `../` segments can escape the
reports location and traverse folders outside of it in order to reveal or change files.

## Example attack

Open an exported provider URI containing traversal segments to target another
file under the app's private directory.

## Mitigations

Use opaque file identifiers, resolve canonical paths, enforce a fixed root, and
reject `../` segments and unsupported open modes.

## References

- [OWASP MASTG platform testing](https://mas.owasp.org/MASTG/tests/android/MASVS-PLATFORM/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
