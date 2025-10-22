# Error Reporting & Cybersecurity Measures

## [**Error Reporting**](error-reporting-and-cybersecurity-measures.md#error-reporting)

The SDK utilizes Sentry for error logging and reporting. It is initialized based on the configuration provided by SDK Studio.

However, since the SDK is a framework embedded within the merchant's app, conflicts may arise if the app also integrates Sentry.

To prevent conflicts, the merchant can disable Sentry within the Checkout SDK by setting the `is_enabled` flag to `false` in the configuration inside the SDK studio.

## [**Cyber Security Measures**](error-reporting-and-cybersecurity-measures.md#cyber-security-measures)

### **Rooting and Jailbreak Detection**

The **Flutter SDK** does **not** perform **rooting** or **jailbreak detection** independently. Instead, these security checks are entirely handled by the **native SDKs**.

For more details, refer to the following links:

[Android  ](../android/#rooting-detection)

[iOS](../ios/#jailbreak-detection)

### Screen Capture Prevention

The SDK includes mechanisms to prevent screen capturing (such as screenshots and video recordings) on screens that display sensitive information. The Flutter SDK does not handle this independently; instead, it relies on the logic implemented in the native SDKs for Android and iOS.

Since the implementation differs between the two platforms, please refer to the respective native documentation for more details.

[Android](../android/#screen-capture-prevention)&#x20;

[iOS](../ios/#screen-capture-prevention)
