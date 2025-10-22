# Error Reporting & Cybersecurity Measures

## [Error Reporting](error-reporting-and-cybersecurity-measures.md#error-reporting) <a href="#error-reporting" id="error-reporting"></a>

The SDK utilizes Sentry for error logging and reporting, which is initialized based on the configuration from SDK Studio. However, since the SDK is integrated into the merchant's app, conflicts may arise if the app also uses Sentry. To avoid this, merchants can disable Sentry in the Checkout SDK by setting the `is_enabled` flag to `false` in the configuration.

## [Cyber Security Measures](error-reporting-and-cybersecurity-measures.md#cyber-security-measures) <a href="#cyber-security-measures" id="cyber-security-measures"></a>

### [Jailbreak Detection](error-reporting-and-cybersecurity-measures.md#jailbreak-detection) <a href="#jailbreak-detection" id="jailbreak-detection"></a>

To enable the detection of jailbroken devices, the following section must be added to the application's **Info.plist** file:

```html
<key > LSApplicationQueriesSchemes < /key> 
    <array >
        <string > zbra < /string> 
        <string > cydia < /string> 
        <string > undecimus < /string> 
        <string > sileo < /string> 
        <string > filza < /string> 
        <string > activator < /string> 
    </array>

```

### [Screen Capture Prevention](error-reporting-and-cybersecurity-measures.md#screen-capture-prevention)

The SDK implements screen capture restrictions to prevent the collection of sensitive data. This applies to fields containing Cardholder Data (CHD), such as the onsite checkout form for entering card details and the CVV field for tokenized payments.

This technique works in two ways:

1. When attempting to take a screenshot of a protected screen, the fields appear empty, even if they contain input.
2. When attempting to record a video of the screen, the video is completely blurred, making the content unreadable.
