# NSJ - Cleartext network traffic

## Threat

Attackers can intercept or modify application data in transit when the app
permits cleartext traffic or accepts deprecated network protections.

## How This Applies

This card is not applicable to the Android scenario. The APK does not request
the Internet permission and has no host model endpoint. TinyLlama inference,
prompt handling, SQL generation, and summarization all run inside the Android
application process.

## Example Attack

There is no model request for an emulator proxy, packet capture, or compromised
Wi-Fi network to intercept. Network testing remains relevant to other
applications, but it is outside of this scenario.

## Mitigations

Use HTTPS with modern TLS, disable cleartext traffic in every build, validate
certificates and hostnames, use Network Security Configuration deliberately,
and authenticate, authorize, validate and filter model responses
before treating them as tool instructions.

## MASTG and MASWE references

- MASTG tests: [0217](https://mas.owasp.org/MASTG-TEST-0217),
  [0218](https://mas.owasp.org/MASTG-TEST-0218),
  [0233](https://mas.owasp.org/MASTG-TEST-0233),
  [0235](https://mas.owasp.org/MASTG-TEST-0235),
  [0236](https://mas.owasp.org/MASTG-TEST-0236),
  [0295](https://mas.owasp.org/MASTG-TEST-0295),
  [0321](https://mas.owasp.org/MASTG-TEST-0321),
  [0322](https://mas.owasp.org/MASTG-TEST-0322),
  [0323](https://mas.owasp.org/MASTG-TEST-0323),
  [0342](https://mas.owasp.org/MASTG-TEST-0342),
  [0343](https://mas.owasp.org/MASTG-TEST-0343),
  [0344](https://mas.owasp.org/MASTG-TEST-0344),
  [0345](https://mas.owasp.org/MASTG-TEST-0345),
  [0348](https://mas.owasp.org/MASTG-TEST-0348).
- MASTG best practices: [0020](https://mas.owasp.org/MASTG-BEST-0020),
  [0042](https://mas.owasp.org/MASTG-BEST-0042),
  [0043](https://mas.owasp.org/MASTG-BEST-0043).
- MASWE: [0026](https://mas.owasp.org/MASWE-0026),
  [0027](https://mas.owasp.org/MASWE-0027).

## IOS implementation

This card is not applicable to the native iOS AI Anti Fraud 3.0 scenario.
