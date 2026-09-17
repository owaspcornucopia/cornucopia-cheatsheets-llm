# NS7 - Sensitive data retained in memory

## Threat

Sensitive values can be recovered from process memory when prompts, rows, and
input values remain in ordinary objects after the operation has completed.

## How This Applies

`MainActivity` retains the full formatted investigation result in
`lastSensitiveResult`. It contains the model answer, generated SQL, all rows,
and data for as long as the activity instance remains alive.

## Example Attack

Run an investigation, keep the activity alive, and inspect the process with a debugger or memory-dump tool.
The full result remains reachable from the activity field instead of being cleared.

## Mitigations

Minimize sensitive data in memory during the lifetime of the app's state and changing authorization scope,
avoid retaining full result objects, clear references when the screen pauses,
use appropriately protected input controls (filtering/validation), and evaluate memory exposure on rooted,
debuggable, and backup-capable devices.

## MASTG and MASWE references

- MASTG knowledge: [0051](https://mas.owasp.org/MASTG-KNOW-0051),
  [0103](https://mas.owasp.org/MASTG-KNOW-0103).
- The supplied MobileApp mapping does not list a dedicated MASTG test or MASWE
  weakness for this card.

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

The view model and native model retain the question, prompt, SQL, rows, summary, and model output in process memory after the result is shown.
