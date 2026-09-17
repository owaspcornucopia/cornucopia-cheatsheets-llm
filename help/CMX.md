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

The custom URL path is passed to a file resolver that does not canonicalize or constrain the final path.
