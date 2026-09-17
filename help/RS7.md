# RS7 - Missing emulator and hostile-device detection

## Threat

Attackers can analyze and automate the app in an emulator, virtual device, or
other controlled environment when environment detection or attestation is
absent or too weak.

## How This Applies

The app displays `Build.FINGERPRINT` for the attacker but never detects or
reacts to emulators, rooted devices, instrumentation, or an untrusted
runtime. The fraud workflow remains fully available.

## Example Attack

Run the app in an Android emulator, observe the generic emulator fingerprint,
and perform the same investigation and approval as on a physical device.

## Mitigations

Use risk-appropriate Play Integrity checks or equivalent server-side attestation,
combine signals rather than trusting a single local check, and require a
trusted environment for high-risk operations.

## MASTG and MASWE references

- MASTG tests: [0351](https://mas.owasp.org/MASTG-TEST-0351),
  [0367](https://mas.owasp.org/MASTG-TEST-0367).
- MASTG best practices: [0046](https://mas.owasp.org/MASTG-BEST-0046),
  [0053](https://mas.owasp.org/MASTG-BEST-0053).
- MASTG knowledge: [0031](https://mas.owasp.org/MASTG-KNOW-0031),
  [0135](https://mas.owasp.org/MASTG-KNOW-0135).
- MASWE: [0053](https://mas.owasp.org/MASWE-0053),
  [0054](https://mas.owasp.org/MASWE-0054).

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

The app runs fully on the iPhone 8 simulator and does not detect or restrict simulators, jailbroken devices, or hostile environments.
