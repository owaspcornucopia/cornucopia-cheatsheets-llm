# AA9 - Broad component and resource access

## Threat

Attackers can access sensitive data or functionality because component
permissions, entitlements, local resources, or content-provider controls are
too broad or absent.

## How This Applies

The main activity, broadcast receiver, query provider, and file provider are
exported without a signature permission. The providers expose db transactions
and training files to any installed application. 
This means that the internal parts of the mobile app are wide open for any
malicious or third-party app.

## Example Attack

Use `dumpsys package` to enumerate the exported components, then invoke the
receiver, query the transaction provider, or request a file URI without an
application permission.

## Mitigations

Make components private by default, use signature-level permissions for
trusted integrations, validate caller identity and URI grants, minimize exposed
data, and test the merged manifest as part of release review.

## MASTG and MASWE references

- MASTG tests: [0250](https://mas.owasp.org/MASTG-TEST-0250),
  [0251](https://mas.owasp.org/MASTG-TEST-0251),
  [0252](https://mas.owasp.org/MASTG-TEST-0252),
  [0253](https://mas.owasp.org/MASTG-TEST-0253),
  [0254](https://mas.owasp.org/MASTG-TEST-0254),
  [0255](https://mas.owasp.org/MASTG-TEST-0255),
  [0256](https://mas.owasp.org/MASTG-TEST-0256),
  [0257](https://mas.owasp.org/MASTG-TEST-0257),
  [0335](https://mas.owasp.org/MASTG-TEST-0335),
  [0336](https://mas.owasp.org/MASTG-TEST-0336),
  [0360](https://mas.owasp.org/MASTG-TEST-0360),
  [0361](https://mas.owasp.org/MASTG-TEST-0361),
  [0362](https://mas.owasp.org/MASTG-TEST-0362),
  [0363](https://mas.owasp.org/MASTG-TEST-0363).
- MASTG best practices: [0010](https://mas.owasp.org/MASTG-BEST-0010),
  [0011](https://mas.owasp.org/MASTG-BEST-0011),
  [0012](https://mas.owasp.org/MASTG-BEST-0012),
  [0013](https://mas.owasp.org/MASTG-BEST-0013),
  [0033](https://mas.owasp.org/MASTG-BEST-0033),
  [0049](https://mas.owasp.org/MASTG-BEST-0049),
  [0051](https://mas.owasp.org/MASTG-BEST-0051).
- MASWE: [0034](https://mas.owasp.org/MASWE-0034),
  [0066](https://mas.owasp.org/MASWE-0066).

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

An attacker can use the matching iOS entry point, local storage, process state,
or on-device model flow to expose data or change the fraud investigation.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above. Test
the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and
verify that input validation, authorization, data minimization, integrity, and
protected storage are enforced at the native boundary.

### IOS details

Any app that can open the registered pwnednext:// scheme can invoke the investigation and report workflow.
