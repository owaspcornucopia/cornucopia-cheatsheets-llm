# PC8 - File-provider path traversal

## Threat

An attacker can traverse or modify protected files by exploiting a file-backed
content provider that does not validate canonical paths and containment.

## How This Applies

`TrainingFileProvider.openFile` joins a URI path to the app's files directory
without resolving the canonical path or checking that the result stays below
the intended reports directory. It is exported and grants URI permissions.

## Example Attack

```text
content://org.owasp.pwnednext.android.training-files/../databases/pwnednext.db
```

A caller can attempt to resolve the app's SQLite database through the exported
file boundary.

## Mitigations

Use `FileProvider` with a narrow XML path policy, canonicalize and compare the
resolved path with an approved root, reject traversal and symlinks, use
read-only grants where possible, and keep the provider non-exportable.

## MASTG and MASWE references

- MASTG test: [0357](https://mas.owasp.org/MASTG-TEST-0357).
- MASTG best practice: [0049](https://mas.owasp.org/MASTG-BEST-0049).
- MASWE: [0018](https://mas.owasp.org/MASWE-0018).

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

The URL path parameter is joined to Application Support without canonical containment checks before the app attempts to read it.
