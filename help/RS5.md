# RS5 - Debuggable production-shaped build

## Threat

Attackers can inspect and manipulate a debuggable application at runtime when
debugging or dynamic instrumentation remains enabled.

## How This Applies

The debug build explicitly sets `debuggable true`. It also exposes the
investigation activity, app receiver, app providers, and verbose Logcat output
so a runtime app inspector can observe and change the workflow. The private
application manifest metadata
`org.owasp.pwnednext.android.SHOW_DEBUG_DETAILS` can additionally expose raw SQL
and returned rows in the review result when set to `true` for a debug build.

## Example Attack

Install the debug APK (app binary), attach a debugger or instrumentation tool,
and inspect the activity's retained result or alter the approval state.

## Mitigations

Never distribute debuggable builds, keep debug-only components out of release
manifests, disable verbose diagnostics, and verify the final merged manifest
and APK flags during CI.

## MASTG and MASWE references

- MASTG tests: [0226](https://mas.owasp.org/MASTG-TEST-0226),
  [0227](https://mas.owasp.org/MASTG-TEST-0227),
  [0261](https://mas.owasp.org/MASTG-TEST-0261).
- MASTG best practices: [0007](https://mas.owasp.org/MASTG-BEST-0007),
  [0008](https://mas.owasp.org/MASTG-BEST-0008).
- MASTG knowledge: [0007](https://mas.owasp.org/MASTG-KNOW-0007),
  [0028](https://mas.owasp.org/MASTG-KNOW-0028),
  [0062](https://mas.owasp.org/MASTG-KNOW-0062).
- MASWE: [0063](https://mas.owasp.org/MASWE-0063).
