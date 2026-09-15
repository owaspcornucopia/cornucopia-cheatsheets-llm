# PC3 - Excessive and unmasked sensitive data

## Threat

An attacker can capture sensitive data displayed, entered, cached, or made
available to unnecessary third-party components because the application does
not minimize, mask, or clear it.

## How This Applies

The Android fraud screen renders the complete generated SQL, all returned
columns, encrypted values, payee names, and the model answer. The same
complete result can be copied to the system clipboard. The manifest also
declares unrelated location, camera, microphone, media, and notification
permissions.

## Example Attack

1. Run an investigation for `TX-1002`.
2. Tap **Copy complete result to clipboard**.
3. Read the clipboard from another app or inspect the unmasked result on the
   screen.
4. Review the manifest. It grants unrelated device capabilities that this
   fraud workflow never uses.

## Mitigations

Return only the fields needed for the current action, mask identifiers and
amounts in the UI, use sensitive clipboard flags or avoid copying entirely,
clear temporary values after use, and request only feature-specific runtime
permissions.

## MASTG and MASWE references

- MASTG tests: [0316](https://mas.owasp.org/MASTG-TEST-0316),
  [0320](https://mas.owasp.org/MASTG-TEST-0320),
  [0346](https://mas.owasp.org/MASTG-TEST-0346),
  [0347](https://mas.owasp.org/MASTG-TEST-0347),
  [0378](https://mas.owasp.org/MASTG-TEST-0378),
  [0390](https://mas.owasp.org/MASTG-TEST-0390).
- MASTG best practices: [0028](https://mas.owasp.org/MASTG-BEST-0028),
  [0044](https://mas.owasp.org/MASTG-BEST-0044),
  [0059](https://mas.owasp.org/MASTG-BEST-0059),
  [0060](https://mas.owasp.org/MASTG-BEST-0060),
  [0069](https://mas.owasp.org/MASTG-BEST-0069).
- MASWE: [0001](https://mas.owasp.org/MASWE-0001),
  [0034](https://mas.owasp.org/MASWE-0034),
  [0036](https://mas.owasp.org/MASWE-0036),
  [0066](https://mas.owasp.org/MASWE-0066).
