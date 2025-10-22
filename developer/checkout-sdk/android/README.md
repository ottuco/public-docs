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
