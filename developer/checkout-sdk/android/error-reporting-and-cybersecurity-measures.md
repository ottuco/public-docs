# Error Reporting & Cybersecurity Measures

## [Error Reporting](error-reporting-and-cybersecurity-measures.md#error-reporting) <a href="#error-reporting" id="error-reporting"></a>

The SDK utilizes Sentry for error logging and reporting, with initialization based on the configuration provided by SDK Studio.

Since the SDK is embedded within the merchant's application, conflicts may arise if Sentry is also integrated into the app. To prevent such conflicts, Sentry can be disabled within the Checkout SDK by setting the `is_enabled` flag to `false` in the configuration.

## [Cyber Security Measures](error-reporting-and-cybersecurity-measures.md#cyber-security-measures) <a href="#cyber-security-measures" id="cyber-security-measures"></a>

### [Rooting Detection](error-reporting-and-cybersecurity-measures.md#rooting-detection)

The SDK prevents execution on rooted devices.

To enforce this restriction, rooting checks are performed during SDK initialization. If a device is detected as rooted, a modal alert dialog is displayed, providing an explanation. The message shown is as follows:

{% hint style="danger" %}
This device is not secure for payments. Transactions are blocked for security reasons.
{% endhint %}

After dismissing the alert, the app crashes unexpectedly

{% hint style="info" %}
&#x20;`Checkout.init` function needs to be called in a coroutine.
{% endhint %}

### [Screen Capture Prevention](error-reporting-and-cybersecurity-measures.md#screen-capture-prevention)

The SDK is designed to protect sensitive data by restricting screen capture functionalities. These restrictions apply to the entire Activity that contains the SDK and operate as follows:

* **Screenshot Attempts:**\
  If a user attempts to take a screenshot, a toast message will appear stating:\
  &#xNAN;_"This app doesn’t allow screenshots."_
* **Screen Recording After SDK Initialization:**\
  If screen recording is initiated **after** the SDK has been initialized, the following toast message is displayed:\
  &#xNAN;_"Can't record screen due to security policy."_
* **Screen Recording Before SDK Initialization:**\
  If screen recording begins **before** the SDK is initialized, the entire Activity containing the SDK will appear as a black screen in the recorded video.
