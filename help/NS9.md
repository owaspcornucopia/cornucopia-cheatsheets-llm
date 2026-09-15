# NS9 - Tamperable local security state

## Threat

Attackers can change security-relevant preferences, files, or database values
when the app does not verify their integrity before using them.

## How This Applies

The activity trusts the `fraud_override` SharedPreferences value. The value can
also be seeded by an exported intent extra (additional data needed for the action),
so a caller can alter the displayed fraud outcome without server-side authorization.

## Example Attack

```powershell
adb shell am start -n org.owasp.pwnednext.android/.MainActivity `
  --ez fraudOverride true `
  --es question "Is transaction TX-1001 fraudulent?" `
  --ez autoInvestigate true
```

The activity accepts the local override and exposes the modified state.

## Mitigations

Do not treat client-controlled state as authorization. Store security decisions
server-side or protect local state with authenticated encryption and a
device/user-bound key, then fail closed when integrity verification fails.

## MASTG and MASWE references

- MASTG tests: [0338](https://mas.owasp.org/MASTG-TEST-0338),
  [0387](https://mas.owasp.org/MASTG-TEST-0387).
- MASTG best practices: [0065](https://mas.owasp.org/MASTG-BEST-0065),
  [0066](https://mas.owasp.org/MASTG-BEST-0066).
- MASWE: [0057](https://mas.owasp.org/MASWE-0057).
