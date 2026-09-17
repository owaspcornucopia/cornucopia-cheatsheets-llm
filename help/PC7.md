# PC7 - Unsanitized content-provider queries

## Threat

An attacker can inject provider query arguments or reach sensitive data because
content-provider methods are exported and query inputs are not parameterized.

## How This Applies

`InvestigationProvider` accepts the caller's `selection` string and concatenates
it into `SELECT * FROM transactions WHERE ...`. `selectionArgs` are ignored.
The provider is exported without a permission.

## Example Attack

```powershell
adb shell content query `
  --uri content://org.owasp.pwnednext.android.investigations/transactions `
  --where "1=1"
```

The caller receives all transaction rows instead of a narrowly scoped transaction result.

## Mitigations

Use a non-exported provider or a signature permission, allow only fixed query
operations, bind values through `selectionArgs`, restrict sql projection and sort
columns, and authorize every record access.

## MASTG and MASWE references

- MASTG tests: [0339](https://mas.owasp.org/MASTG-TEST-0339),
  [0355](https://mas.owasp.org/MASTG-TEST-0355),
  [0356](https://mas.owasp.org/MASTG-TEST-0356),
  [0357](https://mas.owasp.org/MASTG-TEST-0357).
- MASTG best practices: [0039](https://mas.owasp.org/MASTG-BEST-0039),
  [0049](https://mas.owasp.org/MASTG-BEST-0049).
- MASWE: [0018](https://mas.owasp.org/MASWE-0018),
  [0050](https://mas.owasp.org/MASWE-0050).

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

The URL where parameter is interpolated into SELECT * FROM transactions WHERE ... before SQLite execution, so a caller can inject SQL predicates.
