# AA2 - Missing step-up authentication

## Threat

An attacker can perform a sensitive operation without re-authentication when
the app does not consider transaction risk or contextual changes.

## How This Applies

The **Approve latest transaction** button approves a training
transaction without a biometric prompt, device credential, remote check, or
fresh user confirmation.

## Example Attack

Run any investigation and tap **Approve latest transaction**. The app reports
approval even though no step-up authentication occurred.

## Mitigations

Require fresh, transaction-bound authentication for high-risk actions. Prefer a
remote authorization decision and use a Keystore key whose use requires the
appropriate user authentication policy.

## MASTG and MASWE references

- MASTG tests: [0266](https://mas.owasp.org/MASTG-TEST-0266),
  [0267](https://mas.owasp.org/MASTG-TEST-0267),
  [0268](https://mas.owasp.org/MASTG-TEST-0268),
  [0269](https://mas.owasp.org/MASTG-TEST-0269).
- MASTG knowledge: [0056](https://mas.owasp.org/MASTG-KNOW-0056),
  [0057](https://mas.owasp.org/MASTG-KNOW-0057).
- MASWE: [0020](https://mas.owasp.org/MASWE-0020),
  [0021](https://mas.owasp.org/MASWE-0021).
