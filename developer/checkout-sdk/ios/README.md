# iOS

The [Checkout SDK](../) is a Swift framework (library) provided by Ottu, designed to facilitate the seamless integration of an Ottu-powered checkout process into iOS applications.

With the Checkout SDK, both the visual appearance and the forms of payment available during the [checkout process](../#ottu-checkout-sdk-flow) can be fully customized.

To integrate the Checkout SDK, the library must be included in the iOS application and initialized with the following parameters:

* [merchant\_id](https://app.gitbook.com/s/su3y9UFjvaXZBxug1JWQ/#merchant_id-string)
* [session\_id](../../checkout-api.md#session_id-string-mandatory)
* [API public key](../../authentication.md#public-key)

Additionally, optional configurations such as the [forms of payment](./#formsofpayment-array-optional) to accept and the [theme](./#customization-theme) styling for the checkout interface can be specified.

{% hint style="warning" %}
[API private key](../../authentication.md#private-key-api-key) should never be used on the client side. Instead, [API public key ](../../authentication.md#public-key)should be used. This is essential to ensure the security of your application and the protection of sensitive data.
{% endhint %}
