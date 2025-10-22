# Flutter

The Checkout SDK is a Flutter framework (library) developed by Ottu, designed to seamlessly integrate an Ottu-powered [checkout process](../#ottu-checkout-sdk-flow) into Flutter applications for both iOS and Android platforms. This framework functions as a wrapper over the corresponding native SDKs, ensuring a smooth and efficient payment experience.

With the Checkout SDK, both the visual appearance and the forms of payment available during the checkout process can be fully customized to meet specific requirements.

To integrate the Checkout SDK, the library must be added to the Flutter application and initialized with the following parameters:

* [merchant\_id](../web.md#merchant_id-string)
* [session\_id](../../checkout-api.md#session_id-string-mandatory)
* [API key](../../authentication.md#public-key)

Additionally, optional configurations such as the [forms of payment](./#formsofpayment-array-optional) to be accepted and the [theme](./#customization-theme) styling for the checkout interface can be specified.
