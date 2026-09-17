[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# NS2 — Sensitive data in application logs

## Threat

Production logs often travel farther and live longer than developers expect.
Questions, generated SQL, names, amounts, and fraud results in Logcat can be
collected from a compromised device, a debug bridge, a crash reporter, or an
over-privileged diagnostic component.

## How This Applies

After every investigation the Android activity calls `Log.d` with the user
question, generated SQL, and returned database rows. The message is useful during
development and testing, but in production, it is unsafe application logging as described by NS2.
The release build doesn't appear to redact or remove log statements.

## Example Attack

1. Run `Is transaction TX-1002 fraudulent?`.
2. Connect to the emulator with `adb logcat`.
3. Filter for `PwnedNextTraining`.
4. Recover the natural-language prompt, SQL, payee names, and fraud decision.

On a real device, a malicious or compromised environment could read the same
records through system logging, instrumentation, or a crash/analytics SDK.

## Mitigations

1. Do not log prompts, generated SQL, transaction rows, tokens, or personal data.
2. Compile out debug logging from release versions and use a reviewed logging
   policy that ensures safe redactions.
3. Treat third-party crash and analytics SDKs as additional data recipients.
4. Test Logcat and crash reports on a production-like, non-debuggable build.
5. Rotate and delete retained logs according to your documented data-minimization
   policy.

## MASTG and MASWE references

- MASTG tests: [0203](https://mas.owasp.org/MASTG-TEST-0203),
  [0231](https://mas.owasp.org/MASTG-TEST-0231),
  [0296](https://mas.owasp.org/MASTG-TEST-0296), and
  [0297](https://mas.owasp.org/MASTG-TEST-0297).
- MASTG best practices: [0002](https://mas.owasp.org/MASTG-BEST-0002) and
  [0022](https://mas.owasp.org/MASTG-BEST-0022).
- MASTG knowledge: [0049](https://mas.owasp.org/MASTG-KNOW-0049) and
  [0101](https://mas.owasp.org/MASTG-KNOW-0101).
- MASWE weakness: [0005](https://mas.owasp.org/MASWE-0005).

## Applicability

The corresponding Cornucopia card is [NS2](https://cornucopia.owasp.org/cards/NS2).

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

The app logs the question, generated SQL, and returned rows through OSLog, even though the normal screen hides SQL and rows.
