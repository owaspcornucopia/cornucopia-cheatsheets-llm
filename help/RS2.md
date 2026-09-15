[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# RS2 — Debug and verbose diagnostics remain in production

## Threat

Debug code, verbose diagnostics, test resources, and runtime logging can reveal
implementation details or sensitive data after release. They also give a malicious
actor valuable clues for bypassing controls and chaining other weaknesses.

## How This Applies

The app's screen and debug build leave verbose `Log.d` statements enabled.
Each successful investigation records the question, generated SQL,
and returned rows. This overlaps with NS2 intentionally: NS2
focuses on the sensitive log data, while RS2 focuses on
debug/runtime diagnostics.

## Example Attack

1. Install the debug APK on an emulator.
2. Run a broad query such as `Show all transactions`.
3. Read the detailed `PwnedNextTraining` entries with Logcat.
4. Use the exposed SQL and table names to craft a damaging prompt.

## Mitigations

1. Remove or compile out verbose diagnostics in release builds.
2. Use different builds for test and production and a reviewed logging facade with strict redaction.
3. Disable debugging and test-only resources in production.
4. Minimize implementation details in user-visible errors and telemetry.
5. Verify the signed release artifact, inspect Logcat, and run reverse-engineering checks as part of release testing.

## MASTG and MASWE references

- MASTG tests: [0263](https://mas.owasp.org/MASTG-TEST-0263),
  [0264](https://mas.owasp.org/MASTG-TEST-0264),
  [0265](https://mas.owasp.org/MASTG-TEST-0265),
  [0358](https://mas.owasp.org/MASTG-TEST-0358), and
  [0359](https://mas.owasp.org/MASTG-TEST-0359).
- MASTG best practice: [0022](https://mas.owasp.org/MASTG-BEST-0022).
- MASTG knowledge: [0064](https://mas.owasp.org/MASTG-KNOW-0064) and
  [0101](https://mas.owasp.org/MASTG-KNOW-0101).
- MASWE weakness: [0061](https://mas.owasp.org/MASWE-0061).

## Applicability

The corresponding Cornucopia card is [RS2](https://cornucopia.owasp.org/cards/RS2).
