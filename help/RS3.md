# RS3 - Debug metadata in the package

## Threat

Debug symbols and non-production metadata can disclose sensitive data and
implementation details or make security controls easier to disable.

## How This Applies

The training APK (app's binary) is intentionally readable.
Release minification is disabled, the model endpoint and provider authorities are strings,
BuildConfig state is shown, and the package contains training secrets.

## Example Attack

Decode the APK (app binary) with `apkanalyzer` or a reverse-engineering tool and search for
`PwnedNextKey`, provider authorities, SQL strings, and the model URL.

## Mitigations

Use release-only builds for distribution, enable appropriate shrinking and
obfuscation, remove debug resources and secrets, and verify that symbols and
diagnostic metadata are not packaged.

## MASTG and MASWE references

- MASTG tests: [0219](https://mas.owasp.org/MASTG-TEST-0219),
  [0288](https://mas.owasp.org/MASTG-TEST-0288).
- MASTG knowledge: [0063](https://mas.owasp.org/MASTG-KNOW-0063).
- MASWE: [0061](https://mas.owasp.org/MASWE-0061).

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

The app bundle and binary retain readable Swift/C++ strings, SQL schema details, model metadata, and training secrets.
