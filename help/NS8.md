# NS8 - Inadequate protection of data at rest

## Threat

Attackers can read or modify sensitive data at rest when local storage and its
integrity protections are inadequate.

## How This Applies

The app stores `last_result` and `fraud_override` in ordinary
SharedPreferences. The database also uses the intentionally weak AES/CBC
implementation rather than authenticated, keystore-backed storage.

## Example Attack

After an investigation, pull or inspect the app preferences and edit
`fraud_override=true`. Relaunch or investigate again and observe the
`LOCAL PREFERENCE OVERRIDE ACCEPTED` message.

## Mitigations

Use Android Keystore-backed authenticated encryption, bind integrity to the
authenticated user and transaction, minimize persistence, clear stale values,
and reject modified or unverifiable records rather than accepting them.

## MASTG and MASWE references

- MASTG tests: [0200](https://mas.owasp.org/MASTG-TEST-0200),
  [0201](https://mas.owasp.org/MASTG-TEST-0201),
  [0207](https://mas.owasp.org/MASTG-TEST-0207),
  [0299](https://mas.owasp.org/MASTG-TEST-0299),
  [0300](https://mas.owasp.org/MASTG-TEST-0300),
  [0301](https://mas.owasp.org/MASTG-TEST-0301),
  [0302](https://mas.owasp.org/MASTG-TEST-0302),
  [0303](https://mas.owasp.org/MASTG-TEST-0303),
  [0304](https://mas.owasp.org/MASTG-TEST-0304),
  [0305](https://mas.owasp.org/MASTG-TEST-0305),
  [0306](https://mas.owasp.org/MASTG-TEST-0306),
  [0338](https://mas.owasp.org/MASTG-TEST-0338),
  [0387](https://mas.owasp.org/MASTG-TEST-0387).
- MASTG best practices: [0024](https://mas.owasp.org/MASTG-BEST-0024),
  [0050](https://mas.owasp.org/MASTG-BEST-0050),
  [0065](https://mas.owasp.org/MASTG-BEST-0065),
  [0066](https://mas.owasp.org/MASTG-BEST-0066).
- MASWE: [0001](https://mas.owasp.org/MASWE-0001),
  [0002](https://mas.owasp.org/MASWE-0002),
  [0057](https://mas.owasp.org/MASWE-0057).
