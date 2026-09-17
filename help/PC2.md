[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# PC2 — Screen capture and background preview expose sensitive data

## Threat

Sensitive information displayed by a mobile app can be captured in screenshots,
screen recordings, task switcher previews, or accessibility snapshots. A user
does not need to break the database if the app puts the transaction and generated
SQL directly on screen.

## How This Applies

The PwnedNext Android activity displays the fraud result and human-readable model
notes by default. Generated SQL and returned rows appear only when the developer
sets the application manifest metadata
`org.owasp.pwnednext.android.SHOW_DEBUG_DETAILS` to `true` and builds the debug
APK. The activity does not set `FLAG_SECURE`, redact account data, or clear the
result before entering the background.

## Example Attack

1. Set `org.owasp.pwnednext.android.SHOW_DEBUG_DETAILS` to `true` in the
   application manifest and build/install the debug APK.
2. Ask `Show all transactions`.
3. Leave the app open in the emulator.
4. Capture the screen, start screen recording, or open the task switcher.
5. Read the transaction rows and model-generated SQL from the capture.

The same issue can expose prompts and results when a user shares a device or when
a preview is collected by a management tool.

## Mitigations

1. Set `WindowManager.LayoutParams.FLAG_SECURE` on screens containing sensitive
   content, while understanding that it is a defense-in-depth control.
2. Mask names, identifiers, amounts, and generated SQL by default.
3. Clear sensitive views when the activity pauses and avoid putting secrets in
   accessibility or notification content.
4. Use an explicit reveal action with re-authentication for high-risk details.
5. Test screenshots, recordings, recents previews, and accessibility services on
   supported Android versions.

## MASTG and MASWE references

- MASTG tests: [0289](https://mas.owasp.org/MASTG-TEST-0289),
  [0290](https://mas.owasp.org/MASTG-TEST-0290),
  [0291](https://mas.owasp.org/MASTG-TEST-0291),
  [0292](https://mas.owasp.org/MASTG-TEST-0292),
  [0293](https://mas.owasp.org/MASTG-TEST-0293), and
  [0294](https://mas.owasp.org/MASTG-TEST-0294).
- MASTG best practices: [0014](https://mas.owasp.org/MASTG-BEST-0014),
  [0015](https://mas.owasp.org/MASTG-BEST-0015),
  [0017](https://mas.owasp.org/MASTG-BEST-0017),
  [0018](https://mas.owasp.org/MASTG-BEST-0018), and
  [0033](https://mas.owasp.org/MASTG-BEST-0033).
- MASTG knowledge: [0053](https://mas.owasp.org/MASTG-KNOW-0053),
  [0099](https://mas.owasp.org/MASTG-KNOW-0099), and
  [0076](https://mas.owasp.org/MASTG-KNOW-0076).
- MASWE weaknesses: [0038](https://mas.owasp.org/MASWE-0038) and
  [0034](https://mas.owasp.org/MASWE-0034).

## Applicability

The corresponding Cornucopia card is
[PC2](https://cornucopia.owasp.org/cards/PC2); the app behavior is in
[`MainActivity`](https://github.com/owaspcornucopia/llm-companion-scenario-android/blob/main/app/src/main/java/org/owasp/pwnednext/android/MainActivity.java).

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

The SwiftUI result screen does not use a secure display flag. The natural-language answer can appear in screenshots, recordings, previews, and accessibility snapshots.
