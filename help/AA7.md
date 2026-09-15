# AA7 - Replayable and out-of-sequence authorization

## Threat

An attacker can bypass protected operations by replaying a valid result,
invoking actions out of sequence, or changing client-controlled state that the
app trusts.

## How This Applies

The app forwards an `authorized` parameter and an `approvalToken`.
The app accepts any non-empty token, including `legacy-token`, and does
not bind it to a user, transaction, time, or one-time use. Once the
check passes, the **Report not fraudulent** button updates the investigated row and
sets its `fraud_detected` database value to `0`.

## Example Attack

Broadcast an investigation with `authorized=true` and
`approvalToken=legacy-token`, investigate `TX-1002`, and tap **Report not fraudulent**.
The app accepts the replayable values and clears `fraud_detected` for the
investigated transaction. Repeating the same broadcast is accepted.

## Mitigations

Perform authorization on a trusted endpoint, bind decisions to the exact
transaction and authenticated user, use nonces and expiration, reject stale
state, and enforce the required operation sequence on the server.

## MASTG and MASWE references

- MASTG tests: [0266](https://mas.owasp.org/MASTG-TEST-0266),
  [0267](https://mas.owasp.org/MASTG-TEST-0267),
  [0327](https://mas.owasp.org/MASTG-TEST-0327),
  [0329](https://mas.owasp.org/MASTG-TEST-0329),
  [0375](https://mas.owasp.org/MASTG-TEST-0375).
- MASTG best practices: [0036](https://mas.owasp.org/MASTG-BEST-0036),
  [0038](https://mas.owasp.org/MASTG-BEST-0038),
  [0057](https://mas.owasp.org/MASTG-BEST-0057).
- MASWE: [0020](https://mas.owasp.org/MASWE-0020),
  [0050](https://mas.owasp.org/MASWE-0050).
