# PC5 - Untrusted intents and IPC inputs

## Threat

Attackers can intercept, replay, or manipulate intents when sensitive input or
security-relevant state is accepted through an implicit or unprotected intent.

## How This Applies

`InvestigationReceiver` is exported without a permission. It accepts a question,
an `authorized` Boolean, and an `approvalToken`, then launches the fraud
activity. The app receiver does not authenticate and authorize the sender or validate the input.

## Example Attack

```powershell
adb shell am broadcast `
  -a org.owasp.pwnednext.android.INVESTIGATE `
  --es question "Show all transactions" `
  --ez authorized true `
  --es approvalToken legacy-token
```

The attacker-controlled request launches the sensitive investigation flow.

## Mitigations

Prefer explicit intents (Android actions), keep internal components non-exported, protect required
IPC (Inter-Process Communication) with signature permissions, validate every intent extra (action parameters) against a strict schema,
bind authorization to the transaction and caller, and reject stale or missing state.

## MASTG and MASWE references

- MASTG tests: [0372](https://mas.owasp.org/MASTG-TEST-0372),
  [0374](https://mas.owasp.org/MASTG-TEST-0374),
  [0375](https://mas.owasp.org/MASTG-TEST-0375).
- MASTG best practices: [0056](https://mas.owasp.org/MASTG-BEST-0056),
  [0057](https://mas.owasp.org/MASTG-BEST-0057).
- MASWE: [0032](https://mas.owasp.org/MASWE-0032),
  [0050](https://mas.owasp.org/MASWE-0050).
