[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# PCJ — WebView script injection through unsafe deep links

## Threat

Xavier can inject scripts into the web view because it allows embedding content using deep linking without proper authorization and validation of the host, schema and path of the target as these can be changed by the user or because safe browsing is disabled.

## Applicability

PCJ is not applicable to the current Android or iOS scenarios.
The attack requires a WebView or `WKWebView` that loads content selected or influenced by a deep link. Neither scenario embeds an in-app browser or renders attacker-controlled HTML and JavaScript.

## Android-specific scenario

The Android app uses a native `Activity`, an on-device model, and SQLite.
Its manifest declares a launcher activity but no `VIEW`/`BROWSABLE` URI handler.
The app does not create a `WebView`, `WebViewClient`, JavaScript bridge, or HTML/URL loading path.

The Android intent and IPC inputs therefore reach native investigation and database operations, not web content. A crafted input may be relevant to other IPC or input-validation cards, but it cannot inject JavaScript into a WebView that is not present.

## Example attack

There is no executable PCJ attack in this build. A malicious URI cannot place an attacker page in an app WebView, change a WebView navigation target, or exploit a JavaScript bridge because none of those components exist.

The absence of a WebView does not make every incoming value safe. Native authorization, SQL, file, and IPC behavior must be assessed under their corresponding cards rather than described as WebView script injection.

## Mitigations

No PCJ-specific mitigation is required for the current implementation.
If a future feature adds a WebView, allowlist the scheme, host, and path before loading content; validate the source and all parameters; keep Safe Browsing enabled; disable unnecessary file access and JavaScript bridges; and open untrusted external links in the system browser.

## References

- [OWASP MASTG WebView testing](https://mas.owasp.org/MASTG/tests/android/MASVS-PLATFORM/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
- MASTG tests: [0332](https://mas.owasp.org/MASTG-TEST-0332) for
  attacker-controlled WebView URIs; [0370](https://mas.owasp.org/MASTG-TEST-0370)
  and [0371](https://mas.owasp.org/MASTG-TEST-0371) for custom URL scheme
  validation; [0394](https://mas.owasp.org/MASTG-TEST-0394) and
  [0395](https://mas.owasp.org/MASTG-TEST-0395) for Android and iOS deep-link
  validation; and [0398](https://mas.owasp.org/MASTG-TEST-0398),
  [0399](https://mas.owasp.org/MASTG-TEST-0399), and
  [0400](https://mas.owasp.org/MASTG-TEST-0400) for WebView navigation and Safe
  Browsing checks.
- MASTG best practices: [0034](https://mas.owasp.org/MASTG-BEST-0034) for
  WebView input validation, [0054](https://mas.owasp.org/MASTG-BEST-0054) and
  [0055](https://mas.owasp.org/MASTG-BEST-0055) for custom URL handlers, and
  [0070](https://mas.owasp.org/MASTG-BEST-0070),
  [0071](https://mas.owasp.org/MASTG-BEST-0071), and
  [0072](https://mas.owasp.org/MASTG-BEST-0072) for verified links and input
  validation.
- MASTG knowledge: [0018](https://mas.owasp.org/MASTG-KNOW-0018) and
  [0076](https://mas.owasp.org/MASTG-KNOW-0076) on WebViews,
  [0079](https://mas.owasp.org/MASTG-KNOW-0079) on custom URL schemes,
  [0019](https://mas.owasp.org/MASTG-KNOW-0019) on deep links, and
  [0080](https://mas.owasp.org/MASTG-KNOW-0080) on universal links.
- MASWE: [0035](https://mas.owasp.org/MASWE-0035) for WebViews loading
  untrusted content, [0034](https://mas.owasp.org/MASWE-0034) for WebViews
  exposing local resources, and [0029](https://mas.owasp.org/MASWE-0029) for
  insecure deep links.

## IOS implementation

The app uses a native SwiftUI equivalent of the mobile behavior described above, not an embedded browser.

### What can go wrong

The iOS app registers `pwnednext://` and passes incoming URLs to its native `handle(url:)` method. That handler can influence native question, SQL, file, and approval state, but it does not construct a `WKWebView` or load remote or local HTML. Consequently, the PCJ script-injection path cannot be reached.

### What to do

No PCJ-specific iOS control is needed while the application remains native SwiftUI. If a WebView is introduced, apply the MASTG, MASVS, and MASWE controls above before allowing deep-link values to select content. 
Continue to assess the existing custom URL handler under the applicable IPC and input validation cards.

### IOS details

`onOpenURL` forwards `pwnednext://` values to native application logic. There is no `WKWebView`, JavaScript bridge, HTML loader, or Safe Browsing setting in the scenario.
