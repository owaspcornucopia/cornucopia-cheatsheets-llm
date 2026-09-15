# PC6 - Unprotected inter-process communication

## Threat

Sensitive functionality can be invoked or intercepted by another application
when exported components, broadcasts, sharing, or local services lack narrow
permissions.

## How This Applies

The activity, `InvestigationReceiver`, `InvestigationProvider`, and
`TrainingFileProvider` are exported. No component requires a signature-level
permission, so every installed app can do fraud investigations, or database and file requests.

## Example Attack

Enumerate the package with `adb shell dumpsys package
org.owasp.pwnednext.android` and invoke the receiver or content providers
without holding any application-specific permission.

## Mitigations

Set `android:exported="false"` unless external access is required. For required
IPC (Inter-Process Communication), define a signature permission, use explicit intents (defined Android actions), validate the caller,
minimize returned data, and disable URI grants (temporary data sharing) unless they are essential.

## MASTG and MASWE references

- MASTG tests: [0364](https://mas.owasp.org/MASTG-TEST-0364),
  [0365](https://mas.owasp.org/MASTG-TEST-0365),
  [0366](https://mas.owasp.org/MASTG-TEST-0366),
  [0381](https://mas.owasp.org/MASTG-TEST-0381).
- MASTG best practices: [0052](https://mas.owasp.org/MASTG-BEST-0052),
  [0063](https://mas.owasp.org/MASTG-BEST-0063).
- MASWE: [0018](https://mas.owasp.org/MASWE-0018),
  [0032](https://mas.owasp.org/MASWE-0032).
