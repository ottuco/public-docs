# iOS

The [Checkout SDK](./) is a Swift framework (library) provided by Ottu, designed to facilitate the seamless integration of an Ottu-powered checkout process into iOS applications.

With the Checkout SDK, both the visual appearance and the forms of payment available during the [checkout process](./#ottu-checkout-sdk-flow) can be fully customized.

To integrate the Checkout SDK, the library must be included in the iOS application and initialized with the following parameters:

* [merchant\_id](https://app.gitbook.com/s/su3y9UFjvaXZBxug1JWQ/#merchant_id-string)
* [session\_id](../checkout-api.md#session_id-string-mandatory)
* [API public key](../authentication.md#public-key)

Additionally, optional configurations such as the [forms of payment](ios.md#formsofpayment-array-optional) to accept and the [theme](ios.md#customization-theme) styling for the checkout interface can be specified.

{% hint style="warning" %}
[API private key](../authentication.md#private-key-api-key) should never be used on the client side. Instead, [API public key ](../authentication.md#public-key)should be used. This is essential to ensure the security of your application and the protection of sensitive data.
{% endhint %}

## [Installation](ios.md#installation) <a href="#installation" id="installation"></a>

### [Minimum Requirements](ios.md#minimum-requirements) <a href="#minimum-requirements" id="minimum-requirements"></a>

The SDK is supported on devices running iOS 14 or higher.

### [**Installation with CocoaPods**](ios.md#installation-with-cocoapods)

Ottu is available via [CocoaPods](http://cocoapods.org/). To install it, the following line must be added to the Podfile:

{% code overflow="wrap" %}
```ruby
pod 'ottu_checkout_sdk', :git => 'https://github.com/ottuco/ottu-ios.git', :tag => '2.1.2'
```
{% endcode %}

{% hint style="info" %}
* When `ottu_checkout_sdk` is added to the **Podfile**, the **GitHub repository** must also be specified as follows:
* If CocoaPods returns an error like _"could not find compatible versions for pod"_, try running the `pod repo update` command to resolve it.
{% endhint %}

```ruby
pod 'ottu_checkout_sdk', :git => 'https://github.com/ottuco/ottu-ios'
```

### [**Installation with Swift Package Manager**](ios.md#installation-with-swift-package-manager)

The [Swift Package Manager](https://swift.org/package-manager/) (SPM) is a tool designed for automating the distribution of Swift code and is integrated into the `Swift` compiler.

Once the Swift package has been set up, adding Alamofire as a dependency requires simply including it in the `dependencies` value of the `Package.swift` file.

```swift
dependencies: [
    .package(url: "https://github.com/ottuco/ottu-ios.git", from: "2.1.2")
]
```

## [Native UI](ios.md#native-ui) <a href="#native-ui" id="native-ui"></a>

The SDK UI is embedded as a `View` within any part of a `ViewController` in the merchant's application.

**Example:**

<figure><img src="../../.gitbook/assets/image (97).png" alt="" width="375"><figcaption></figcaption></figure>

If only one payment option is available and it is a wallet, the UI is automatically minimized.

<figure><img src="../../.gitbook/assets/image (98).png" alt="" width="375"><figcaption></figcaption></figure>

## [SDK Configuration](ios.md#sdk-configuration) <a href="#sdk-configuration" id="sdk-configuration"></a>

### [Language](ios.md#language) <a href="#language" id="language"></a>

The SDK supports two languages: English and Arabic, with English set as the default.

The language applied in the device settings is automatically used by the SDK, requiring no manual adjustments within the application.

However, if the transaction is created in a different language and setup preload is enabled, texts retrieved from the backend (such as fee descriptions) will be displayed in the transaction language rather than the device's language.

Therefore, the currently selected device language or the app's selected language should be considered when specifying a language code in the transaction creation request of the [Checkout API](../checkout-api.md).

### [Light and dark theme](ios.md#light-and-dark-theme) <a href="#light-and-dark-theme" id="light-and-dark-theme"></a>

The SDK also supports UI adjustments based on the device's theme settings (light or dark mode).

The appropriate theme is applied automatically during SDK initialization, aligning with the device's settings. Similar to language settings, no manual adjustments are required within the application.

## [Functions](ios.md#functions) <a href="#functions" id="functions"></a>

The SDK currently provides a single function, serving as the entry point for the merchant's application.

Additionally, callbacks are provided and must be handled by the parent application. These callbacks are described [here](ios.md#callbacks).

### [**Checkout.init**](ios.md#checkout.init)

The `Checkout.init` function is responsible for initializing the checkout process and configuring the necessary settings for the Checkout SDK.

It must be called once by the parent application and provided with a set of configuration fields that define all the required options for the checkout process.

When `Checkout.init` is invoked, the SDK automatically sets up the essential components, including:

* Generating a form for the customer to enter their payment details.
* Handling communication with Ottu's servers to process the payment.

This function returns a `View` object, which is a native iOS UI component. It can be embedded within any `ViewController` instance in the application.

### [Properties](ios.md#properties) <a href="#properties" id="properties"></a>

#### [**merchantId**](ios.md#merchantid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The **`merchant_id`** specifies the **Ottu merchant domain** and must be set to the **root domain** of the **Ottu account**, excluding the **"https://"** or **"http://"** prefix.

For example, if the **Ottu URL** is **`https://example.ottu.com`**, then the corresponding **`merchant_id`** is **`example.ottu.com`**.

This parameter is used to **link** the **checkout process** to the appropriate **Ottu merchant account**.

#### [**apiKey**](ios.md#apikey-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `apiKey` is the Ottu API [public key](../authentication.md#public-key), used for authentication when communicating with Ottu's servers during the checkout process.

{% hint style="warning" %}
Only the public key should be used. The private key must remain confidential at all times and must not be shared with any clients.
{% endhint %}

#### [**sessionId**](ios.md#sessionid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `session_id` is a unique identifier assigned to the payment transaction associated with the checkout process.

This identifier is automatically generated when the payment transaction is created.

For more details on how to use the `session_id` parameter in the Checkout API, refer to the [session\_id](ios.md#sessionid-string-required).

#### [**formsOfPayment**](ios.md#formsofpayment-string-required) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The forms of payment displayed in the [checkout process](./#ottu-checkout-sdk-flow) can be customized using `formsOfPayment`. By default, all forms of payment are enabled.

Available options for `formsOfPayment`:

* `applePay`: Supports Apple Pay, allowing purchases to be made using Apple Pay-enabled devices.
* `stcPay`: Requires customers to enter their mobile number and authenticate with an OTP sent to their device to complete the payment.
* `cardOnsite`: Enables direct payments (onsite checkout), where Cardholder Data (CHD) is entered directly in the SDK. If 3DS authentication is required, a payment provider is involved in the process.
* `tokenPay`: Uses [tokenization](../tokenization.md) to securely store and process customers' payment information.
* `redirect`: Redirects customers to an external [payment gateway](../../user-guide/payment-gateway.md#payment-gateway-features-summary) or a third-party payment processor to complete the transaction.

#### [**setupPreload**](ios.md#setuppreload-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An `ApiTransactionDetails` struct object is used to store transaction details.

If provided, the SDK will not request transaction details from the backend, reducing processing time and improving efficiency.

#### [**theme**](ios.md#theme-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `Theme` struct object is used for UI customization, allowing modifications to background colors, text colors, and fonts for various components. It supports customization for both light and dark device modes. All fields in the `Theme` struct are optional. If a theme is not provided, the default UI settings will be applied. For more details, refer to the [Customization Theme](ios.md#customization-theme) section.

#### [displaySettings](ios.md#displaysettings-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The display of payment options is configured using the `PaymentOptionsDisplaySettings` struct. Additional information is provided in the [Payment Options Display Mode](ios.md#payment-options-display-mode) section.

#### [**delegate**](ios.md#delegate-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:red;">`required`</mark>_

An object is used to provide SDK callbacks to the application. Typically, this is the parent app’s class that conforms to `OttuDelegate`, aggregating the SDK object.

To implement this delegate, the class must define three callback functions. More details are accessible next [secion](ios.md#callbacks).

## [Callbacks](ios.md#callbacks) <a href="#callbacks" id="callbacks"></a>

In the Checkout SDK, callback functions are essential for providing real-time updates on the status of payment transactions.

These `callbacks` improve the user experience by enabling seamless and efficient handling of different payment scenarios, including:

* Successful payments
* Transaction cancellations
* Errors encountered during the payment process

All the callbacks described below can be triggered for any type of payment.

### [**errorCallback**](ios.md#errorcallback)

The `errorCallback` function is triggered when an issue occurs during the payment process. Proper error handling is essential to ensure a smooth user experience.

**Best Practice for Handling Errors**

In the event of an error, the recommended approach is to restart the checkout process by generating a new `session_id` through the [Checkout API](../checkout-api.md).

**Defining the** `errorCallback` **Function**

The `errorCallback` function can be defined using the `data-error` attribute on the Checkout script tag. This attribute allows the specification of a global function to handle errors.

When an **error occurs**, the **`errorCallback`** function is invoked with a **`data` JSON object**, where **`data.status`** is set to **`error`**.

**Params Available in** `data` **`JSONObject` for** `errorCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `status` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `order_no` optional
* `reference_number` optional

### [**cancelCallback**](ios.md#cancelcallback)

The `cancelCallback` function in the [Checkout SDK](./) is triggered when a payment is canceled.

**Defining the** `cancelCallback` **Function**

The `cancelCallback` function can be defined using the `data-cancel` attribute on the Checkout script tag. This attribute allows the specification of a global function to handle cancellations.

**Invocation of** `cancelCallback`

If a customer cancels a payment, the `cancelCallback` function is invoked with a `data` JSON object, where `data.status` is set to `canceled`.

**Params Available in** `data` **`JSONObject` for** `cancelCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `status` mandatory
* `order_no` optional
* `reference_number` optional
* `payment_gateway_info` optional

{% hint style="warning" %}
In both `cancelCallback` and `errorCallback`, the SDK must be reinitialized, either on the same session or on a new session.
{% endhint %}

### [**successCallback**](ios.md#successcallback)

In the [Checkout SDK](./), the `successCallback` function is triggered upon the successful completion of the [payment process](./#ottu-checkout-sdk-flow).

**Defining the `successCallback` Function**

The `successCallback` function is defined and assigned as the value of the `data-success` attribute within the Checkout script tag.

**Invocation of** `successCallback`

When a payment is successfully processed, the `successCallback` function is invoked with a `data` JSON object, where `data.status` is set to `success`.

**Params Available in** `data` **`JSONObject` for** `successCallback`

* `message` mandatory
* `form_of_payment`mandatory
* `challenge_occurred` optional
* `session_id` optional
* `status` mandatory
* `order_no` optional
* `reference_number` optional
* `redirect_url` optional
* `payment_gateway_info` optional

## [Example](ios.md#example)

There are both UIKit and SwiftUI samples available at the sample repo:

* UIKit: [<img src="https://github.com/fluidicon.png" alt="" data-size="line">ottu-ios/Example at main · ottuco/ottu-ios](https://github.com/ottuco/ottu-ios/tree/main/Example)
* SwiftUI: [<img src="https://github.com/fluidicon.png" alt="" data-size="line">ottu-ios/Example\_SwiftUI at main · ottuco/ottu-ios](https://github.com/ottuco/ottu-ios/tree/main/Example_SwiftUI)

The SDK initialization process and the callback delegate remain identical for both implementations.

**Code Sample:**

{% code overflow="wrap" %}
```swift
do {
  self.checkout =
    try Checkout(
      formsOfPayments: formsOfPayment,
      theme: theme,
      displaySettings: PaymentOptionsDisplaySettings(
        mode: paymentOptionsDisplayMode,
        visibleItemsCount: visibleItemsCount,
        defaultSelectedPgCode: defaultSelectedPgCode
      ),
      sessionId: sessionId,
      merchantId: merchantId,
      apiKey: apiKey,
      setupPreload: transactionDetailsPreload,
      delegate: self
    )
} catch
let error as LocalizedError {
  showFailAlert(error)
  return
} catch {
  print("Unexpected error: \(error)")
  return
}

if let paymentView = self.checkout?.paymentView() {
  paymentView.translatesAutoresizingMaskIntoConstraints = false
  self.paymentContainerView.addSubview(paymentView)

  NSLayoutConstraint.activate([
        paymentView.leadingAnchor.constraint(equalTo: self.paymentContainerView.leadingAnchor),
        self.paymentContainerView.trailingAnchor.constraint(equalTo: paymentView.trailingAnchor),
        paymentView.topAnchor.constraint(equalTo: self.paymentContainerView.topAnchor),
        self.paymentContainerView.bottomAnchor.constraint(equalTo: paymentView.bottomAnchor)
      ])
}
extension OttuPaymentsViewController: OttuDelegate {
  func errorCallback(_ data: [String: Any] ? ) {
    paymentContainerView.isHidden = true

    let alert = UIAlertController(title: "Error", message: data?.debugDescription ?? "", preferredStyle:
      .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
  }

  func cancelCallback(_ data: [String: Any] ? ) {
    var message = ""

    if let paymentGatewayInfo = data ? ["payment_gateway_info"] as ? [String: Any],
      let pgName = paymentGatewayInfo["pg_name"] as ? String,
        pgName == "kpay" {
          message = paymentGatewayInfo["pg_response"].debugDescription
        } else {
          message = data?.debugDescription ?? ""
        }

    paymentContainerView.isHidden = true

    let alert = UIAlertController(title: "Canсel", message: message, preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
  }

  func successCallback(_ data: [String: Any] ? ) {
    paymentContainerView.isHidden = true
    paymentSuccessfullLabel.isHidden = false

    let alert = UIAlertController(title: "Success", message: data?.debugDescription ?? "", preferredStyle:
      .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    present(alert, animated: true)
  }
}

```
{% endcode %}

## [Customization Theme](ios.md#customization-theme) <a href="#customization-theme" id="customization-theme"></a>

The main class describing theme is called `CheckoutTheme`.

It uses additional component classes like:

* `ButtonComponent`
* `LabelComponent`
* `TextFieldComponent`

The `CheckoutTheme` class consists of objects that define various UI components. While the names of these components largely correspond to those listed [here](ios.md#properties-description), they also include platform-specific fields for further customization.

{% embed url="https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG/Ottu-SDK---Components-Documentation?node-id=1-624&t=7reOLSB5zVdlOwz0-1" %}

### [Properties description](ios.md#properties-description) <a href="#properties-description" id="properties-description"></a>

All properties in the `CheckoutTheme` class are optional, allowing users to customize any of them as needed.

If a property is not specified, the default value (as defined in the Figma design [here](https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG?content-scaling=fixed\&kind=proto\&node-id=1-624\&scaling=scale-down)) will be automatically applied.

#### [**Texts**](ios.md#texts)

#### **General**

| Property Name |                            Description                            |                Data Type                |
| ------------- | :---------------------------------------------------------------: | :-------------------------------------: |
| `mainTitle`   |                 Font and color for all “Captions”                 | [LabelComponent](ios.md#labelcomponent) |
| `title`       |           Font and color for payment options in the list          | [LabelComponent](ios.md#labelcomponent) |
| `subtitle`    | Font and color for payment options details (like expiration date) | [LabelComponent](ios.md#labelcomponent) |

#### **Fees**

| Property Name  |                           Description                          |                Data Type                |
| -------------- | :------------------------------------------------------------: | :-------------------------------------: |
| `feesTitle`    |    Font and color of fees value in the payment options list    | [LabelComponent](ios.md#labelcomponent) |
| `feesSubtitle` | Font and color of fees description in the payment options list | [LabelComponent](ios.md#labelcomponent) |

#### **Data**

| Property Name |                        Description                       |                Data Type                |
| ------------- | :------------------------------------------------------: | :-------------------------------------: |
| `dataLabel`   | Font and color of payment details fields (like “Amount”) | [LabelComponent](ios.md#labelcomponent) |
| `dataValue`   |         Font and color of payment details values         | [LabelComponent](ios.md#labelcomponent) |

**Other**

| Property Name      |                   Description                   |                Data Type                |
| ------------------ | :---------------------------------------------: | :-------------------------------------: |
| `errorMessageText` | Font and color of error message text in pop-ups | [LabelComponent](ios.md#labelcomponent) |

#### [**Text Fields**](ios.md#text-fields)

| Property Name    |                              Description                             |                    Data Type                    |
| ---------------- | :------------------------------------------------------------------: | :---------------------------------------------: |
| `inputTextField` | Font and color of text in any input field (including disabled state) | [TextFieldComponent](ios.md#textfieldcomponent) |

#### [**Colors**](ios.md#colors)

| Property Name          |                  Description                  | Data Type |
| ---------------------- | :-------------------------------------------: | :-------: |
| `backgroundColor`      | The main background of the SDK view component |  UIColor  |
| `backgroundColorModal` |       The background of any modal window      |  UIColor  |
| `iconColor`            |      The color of the icon of the payment     |  UIColor  |

#### [**Buttons**](ios.md#buttons)

| Property Name    |                            Description                            |                 Data Type                 |
| ---------------- | :---------------------------------------------------------------: | :---------------------------------------: |
| `button`         |           Background, text color and font for any button          | [ButtonComponent](ios.md#buttoncomponent) |
| `selectorButton` | Background, text color and font for payment item selection button | [ButtonComponent](ios.md#buttoncomponent) |

#### [**Switch**](ios.md#switch)

| Property Name       |              Description             | Data Type |
| ------------------- | :----------------------------------: | :-------: |
| `switchOnTintColor` | The color of switch (toggle) control |  UIColor  |

#### [**Margins**](ios.md#margins)

| Property Name |                     Description                     |              Data Type              |
| ------------- | :-------------------------------------------------: | :---------------------------------: |
| margins       | Top, left, bottom and right margins between compone | [UIEdgeInsets](ios.md#uiedgeinsets) |

#### [Payment Details](ios.md#payment-details)

| Property Name        |                                            Description                                            | Data Type |
| -------------------- | :-----------------------------------------------------------------------------------------------: | :-------: |
| `showPaymentDetails` | Boolean variable determining whether the “Payment Details” section should be displayed or hidden. |  Boolean  |

### [Data types description](ios.md#data-types-description) <a href="#data-types-description" id="data-types-description"></a>

#### [**LabelComponent**](ios.md#labelcomponent)

| Property Name | Data Type  |
| ------------- | :--------: |
| `color`       |   UIColor  |
| `font`        |   UIFont   |
| `fontFamily`  |   String   |

#### [**TextFieldComponent**](ios.md#textfieldcomponent)

| Property Name     |                                                                    Data Type                                                                    |
| ----------------- | :---------------------------------------------------------------------------------------------------------------------------------------------: |
| `label`           |                                                     [LabelComponent](ios.md#labelcomponent)                                                     |
| `text`            | [LabelComponent](https://app.gitbook.com/o/RxY0H8C3fNw3knTb5iVs/s/XdPwy0yrnZJ8gfKCUk9E/~/changes/601/developer/checkout-sdk/ios#labelcomponent) |
| `backgroundColor` |                                                                     UIColor                                                                     |

#### [**ButtonComponent**](ios.md#buttoncomponent)

| Property Name             | Data Type |
| ------------------------- | :-------: |
| `enabledTitleColor`       |  UIColor  |
| `disabledTitleColor`      |  UIColor  |
| `font`                    |   UIFont  |
| `enabledBackgroundColor`  |  UIColor  |
| `disabledBackgroundColor` |  UIColor  |
| `fontFamily`              |   String  |

#### [**UIEdgeInsets**](ios.md#uiedgeinsets)

| Property Name | Data Type |
| ------------- | :-------: |
| `left`        |    Int    |
| `top`         |    Int    |
| `right`       |    Int    |
| `bottom`      |    Int    |

## [Example](ios.md#example.1) <a href="#example.1" id="example.1"></a>

To configure the `theme`, similar steps must be followed as described in the test app file.

**Code Snippet:**

```swift
func createTheme() - > CheckoutTheme {
    var theme = CheckoutTheme()
    theme.backgroundColor = .systemBackground
    theme.backgroundColorModal = .secondarySystemBackground
    theme.margins = UIEdgeInsets(top: 8, left: 2, bottom: 8, right: 2)
    theme.mainTitle.color = .label
    theme.mainTitle.fontFamily = "Arial"
    theme.button.enabledTitleColor = .payButtonTitle
    theme.button.disabledTitleColor = .payButtonDisabledTitle
    theme.button.fontFamily = "Arial"
    theme.button.enabledBackgroundColor = .payButtonBackground
    theme.button.disabledBackgroundColor = .payButtonDisabledBackground
    return theme
}
```

The theme object is passed to the SDK initialization as shown below:

**Code Snippet:**

```swift
self.checkout = Checkout(
    theme: theme,
    sessionId: sessionId,
    merchantId: merchantId,
    apiKey: apiKey,
    delegate: self
)
```

## [Payment Options Display Mode](ios.md#payment-options-display-mode)

The payment options display can be adjusted using the SDK with the following settings:

* `mode` (`BottomSheet` or `List`)
* `visibleItemsCount` (default is 5)
* `defaultSelectedPgCode` (default is empty)

By default, **BottomSheet mode** is used, as set in previous releases. **List mode** is a new option, where a list of payment methods is displayed above the **Payment Details** section and the **Pay** button.

<figure><img src="../../.gitbook/assets/image (99).png" alt="" width="375"><figcaption></figcaption></figure>

**A view with a selected item**

<figure><img src="../../.gitbook/assets/image (100).png" alt="" width="375"><figcaption></figcaption></figure>

**A view with an expanded list:**

<figure><img src="../../.gitbook/assets/image (101).png" alt="" width="375"><figcaption></figcaption></figure>

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

To view the full function call, please refer to the [Ottu SDK - iOS | Example](ios.md#example) chapter in the documentation. This section provides the complete example, including how the function is used in the context of the Ottu SDK.

## [Apple Pay](ios.md#apple-pay) <a href="#apple-pay" id="apple-pay"></a>

When the [integration ](web.md#apple-pay)between Ottu and Apple for Apple Pay is completed, the necessary checks to display the Apple Pay button are handled automatically by the Checkout SDK.

1. **Initialization**: Upon initialization of the Checkout SDK with the provided [session\_id](ios.md#sessionid-string-required) and payment gateway codes ([pg\_codes](../checkout-api.md#pg_codes-array-required)), several conditions are automatically verified:
   * It is confirmed that a `session_id` and `pg_codes` associated with the Apple Pay Payment Service have been supplied.
   * It is ensured that the customer is using an Apple device that supports Apple Pay. If the device is not supported, the button will not be shown, and an error message stating `This device doesn't support Apple Pay` will be displayed to inform the user of the compatibility issue.
   * It is verified that the customer has a wallet configured on their Apple Pay device. if the wallet is not configured (i.e., no payment cards are added), the Setup button will  appear. Clicking on this button will prompt the Apple Pay wallet on the user's device to open, allowing them to configure it by adding payment cards.
2. **Displaying the Apple Pay Button**: If all these conditions are met, the Apple Pay button is displayed and made available for use in the checkout flow.
3. **Restricting Payment Options**: To display only the Apple Pay button, `applePay` should be passed within the `formsOfPayment` parameter. The `formsOfPayment` property instructs the Checkout SDK to render only the Apple Pay button. If this property is not included, all available payment options are rendered by the SDK.

This setup ensures a seamless integration and user experience, allowing customers to easily set up and use Apple Pay during the checkout process.

## [STC Pay](ios.md#stc-pay) <a href="#stc-pay" id="stc-pay"></a>

When the [integration](web.md#stc-pay) between Ottu and STC Pay is completed, the necessary checks to display the STC Pay button are handled seamlessly by the Checkout SDK.

**Initialization**: Upon initialization of the Checkout SDK with the provided [session\_id](ios.md#sessionid-string-required) and payment gateway codes ([pg\_codes](../checkout-api.md#pg_codes-array-required)), several conditions are automatically verified:

* It is confirmed that the `session_id` and `pg_codes` provided during SDK initialization are associated with the STC Pay Payment Service. This ensures that the STC Pay option is available for the customer to choose as a payment method.
* It is ensured that the STC Pay button is displayed by the iOS SDK, regardless of whether the customer has provided a mobile number while creating the transaction.

This setup ensures a seamless integration and user experience, allowing customers to easily set up and use STC Pay during the checkout process.

## [KNET - Apple Pay](ios.md#knet-apple-pay) <a href="#knet-apple-pay" id="knet-apple-pay"></a>

Due to compliance requirements, KNET necessitates a popup displaying the payment result after each failed payment. This functionality is available only in the `cancelCallback` when there is a response from the payment gateway. As a result, the user must click on the Apple Pay button again to retry the payment.

{% hint style="info" %}
The popup notification requirement is specific to the KNET payment gateway. Other payment gateways may have different requirements or notification mechanisms, so it is essential to follow the respective documentation for each payment gateway integration.
{% endhint %}

To properly handle the popup notification for KNET, the following code snippet should be implemented into your payment processing flow:

```swift
func cancelCallback(_ data: [String: Any] ? ) {
    var message = ""

    if let paymentGatewayInfo = data ? ["payment_gateway_info"] as ? [String: Any],
        let pgName = paymentGatewayInfo["pg_name"] as ? String,
            pgName == "kpay" {
                message = paymentGatewayInfo["pg_response"].debugDescription
            } else {
                message = data?.debugDescription ?? ""
            }

    navigationController?.popViewController(animated: true)
    let alert = UIAlertController(title: "Canсel", message: message, preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
}
}
```

The above code performs the following checks and actions:

1. **Verification**: It first checks if the cancel object contains information about the payment gateway `payment_gateway_info`.
2. **Payment Gateway Identification**: It then verifies if the `pg_name` property in `payment_gateway_info` is equal to "kpay", confirming that the payment gateway used is KNET.
3. **Response Handling**: If the conditions are met, it retrieves the payment gateway's response from the `pg_response` property. If not available, it uses a default "Payment was cancelled." error message.
4. **Popup Notification**: Finally, it displays the error message in a popup using `self.present(alert, animated: true)` to notify the user about the failed payment.

This setup ensures compliance with KNET's requirements and provides a clear user experience for handling failed payments.

## [Onsite Checkout](ios.md#onsite-checkout)

This payment option enables direct payments through the mobile SDK. The SDK provides a user interface where the Cardholder Data (CHD) is entered by the user. If permitted by the backend, the card can be tokenized and saved for future payments.

Below is an example of how the onsite checkout screen appears:

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

The SDK supports multiple instances of onsite checkout payments. Therefore, for each payment method with a PG code of `ottu_pg`, the card form (as described above) will be displayed.

<figure><img src="../../.gitbook/assets/image (102).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
Fees are not shown for onsite checkout instances because the system supports payments through multiple cards (omni PG). The multiple payment icons indicate the availability of different card options.
{% endhint %}

## [Error Reporting](ios.md#error-reporting) <a href="#error-reporting" id="error-reporting"></a>

The SDK utilizes Sentry for error logging and reporting, which is initialized based on the configuration from SDK Studio. However, since the SDK is integrated into the merchant's app, conflicts may arise if the app also uses Sentry. To avoid this, merchants can disable Sentry in the Checkout SDK by setting the `is_enabled` flag to `false` in the configuration.

## [Cyber Security Measures](ios.md#cyber-security-measures) <a href="#cyber-security-measures" id="cyber-security-measures"></a>

### [Jailbreak Detection](ios.md#jailbreak-detection) <a href="#jailbreak-detection" id="jailbreak-detection"></a>

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

### [Screen Capture Prevention](ios.md#screen-capture-prevention)

The SDK implements screen capture restrictions to prevent the collection of sensitive data. This applies to fields containing Cardholder Data (CHD), such as the onsite checkout form for entering card details and the CVV field for tokenized payments.

This technique works in two ways:

1. When attempting to take a screenshot of a protected screen, the fields appear empty, even if they contain input.
2. When attempting to record a video of the screen, the video is completely blurred, making the content unreadable.

## [FAQ](ios.md#faq) <a href="#faq" id="faq"></a>

#### :digit\_one: [What forms of payments are supported by the SDK?](ios.md#id-1.-what-forms-of-payments-are-supported-by-the-sdk) <a href="#id-1.-what-forms-of-payments-are-supported-by-the-sdk" id="id-1.-what-forms-of-payments-are-supported-by-the-sdk"></a>

The SDK supports the following payment forms: `tokenPay`, `ottuPG`, `redirect` `applePay` and `stcPay`. Merchants can display specific methods according to their needs.

**For example,** if you want to only show the STC Pay button, you can do so using formsOfPayment = \[`stcPay`], and only the STC Pay button will be displayed. The same applies for `applePay` and other methods.

#### :digit\_two: [What are the minimum system requirements for the SDK integration?](ios.md#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration) <a href="#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration" id="id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration"></a>

It is required to have a device running iOS 13 or higher.

#### :digit\_three: [Can I customize the appearance beyond the provided themes?](ios.md#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes) <a href="#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes" id="id-3.-can-i-customize-the-appearance-beyond-the-provided-themes"></a>

Yes, see the [Customization theme](ios.md#customization-theme) section.

#### :digit\_four: [How do I customize the payment request for Apple Pay?](ios.md#id-4.-how-do-i-customize-the-payment-request-for-apple-pay) <a href="#id-4.-how-do-i-customize-the-payment-request-for-apple-pay" id="id-4.-how-do-i-customize-the-payment-request-for-apple-pay"></a>

The payment request for Apple Pay can be customized using its initialization methods. These methods allow the configuration of various properties, including:

* API version
* Supported card types
* Accepted networks
* Applicable countries
* Merchant capabilities

For a complete list of supported properties, refer to the [Apple Pay](https://developer.apple.com/documentation/apple_pay_on_the_web/applepaypaymentrequest) documentation.
