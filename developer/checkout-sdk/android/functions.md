# Functions

Currently, the SDK offers a single [function ](functions.md#checkout.init)that serves as the entry point for the merchant's application. It also includes [callbacks](functions.md#callbacks) that must be managed by the parent app, which are detailed in the following [section](functions.md#callbacks).

## [**Checkout.init**](https://docs.ottu.com/developer/checkout-sdk/web#checkout.init)

The function initiates the checkout process and configures the required settings for the Checkout SDK. It should be invoked once by the parent app to start the checkout sequence, called with set of configuration fields that encapsulate all essential options for the process.

When you call `Checkout.init`, the SDK manages the setup of key components for the checkout, like generating a form for customers to input their payment information, and facilitating the communication with Ottu's servers to process the payment.

This function returns a `Fragment` object, which is a native Android UI component that can be integrated into any part of an `Activity` instance (also native to Android).

## [Properties](functions.md#properties) <a href="#properties" id="properties"></a>

#### [**merchantId**](functions.md#merchantid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `merchant_id` identifies your Ottu merchant domain. It should be the root domain of your Ottu account, excluding the "https://" or "http://" prefix.

For instance, if your Ottu URL is https://example.ottu.com, then your `merchant_id` would be example.ottu.com. This attribute is utilized to determine which Ottu merchant account to associate with the checkout process.

#### [**apiKey**](functions.md#apikey-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `apiKey` is your Ottu [API public key](../../authentication.md#public-key), which is essential for authenticating communications with Ottu's servers during the checkout process.

{% hint style="warning" %}
Make sure to use the public key and avoid using the private key. The [API private key](../../authentication.md#private-key-api-key) must be kept confidential at all times and should never be shared with any clients.
{% endhint %}

#### [**sessionId**](functions.md#sessionid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `session_id` serves as the unique identifier for the payment transaction linked to the checkout process.

This identifier is automatically generated at the creation of the payment transaction. For additional details on how to utilize the `session_id` parameter in the [Checkout AP](../../checkout-api.md)I, refer to the [session\_id](functions.md#sessionid-string-required) section.&#x20;

#### [**formsOfPayment**](functions.md#formsofpayment-string-required) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `formsOfPayment` parameter allows customization of the payment methods displayed in the [checkout process](../#ottu-checkout-sdk-flow). By default, all forms of payment are enabled.

**Available Options for** `formsOfPayment`

* **`cardOnsite`**: A direct payment method (onsite checkout) where cardholder data (CHD) is entered directly in the SDK. If 3DS authentication is required, a payment provider is involved.
* `tokenPay`: Uses [tokenization](../../tokenization.md) to securely store and process customers' payment information.
* `redirect`: Redirects customers to an external [payment gateway](../../../user-guide/payment-gateway.md#payment-gateway-features-summary) or a third-party payment processor to complete the transaction.
* `StcPay`: Requires customers to enter their mobile number and authenticate with an OTP sent to their device to complete the payment.
* `flexMethods`: Allows payments to be split into multiple installments. These methods, also known as BNPL (Buy Now, Pay Later), support providers such as Tabby and Tamara.

#### [**setupPreload**](functions.md#setuppreload-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `ApiTransactionDetails` class object stores transaction details.

If this object is provided, the SDK will not need to retrieve transaction details from the backend, thereby reducing processing time and improving efficiency.

#### [**theme**](functions.md#theme-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `Theme` class object is used for UI customization, allowing modifications to background colors, text colors, and fonts for various components.

All fields in the `Theme` class are optional. If a theme is not specified, the default UI settings will be applied. For more details, refer to the [Customization Theme](functions.md#customization-theme) section.

#### [**displaySettings**](functions.md#displaysettings-object-optional)  _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `PaymentOptionsDisplaySettings` struct is used to configure how payment options are displayed. More details can be found in the [Payment Options Display Mode](functions.md#payment-options-display-mode) section.

#### [**successCallback, errorCallback and successCallback**](functions.md#successcallback-errorcallback-and-successcallback-uint-required) _<mark style="color:blue;">`Uint`</mark>_ _<mark style="color:red;">`required`</mark>_

Callback functions are used to retrieve the payment status and must be provided directly to the Checkout initialization function. For more details, refer to the [Callbacks](functions.md#callbacks) section.
