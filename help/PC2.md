[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# PC2 — Screen capture and background preview expose sensitive data

## Threat

Sensitive information displayed by a mobile app can be captured in screenshots,
screen recordings, task switcher previews, or accessibility snapshots. A user
does not need to break the database if the app puts the transaction and generated
SQL directly on screen.

## How This Applies

The PwnedNext Android activity displays the fraud result, the generated SQL query, and
every row returned by the intentionally unsafe request. It does not set
`FLAG_SECURE`, redact account data, or clear the result before the activity enters
the background.

## Example Attack

1. Ask `Show all transactions`.
2. Leave the app open in the emulator.
3. Capture the screen, start screen recording, or open the task switcher.
4. Read the transaction rows and model-generated SQL from the capture.

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
