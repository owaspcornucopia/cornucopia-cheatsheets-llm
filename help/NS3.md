# NS3 - Clipboard exposure

## Threat

Sensitive fields can leak through the clipboard or keyboard cache when the
application does not restrict or clear them promptly.

## How This Applies

The **Copy complete result to clipboard** button places the full model answer,
SQL, transaction rows, and memo ciphertext in a normal system clipboard clip.
The app never marks the clip sensitive and never clears it.

## Example Attack

Run an investigation, tap the copy button, and read the clipboard from another
application or with a debugging tool.

## Mitigations

Do not copy sensitive data. If copying is unavoidable, copy only a masked value,
mark the clip as sensitive, use a short expiration, clear it when the workflow
ends, and disable keyboard learning for sensitive input fields.

## MASTG and MASWE references

- MASTG tests: [0258](https://mas.owasp.org/MASTG-TEST-0258),
  [0276](https://mas.owasp.org/MASTG-TEST-0276),
  [0277](https://mas.owasp.org/MASTG-TEST-0277),
  [0278](https://mas.owasp.org/MASTG-TEST-0278),
  [0279](https://mas.owasp.org/MASTG-TEST-0279),
  [0280](https://mas.owasp.org/MASTG-TEST-0280),
  [0313](https://mas.owasp.org/MASTG-TEST-0313),
  [0314](https://mas.owasp.org/MASTG-TEST-0314).
- MASTG best practices: [0019](https://mas.owasp.org/MASTG-BEST-0019),
  [0026](https://mas.owasp.org/MASTG-BEST-0026).
- MASWE: [0036](https://mas.owasp.org/MASWE-0036).
