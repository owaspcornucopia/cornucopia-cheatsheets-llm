[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# PCX — Outdated platforms and untrusted components

## Threat

Johan can modify or expose sensitive data by exploiting outdated platforms, SDKs, third-party dependencies, or WebView components when supported versions, trusted components, and security updates are not enforced.

## Android-specific scenario

The Android project compiles and targets API 35 but permits installation from API 26. Android API 36 is the current stable platform, and Google Play's current policy requires API 36 for new apps and updates. The project has no runtime minimum-version policy or enforced-update workflow.

This means that a future distributed build would be able to run on devices that lack newer platform protections. The scenario does not use a WebView, so this is a platform issue rather than a WebView issue.

## Example attack

1. Install the app on an Android device that remains on an old, unpatched platform permitted by the API 26 minimum.
2. Exploit a platform weakness or missing security protection that a newer supported platform would provide.
3. Continue using the app without an enforced update or minimum security baseline while accessing its local model, database, IPC, and report data.

The project does not state a particular Android API 26 vulnerability, but the supported-version is not enforced against a platform baseline. That could expose users to external threats.

## Mitigations

Define a supported OS and security-patch baseline, raise `minSdk` and `targetSdk` according to that policy, and enforce critical updates through the distribution channel.
Keep compile and target SDKs current, scan the Android and native dependencies for known vulnerabilities, and remove unsupported components. Do not add a WebView without applying its separate security controls.

## References

- [Android SDK platform releases](https://developer.android.com/tools/releases/platforms)
- [Google Play target API requirements](https://developer.android.com/google/play/requirements/target-sdk)
- [Apple security releases](https://support.apple.com/en-us/100100)
- MASTG tests: [0245](https://mas.owasp.org/MASTG-TEST-0245) for platform
  version checks; [0272](https://mas.owasp.org/MASTG-TEST-0272),
  [0273](https://mas.owasp.org/MASTG-TEST-0273),
  [0274](https://mas.owasp.org/MASTG-TEST-0274), and
  [0275](https://mas.owasp.org/MASTG-TEST-0275) for dependency vulnerabilities;
  [0382](https://mas.owasp.org/MASTG-TEST-0382),
  [0383](https://mas.owasp.org/MASTG-TEST-0383),
  [0384](https://mas.owasp.org/MASTG-TEST-0384), and
  [0392](https://mas.owasp.org/MASTG-TEST-0392) for enforced updates; and
  [0331](https://mas.owasp.org/MASTG-TEST-0331) for deprecated WebView APIs.
- MASTG best practice: [0032](https://mas.owasp.org/MASTG-BEST-0032) for
  migrating deprecated WebView components.
- MASTG knowledge: [0023](https://mas.owasp.org/MASTG-KNOW-0023) and
  [0074](https://mas.owasp.org/MASTG-KNOW-0074) on enforced updating, and
  [0076](https://mas.owasp.org/MASTG-KNOW-0076) on WebViews.
- MASWE: [0041](https://mas.owasp.org/MASWE-0041) for not ensuring a recent
  platform version, [0044](https://mas.owasp.org/MASWE-0044) for vulnerable
  dependencies, [0043](https://mas.owasp.org/MASWE-0043) for missing enforced
  updates, and [0035](https://mas.owasp.org/MASWE-0035) for WebViews loading
  untrusted content.

## IOS implementation

The app uses a native SwiftUI equivalent of the mobile behavior described above.
Its deployment target is iOS 15.0, and the setup documentation uses an iOS 16.2 Simulator with Xcode 14.2. Apple’s current security releases are much newer, and the project has no minimum-version or enforced-update policy.

### What can go wrong

Users who remain on an old iOS release may lack platform security fixes that the app assumes. A distributed build can continue running there because the deployment target does not require a recent supported release. The app has no WebView, so this scenario does not mean the WebView is vulnerable.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above. Set and review a supported iOS floor, update the Xcode and Swift toolchain, enforce critical updates through the distribution channel, and maintain a dependency inventory where you do scan for security vulnerabilities.

### IOS details

`IPHONEOS_DEPLOYMENT_TARGET = 15.0` is used for both build configurations.
The current Simulator setup is older than the current Apple security-release baseline, so the supported-version should be made explicit before a production distribution.
