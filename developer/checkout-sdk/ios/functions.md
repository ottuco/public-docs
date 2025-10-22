# Functions

The SDK currently provides a single function, serving as the entry point for the merchant's application.

Additionally, callbacks are provided and must be handled by the parent application. These callbacks are described [here](functions.md#callbacks).

## [**Checkout.init**](functions.md#checkout.init)

The `Checkout.init` function is responsible for initializing the checkout process and configuring the necessary settings for the Checkout SDK.

It must be called once by the parent application and provided with a set of configuration fields that define all the required options for the checkout process.

When `Checkout.init` is invoked, the SDK automatically sets up the essential components, including:

* Generating a form for the customer to enter their payment details.
* Handling communication with Ottu's servers to process the payment.

This function returns a `View` object, which is a native iOS UI component. It can be embedded within any `ViewController` instance in the application.

## [Properties](functions.md#properties) <a href="#properties" id="properties"></a>

#### [**merchantId**](functions.md#merchantid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The **`merchant_id`** specifies the **Ottu merchant domain** and must be set to the **root domain** of the **Ottu account**, excluding the **"https://"** or **"http://"** prefix.

For example, if the **Ottu URL** is **`https://example.ottu.com`**, then the corresponding **`merchant_id`** is **`example.ottu.com`**.

This parameter is used to **link** the **checkout process** to the appropriate **Ottu merchant account**.

#### [**apiKey**](functions.md#apikey-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `apiKey` is the Ottu API [public key](../../authentication.md#public-key), used for authentication when communicating with Ottu's servers during the checkout process.

{% hint style="warning" %}
Only the public key should be used. The private key must remain confidential at all times and must not be shared with any clients.
{% endhint %}

#### [**sessionId**](functions.md#sessionid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `session_id` is a unique identifier assigned to the payment transaction associated with the checkout process.

This identifier is automatically generated when the payment transaction is created.

For more details on how to use the `session_id` parameter in the Checkout API, refer to the [session\_id](functions.md#sessionid-string-required).

#### [**formsOfPayment**](functions.md#formsofpayment-string-required) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The forms of payment displayed in the [checkout process](../#ottu-checkout-sdk-flow) can be customized using `formsOfPayment`. By default, all forms of payment are enabled.

Available options for `formsOfPayment`:

* `applePay`: Supports Apple Pay, allowing purchases to be made using Apple Pay-enabled devices.
* `stcPay`: Requires customers to enter their mobile number and authenticate with an OTP sent to their device to complete the payment.
* `cardOnsite`: Enables direct payments (onsite checkout), where Cardholder Data (CHD) is entered directly in the SDK. If 3DS authentication is required, a payment provider is involved in the process.
* `tokenPay`: Uses [tokenization](../../tokenization.md) to securely store and process customers' payment information.
* `redirect`: Redirects customers to an external [payment gateway](../../../user-guide/payment-gateway.md#payment-gateway-features-summary) or a third-party payment processor to complete the transaction.

#### [**setupPreload**](functions.md#setuppreload-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An `ApiTransactionDetails` struct object is used to store transaction details.

If provided, the SDK will not request transaction details from the backend, reducing processing time and improving efficiency.

#### [**theme**](functions.md#theme-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `Theme` struct object is used for UI customization, allowing modifications to background colors, text colors, and fonts for various components. It supports customization for both light and dark device modes. All fields in the `Theme` struct are optional. If a theme is not provided, the default UI settings will be applied. For more details, refer to the [Customization Theme](functions.md#customization-theme) section.

#### [displaySettings](functions.md#displaysettings-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The display of payment options is configured using the `PaymentOptionsDisplaySettings` struct. Additional information is provided in the [Payment Options Display Mode](functions.md#payment-options-display-mode) section.

#### [**delegate**](functions.md#delegate-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:red;">`required`</mark>_

An object is used to provide SDK callbacks to the application. Typically, this is the parent app’s class that conforms to `OttuDelegate`, aggregating the SDK object.

To implement this delegate, the class must define three callback functions. More details are accessible next [secion](functions.md#callbacks).

## [Payment Options Display Mode](functions.md#payment-options-display-mode)

The payment options display can be adjusted using the SDK with the following settings:

* `mode` (`BottomSheet` or `List`)
* `visibleItemsCount` (default is 5)
* `defaultSelectedPgCode` (default is empty)

By default, **BottomSheet mode** is used, as set in previous releases. **List mode** is a new option, where a list of payment methods is displayed above the **Payment Details** section and the **Pay** button.

<figure><img src="../../../.gitbook/assets/image (99).png" alt="" width="375"><figcaption></figcaption></figure>

**A view with a selected item**

<figure><img src="../../../.gitbook/assets/image (100).png" alt="" width="375"><figcaption></figcaption></figure>

**A view with an expanded list:**

<figure><img src="../../../.gitbook/assets/image (101).png" alt="" width="375"><figcaption></figcaption></figure>

* **`visibleItemsCount`** is an unsigned integer that sets how many payment options are shown at once. It only works in **List mode**. If there are fewer options than the number set, the list height will adjust to show only the available options.

{% hint style="info" %}
&#x20;If 0 is set, the SDK will throw an exception that the parent app must handle.
{% endhint %}

* **`defaultSelectedPgCode`** is a payment gateway (PG) code that will be automatically selected. The SDK will look for the matching payment option and select it if found. If not found, no payment option will be selected.

{% hint style="info" %}
If there are many payment options, the total height of the payment list, the **Payment Details** section, and the **Pay** button may exceed the screen size. The SDK doesn't support vertical scrolling, so the parent app must handle it. You can refer to the demo app's source code for guidance.
{% endhint %}

All these parameters are optional and can be passed to `Checkout.init` through the following object:

```swift
displaySettings: PaymentOptionsDisplaySettings(
  mode: paymentOptionsDisplayMode,
  visibleItemsCount: visibleItemsCount,
  defaultSelectedPgCode: defaultSelectedPgCode
)
```

To view the full function call, please refer to the [Ottu SDK - iOS | Example](functions.md#example) chapter in the documentation. This section provides the complete example, including how the function is used in the context of the Ottu SDK.
