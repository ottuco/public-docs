# FAQ

#### :digit\_one: [What forms of payments are supported by the SDK?](faq.md#id-1.-what-forms-of-payments-are-supported-by-the-sdk) <a href="#id-1.-what-forms-of-payments-are-supported-by-the-sdk" id="id-1.-what-forms-of-payments-are-supported-by-the-sdk"></a>

The SDK supports the following payment forms: `tokenPay`, `ottuPG`, `redirect` `applePay` and `stcPay`. Merchants can display specific methods according to their needs.

**For example,** if you want to only show the STC Pay button, you can do so using formsOfPayment = `stcPay`, and only the STC Pay button will be displayed. The same applies for `applePay` and other methods.

#### :digit\_two: [What are the minimum system requirements for the SDK integration?](faq.md#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration) <a href="#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration" id="id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration"></a>

It is required to have a device running iOS 13 or higher.

#### :digit\_three: [Can I customize the appearance beyond the provided themes?](faq.md#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes) <a href="#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes" id="id-3.-can-i-customize-the-appearance-beyond-the-provided-themes"></a>

Yes, see the [Customization theme](faq.md#customization-theme) section.

#### :digit\_four: [How do I customize the payment request for Apple Pay?](faq.md#id-4.-how-do-i-customize-the-payment-request-for-apple-pay) <a href="#id-4.-how-do-i-customize-the-payment-request-for-apple-pay" id="id-4.-how-do-i-customize-the-payment-request-for-apple-pay"></a>

The payment request for Apple Pay can be customized using its initialization methods. These methods allow the configuration of various properties, including:

* API version
* Supported card types
* Accepted networks
* Applicable countries
* Merchant capabilities

For a complete list of supported properties, refer to the [Apple Pay](https://developer.apple.com/documentation/apple_pay_on_the_web/applepaypaymentrequest) documentation.
