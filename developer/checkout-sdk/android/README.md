# Android

The [Checkout SDK](../) from Ottu is a Kotlin-based library designed to streamline the integration of an Ottu-powered [checkout process](../#ottu-checkout-sdk-flow) into Android applications. This SDK allows for complete customization of the checkout experience, including both appearance and functionality, as well as the selection of accepted payment methods.

To integrate the Checkout SDK, it must be incorporated into the Android application and initialized with the following parameters:

* [merchant\_id](https://app.gitbook.com/s/su3y9UFjvaXZBxug1JWQ/#merchant_id-string)
* [session\_id](./#sessionid-string-required)
* [API public key](../../authentication.md#public-key)

Additionally, various configuration options, such as accepted [payment methods](./#formsofpayment-array-optional) and [theme ](./#customization-theme)styling for the checkout interface, can be specified to enhance the user experience.

{% hint style="warning" %}
The [API private key](../../authentication.md#private-key-api-key) should never be utilized on the client side; instead, use the [API public key](../../authentication.md#public-key). This is essential for maintaining the security of your application and safeguarding sensitive data.
{% endhint %}

## [Payment Options Display Mode](./#payment-options-display-mode) <a href="#payment-options-display-mode" id="payment-options-display-mode"></a>

The display of payment options can be adjusted using the SDK with the following settings:

* `mode` (`BottomSheet` or List)
* `visibleItemsCount` (default is 5)
* `defaultSelectedPgCode` (default is none)

By default, **BottomSheet mode** is used, as was implemented in previous releases. **List mode** is a new option that allows the list of payment methods to be displayed above the **Payment Details** section and the **Pay** button.

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

**A view with a selected item:**

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

**A view with an expanded list:**

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* `visibleItemsCount` is an unsigned integer that sets the number of items to be displayed at once. It works only in List mode. If the number of available payment options is fewer than this value, the list height is automatically adjusted to the minimum required.

{% hint style="info" %}
If 0 is passed, an exception is thrown by the SDK, which must be handled by the parent app.
{% endhint %}

* `defaultSelectedPgCode` is a PG code that will be automatically selected. The SDK searches for the payment option with the matching PG code and selects it if found. If no match is found, no selection is made.

All these parameters are optional and are constructed using the following function:

{% code overflow="wrap" %}
```swift
private fun getPaymentOptionSettings(): Checkout.PaymentOptionsDisplaySettings {
    val visibleItemsCount = 5 // set needed value here
    val selectedPgCode = "knet" // set needed value here
    val mode = Checkout.PaymentOptionsDisplaySettings.PaymentOptionsDisplayMode.List(visibleItemsCount)
    return Checkout.PaymentOptionsDisplaySettings(mode, selectedPgCode)
}
```
{% endcode %}

These parameters are passed to the `Checkout.init` builder class via the following object:

```swift
.paymentOptionsDisplaySettings(paymentOptionsDisplaySettings)
```

To view the full function call, please refer to the [Ottu SDK - Android | Example](./#example) chapter in the documentation.

## [STC Pay](./#stc-pay) <a href="#stc-pay" id="stc-pay"></a>

Once the STC Pay integration between Ottu and STC Pay has been completed, the necessary checks are automatically handled by the Checkout SDK to ensure the seamless display of the STC Pay button.

Upon initialization of the Checkout SDK with the [session\_id](./#sessionid-string-required) and payment gateway codes ([pg\_codes](../../checkout-api.md#pg_codes-array-required)), the following condition is automatically verified:

* The `session_id` and pg\_codes provided during SDK initialization must be linked to the STC Pay Payment Service. This verification ensures that the STC Pay option is made available for selection as a payment method.

Regardless of whether a mobile number has been entered by the customer during transaction creation, the STC Pay button is displayed by the Android SDK.

## [Onsite Checkout](./#onsite-checkout)

This payment option facilitates direct payments within the mobile SDK. A user-friendly interface allows users to securely input their CHD. If permitted by the backend, the card can be stored as a tokenized payment for future transactions.

**Example:**

<figure><img src="../../../.gitbook/assets/image (96).png" alt="" width="371"><figcaption></figcaption></figure>

The SDK supports multiple instances of onsite checkout payments. Therefore, for each payment method with a PG code of `ottu_pg`, the card form (as shown above) will be displayed.

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Fees are not displayed for onsite checkout instances due to the support of multiple card types through omni PG (multi-card) configurations. The presence of multiple payment icons also indicates this multi-card functionality.
{% endhint %}

## [Error Reporting](./#error-reporting) <a href="#error-reporting" id="error-reporting"></a>

The SDK utilizes Sentry for error logging and reporting, with initialization based on the configuration provided by SDK Studio.

Since the SDK is embedded within the merchant's application, conflicts may arise if Sentry is also integrated into the app. To prevent such conflicts, Sentry can be disabled within the Checkout SDK by setting the `is_enabled` flag to `false` in the configuration.

## [Cyber Security Measures](./#cyber-security-measures) <a href="#cyber-security-measures" id="cyber-security-measures"></a>

### [Rooting Detection](./#rooting-detection)

The SDK prevents execution on rooted devices.

To enforce this restriction, rooting checks are performed during SDK initialization. If a device is detected as rooted, a modal alert dialog is displayed, providing an explanation. The message shown is as follows:

{% hint style="danger" %}
This device is not secure for payments. Transactions are blocked for security reasons.
{% endhint %}

After dismissing the alert, the app crashes unexpectedly

{% hint style="info" %}
&#x20;`Checkout.init` function needs to be called in a coroutine.
{% endhint %}

### [Screen Capture Prevention](./#screen-capture-prevention)

The SDK is designed to protect sensitive data by restricting screen capture functionalities. These restrictions apply to the entire Activity that contains the SDK and operate as follows:

* **Screenshot Attempts:**\
  If a user attempts to take a screenshot, a toast message will appear stating:\
  &#xNAN;_"This app doesn’t allow screenshots."_
* **Screen Recording After SDK Initialization:**\
  If screen recording is initiated **after** the SDK has been initialized, the following toast message is displayed:\
  &#xNAN;_"Can't record screen due to security policy."_
* **Screen Recording Before SDK Initialization:**\
  If screen recording begins **before** the SDK is initialized, the entire Activity containing the SDK will appear as a black screen in the recorded video.

## [FAQ](./#faq) <a href="#faq" id="faq"></a>

#### :digit\_one: [What forms of payments are supported by the SDK?](./#id-1.-what-forms-of-payments-are-supported-by-the-sdk) <a href="#id-1.-what-forms-of-payments-are-supported-by-the-sdk" id="id-1.-what-forms-of-payments-are-supported-by-the-sdk"></a>

The SDK accommodates various payment forms including`tokenPay`, `redirect`, `StcPay` and `cardOnsite`.&#x20;

Merchants have the flexibility to showcase specific methods based on their requirements.&#x20;

For instance, if you wish to exclusively display the STC Pay button, you can achieve this by setting `formsOfPayment` = `[StcPay]`, which will result in only the STC Pay button being displayed. This approach is applicable to other payment methods as well.

#### :digit\_two: [What are the minimum system requirements for the SDK integration?](./#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration) <a href="#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration" id="id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration"></a>

It is required to have a device running Android 8 or higher (Android API level 26 or higher).

#### :digit\_three: [Can I customize the appearance beyond the provided themes?](./#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes) <a href="#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes" id="id-3.-can-i-customize-the-appearance-beyond-the-provided-themes"></a>

Yes, check the [Customization theme](./#customization-theme) section.
