# NS5 - Backup and local-file exposure

## Threat

Sensitive data can be extracted or tampered with through backups, local
storage, or files exposed by an incorrectly configured provider.

## How This Applies

The manifest leaves `android:allowBackup="true"`. The app stores transaction
data in `pwnednext.db`, persists investigation results in preferences, and
exports a file provider that accepts caller-controlled paths.

## Example Attack

Inspect the manifest and use backup extraction or the file provider to request
`../databases/pwnednext.db`. On a debuggable emulator, the database can also be
pulled with `run-as`.

## Mitigations

Disable backup for sensitive applications or use explicit exclusion rules,
encrypt backed-up data with keys unavailable to the backup transport, keep
as little data as possible stored directly on the device, and never expose
arbitrary paths through a provider.

## MASTG and MASWE references

- MASTG tests: [0200](https://mas.owasp.org/MASTG-TEST-0200),
  [0201](https://mas.owasp.org/MASTG-TEST-0201),
  [0207](https://mas.owasp.org/MASTG-TEST-0207),
  [0215](https://mas.owasp.org/MASTG-TEST-0215),
  [0216](https://mas.owasp.org/MASTG-TEST-0216),
  [0262](https://mas.owasp.org/MASTG-TEST-0262),
  [0287](https://mas.owasp.org/MASTG-TEST-0287),
  [0298](https://mas.owasp.org/MASTG-TEST-0298),
  [0304](https://mas.owasp.org/MASTG-TEST-0304),
  [0305](https://mas.owasp.org/MASTG-TEST-0305),
  [0306](https://mas.owasp.org/MASTG-TEST-0306).
- MASTG best practices: [0004](https://mas.owasp.org/MASTG-BEST-0004),
  [0023](https://mas.owasp.org/MASTG-BEST-0023),
  [0050](https://mas.owasp.org/MASTG-BEST-0050).
- MASWE: [0001](https://mas.owasp.org/MASWE-0001),
  [0002](https://mas.owasp.org/MASWE-0002),
  [0006](https://mas.owasp.org/MASWE-0006).

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

The Application Support database is not excluded from backup, and caller-controlled support paths can reach files outside the intended directory.
