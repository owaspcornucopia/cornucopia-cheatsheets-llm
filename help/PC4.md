# PC4 - Excessive permissions and entitlements

## Threat

An application or embedded component can access more device resources and data
than it needs because permissions are excessive, unexplained, or unjustified.

## How This Applies

The fraud app needs network access and a local database. Its manifest also
declares location, camera, microphone, media-library, and notification
permissions. None of those permissions supports any visible features.

## Example Attack

Inspect the merged manifest or the installed package with the Android `apkanalyzer` and
compare the declared permissions with the actual fraud workflow. A compromised
embedded component can request or use the exposed capabilities.

## Mitigations

Remove unused declarations, apply least privilege, request dangerous
permissions only at the point of need, explain the purpose to the user, and
review transitive SDK permissions before release.

## MASTG and MASWE references

- MASTG tests: [0254](https://mas.owasp.org/MASTG-TEST-0254),
  [0255](https://mas.owasp.org/MASTG-TEST-0255),
  [0256](https://mas.owasp.org/MASTG-TEST-0256),
  [0257](https://mas.owasp.org/MASTG-TEST-0257),
  [0360](https://mas.owasp.org/MASTG-TEST-0360),
  [0361](https://mas.owasp.org/MASTG-TEST-0361),
  [0362](https://mas.owasp.org/MASTG-TEST-0362),
  [0363](https://mas.owasp.org/MASTG-TEST-0363).
- MASTG best practice: [0051](https://mas.owasp.org/MASTG-BEST-0051).
- MASWE: [0066](https://mas.owasp.org/MASWE-0066).
