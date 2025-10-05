# Android

The [Checkout SDK](./) from Ottu is a Kotlin-based library designed to streamline the integration of an Ottu-powered [checkout process](./#ottu-checkout-sdk-flow) into Android applications. This SDK allows for complete customization of the checkout experience, including both appearance and functionality, as well as the selection of accepted payment methods.

To integrate the Checkout SDK, it must be incorporated into the Android application and initialized with the following parameters:

* [merchant\_id](https://app.gitbook.com/s/su3y9UFjvaXZBxug1JWQ/#merchant_id-string)
* [session\_id](android.md#sessionid-string-required)
* [API public key](../authentication.md#public-key)

Additionally, various configuration options, such as accepted [payment methods](android.md#formsofpayment-array-optional) and [theme ](android.md#customization-theme)styling for the checkout interface, can be specified to enhance the user experience.

{% hint style="warning" %}
The [API private key](../authentication.md#private-key-api-key) should never be utilized on the client side; instead, use the [API public key](../authentication.md#public-key). This is essential for maintaining the security of your application and safeguarding sensitive data.
{% endhint %}

## [Installation](android.md#installation) <a href="#installation" id="installation"></a>

### [Minimum Requirements](android.md#minimum-requirements) <a href="#minimum-requirements" id="minimum-requirements"></a>

The SDK is compatible with devices running Android 8 or higher (API version 26 or later).

### [Installation with dependency](android.md#installation-with-dependency) <a href="#installation-with-dependency" id="installation-with-dependency"></a>

```groovy
allprojects {
    repositories {
        // Other repositories...
        maven { url "https://jitpack.io" }
    }
}

dependencies {
    implementation 'com.github.ottuco:ottu-android-checkout:2.1.7'
}
```

## [Native UI](android.md#native-ui) <a href="#native-ui" id="native-ui"></a>

The SDK UI is embedded as a Fragment within any part of an Activity in the merchant's application.

**Example:**

<figure><img src="../../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

If a wallet is the only available payment option, the UI is minimized automatically.

<figure><img src="../../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
To avoid style issues and potential crashes, the parent application's theme must be `Theme.AppCompat` or one of its descendant classes. This theme is specified in the `themes.xml` file located in the `values` directory of the project.
{% endhint %}

## [SDK Configuration](android.md#sdk-configuration) <a href="#sdk-configuration" id="sdk-configuration"></a>

### [Language](android.md#language) <a href="#language" id="language"></a>

The SDK supports two languages, English and Arabic, with English set as the default.

It automatically adopts the language configured in the device settings, requiring no in-app adjustments. However, if a transaction is initiated in a different language and setup preload is utilized, the backend-generated text (such as fee descriptions) will appear in the language of the transaction. Therefore, it is important to ensure that the language code passed to the [Checkout API](../checkout-api.md)'s transaction creation request matches the currently selected language on the device or current selected app language.

### [Light and dark theme](android.md#light-and-dark-theme) <a href="#light-and-dark-theme" id="light-and-dark-theme"></a>

The SDK supports UI customization to match the device theme—light or dark. This adjustment is applied during the SDK initialization, based on the device's settings. Similarly, for language, no adjustments are made within the app.

## [Functions](android.md#functions) <a href="#functions" id="functions"></a>

Currently, the SDK offers a single [function ](android.md#checkout.init)that serves as the entry point for the merchant's application. It also includes [callbacks](android.md#callbacks) that must be managed by the parent app, which are detailed in the following [section](android.md#callbacks).

### [**Checkout.init**](https://docs.ottu.com/developer/checkout-sdk/web#checkout.init)

The function initiates the checkout process and configures the required settings for the Checkout SDK. It should be invoked once by the parent app to start the checkout sequence, called with set of configuration fields that encapsulate all essential options for the process.

When you call `Checkout.init`, the SDK manages the setup of key components for the checkout, like generating a form for customers to input their payment information, and facilitating the communication with Ottu's servers to process the payment.

This function returns a `Fragment` object, which is a native Android UI component that can be integrated into any part of an `Activity` instance (also native to Android).

### [Properties](android.md#properties) <a href="#properties" id="properties"></a>

#### [**merchantId**](android.md#merchantid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `merchant_id` identifies your Ottu merchant domain. It should be the root domain of your Ottu account, excluding the "https://" or "http://" prefix.

For instance, if your Ottu URL is https://example.ottu.com, then your `merchant_id` would be example.ottu.com. This attribute is utilized to determine which Ottu merchant account to associate with the checkout process.

#### [**apiKey**](android.md#apikey-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `apiKey` is your Ottu [API public key](../authentication.md#public-key), which is essential for authenticating communications with Ottu's servers during the checkout process.

{% hint style="warning" %}
Make sure to use the public key and avoid using the private key. The [API private key](../authentication.md#private-key-api-key) must be kept confidential at all times and should never be shared with any clients.
{% endhint %}

#### [**sessionId**](android.md#sessionid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `session_id` serves as the unique identifier for the payment transaction linked to the checkout process.

This identifier is automatically generated at the creation of the payment transaction. For additional details on how to utilize the `session_id` parameter in the [Checkout AP](../checkout-api.md)I, refer to the [session\_id](android.md#sessionid-string-required) section.&#x20;

#### [**formsOfPayment**](android.md#formsofpayment-string-required) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `formsOfPayment` parameter allows customization of the payment methods displayed in the [checkout process](./#ottu-checkout-sdk-flow). By default, all forms of payment are enabled.

**Available Options for** `formsOfPayment`

* **`cardOnsite`**: A direct payment method (onsite checkout) where cardholder data (CHD) is entered directly in the SDK. If 3DS authentication is required, a payment provider is involved.
* `tokenPay`: Uses [tokenization](../tokenization.md) to securely store and process customers' payment information.
* `redirect`: Redirects customers to an external [payment gateway](../../user-guide/payment-gateway.md#payment-gateway-features-summary) or a third-party payment processor to complete the transaction.
* `StcPay`: Requires customers to enter their mobile number and authenticate with an OTP sent to their device to complete the payment.
* `flexMethods`: Allows payments to be split into multiple installments. These methods, also known as BNPL (Buy Now, Pay Later), support providers such as Tabby and Tamara.

#### [**setupPreload**](android.md#setuppreload-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `ApiTransactionDetails` class object stores transaction details.

If this object is provided, the SDK will not need to retrieve transaction details from the backend, thereby reducing processing time and improving efficiency.

#### [**theme**](android.md#theme-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `Theme` class object is used for UI customization, allowing modifications to background colors, text colors, and fonts for various components.

All fields in the `Theme` class are optional. If a theme is not specified, the default UI settings will be applied. For more details, refer to the [Customization Theme](android.md#customization-theme) section.

#### [**displaySettings**](android.md#displaysettings-object-optional)  _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `PaymentOptionsDisplaySettings` struct is used to configure how payment options are displayed. More details can be found in the [Payment Options Display Mode](android.md#payment-options-display-mode) section.

#### [**successCallback, errorCallback and successCallback**](android.md#successcallback-errorcallback-and-successcallback-uint-required) _<mark style="color:blue;">`Uint`</mark>_ _<mark style="color:red;">`required`</mark>_

Callback functions are used to retrieve the payment status and must be provided directly to the Checkout initialization function. For more details, refer to the [Callbacks](android.md#callbacks) section.

## [Callbacks](android.md#callbacks) <a href="#callbacks" id="callbacks"></a>

In the Checkout SDK, callback functions are essential for delivering real-time updates on the status of payment transactions. These callbacks improve the user experience by facilitating smooth and effective management of different payment scenarios, including errors, successful transactions, and cancellations.&#x20;

{% hint style="info" %}
The callbacks outlined below are applicable to any type of payment.
{% endhint %}

### [**errorCallback**](android.md#errorcallback)

The `errorCallback` is a callback function triggered when issues occur during a payment process. Properly handling these errors is essential for maintaining a smooth user experience.&#x20;

{% hint style="info" %}
The best practice recommended in the event of an error is to restart the checkout process by generating a new `session_id` through the [Checkout API](https://docs.ottu.com/developer/checkout-api).
{% endhint %}

To set up the `errorCallback` function, use the `data-error` attribute on the Checkout script tag to designate a global function that will manage errors. If an error arises during a payment, the `errorCallback` function will be called, receiving a `JSONObject` with a `data.status` value indicating an error.

**Params Available in** `data` **`JSONObject` for** `errorCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `status` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `order_no` optional
* `reference_number` optional

### [**cancelCallback**](android.md#cancelcallback)

The `cancelCallback` is a callback function in the Checkout SDK that is activated when a payment is canceled.&#x20;

To configure the `cancelCallback` function, you can use the `data-cancel` attribute on the Checkout script tag to specify a global function that will handle cancellations. If a payment is canceled by a customer, the `cancelCallback` function will be called, and it will receive a `JSONObject` containing a `data.status` value of "canceled".

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

### [**successCallback**](android.md#successcallback)

The `successCallback` is a function that is triggered when the payment process is successfully completed. This callback receives a `JSONObject` containing a `data.status` value of "success."

**Params Available in** `data` `JSONObject` for `successCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `status` mandatory
* `order_no` optional
* `reference_number` optional
* `redirect_url` optional
* `payment_gateway_info` optional

The `successCallback` function is defined and assigned by setting the `data-success` attribute on the Checkout script tag. This attribute specifies a global function that will be invoked when the payment process successfully completes.

## [Example](android.md#example)

```swift
val theme = getCheckoutTheme()

// Builder class is used to construct an object passed to the SDK initializing function
val builder = Checkout
  .Builder(merchantId!!, sessionId, apiKey!!, amount!!)
    .paymentOptionsDisplaySettings(paymentOptionsDisplaySettings)
    .formsOfPayments(formsOfPayment)
    .theme(theme)
    .logger(Checkout.Logger.INFO)
    .build()


// Actual `init` function calling, returning a `Fragment` object  
checkoutFragment = Checkout.init(
    context = this@CheckoutSampleActivity,
    builder = builder,
    transactionResultCallback = object : Checkout.TransactionResultCallback {
        override fun onTransactionResult(result: TransactionResult) {
            showResultDialog(result)
        }
    }
)
```

## [Customization Theme](android.md#customization-theme) <a href="#customization-theme" id="customization-theme"></a>

The class responsible for defining the theme is called `CheckoutTheme`.

**Customization Theme Class Structure:**

```swift
class CheckoutTheme(
    val uiMode: UiMode = UiMode.AUTO,
    val appearanceLight: Appearance ? = null,
    val appearanceDark : Appearance ? = null,
    val showPaymentDetails : Boolean = true,
)
```

#### [uiMode](android.md#uimode)

Specifies the device theme mode, which can be set to:

* Light
* Dark
* Auto (automatically adjusts based on system settings)

#### [**appearanceLight & appearanceDark**](android.md#appearancelight-and-appearancedark)

These are optional instances of the `Appearance` class, which enable UI customization for light mode and dark mode, respectively.

The `Appearance` class serves as the core inner class of `CheckoutTheme`, containing objects that define various UI components.

The component names within `Appearance` largely correspond to those described [here](android.md#properties-description).

{% embed url="https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG/Ottu-SDK---Components-Documentation?node-id=1-624&t=VdaigMvefTae0naX-1" %}

#### [**showPaymentDetails**](android.md#showpaymentdetails)

A boolean field that determines whether the "Payment Details" section should be displayed or hidden.

### [Properties description](android.md#properties-description) <a href="#properties-description" id="properties-description"></a>

All properties are optional and can be customized by the user.

If a property is not specified, the default value (as defined in the Figma design [here](https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG?content-scaling=fixed\&kind=proto\&node-id=1-624\&scaling=scale-down)) will be automatically applied.

#### [**Texts**](android.md#texts)

#### **General**

| Property Name   |                            Description                            |        Data Type        |
| --------------- | :---------------------------------------------------------------: | :---------------------: |
| `mainTitleText` |                 Font and color for all “Captions”                 | [Text](android.md#text) |
| `titleText`     |           Font and color for payment options in the list          | [Text](android.md#text) |
| `subtitleText`  | Font and color for payment options details (like expiration date) | [Text](android.md#text) |

#### **Fees**

| Property Name      |                           Description                          |        Data Type        |
| ------------------ | :------------------------------------------------------------: | :---------------------: |
| `feesTitleText`    |    Font and color of fees value in the payment options list    | [Text](android.md#text) |
| `feesSubtitleText` | Font and color of fees description in the payment options list | [Text](android.md#text) |

#### **Data**

| Property Name   |                        Description                       |        Data Type        |
| --------------- | :------------------------------------------------------: | :---------------------: |
| `dataLabelText` | Font and color of payment details fields (like “Amount”) | [Text](android.md#text) |
| `dataValueText` |         Font and color of payment details values         | [Text](android.md#text) |

**Other**

| Property Name                   |                           Description                          |        Data Type        |
| ------------------------------- | :------------------------------------------------------------: | :---------------------: |
| `errorMessageText`              |         Font and color of error message text in pop-ups        | [Text](android.md#text) |
| `selectPaymentMethodHeaderText` | The text of "Select Payment Method" in the bottom sheet header | [Text](android.md#text) |

#### [**Text Fields**](android.md#text-fields)

| Property Name    |                              Description                             |             Data Type             |
| ---------------- | :------------------------------------------------------------------: | :-------------------------------: |
| `inputTextField` | Font and color of text in any input field (including disabled state) | [TextField](android.md#textfield) |

#### [**Colors**](android.md#colors)

| Property Name                              |                       Description                      |         Data Type         |
| ------------------------------------------ | :----------------------------------------------------: | :-----------------------: |
| `sdkbackgroundColor`                       |      The main background of the SDK view component     | [Color](android.md#color) |
| `modalBackgroundColor`                     |           The background of any modal window           | [Color](android.md#color) |
| `paymentItemBackgroundColor`               |    The background of an item in payment options list   | [Color](android.md#color) |
| `selectorIconColor`                        |          The color of the icon of the payment          | [Color](android.md#color) |
| `savePhoneNumberIconColor`                 | The color of “Diskette” button for saving phone number | [Color](android.md#color) |
| `selectPaymentMethodHeaderBackgroundColor` |    The background of an item in payment options list   | [Color](android.md#color) |

#### [**Buttons**](android.md#buttons)

| Property Name    |                            Description                            |               Data Type               |
| ---------------- | :---------------------------------------------------------------: | :-----------------------------------: |
| `button`         |           Background, text color and font for any button          |      [Button](android.md#button)      |
| `backButton`     |               Color of the “Back” navigation button               | [RippleColor](android.md#ripplecolor) |
| `selectorButton` | Background, text color and font for payment item selection button |      [Button](android.md#button)      |

#### [**Switch**](android.md#switch)

| Property Name |                                        Description                                        |           Data Type           |
| ------------- | :---------------------------------------------------------------------------------------: | :---------------------------: |
| `switch`      | Colors of the switch background and its toggle in different states (on, off and disabled) | [Switch](android.md#switch-1) |

#### [**Margins**](android.md#margins)

| Property Name |                      Description                      |            Data Type            |
| ------------- | :---------------------------------------------------: | :-----------------------------: |
| margins       | Top, left, bottom and right margins between component | [Margins](android.md#margins-1) |

### [Data types description](android.md#data-types-description) <a href="#data-types-description" id="data-types-description"></a>

#### [**Color**](android.md#color)

| Property Name   |             Description             | Data Type |
| --------------- | :---------------------------------: | :-------: |
| `color`         |       Main color integer value      |    Int    |
| `colorDisabled` | Disabled stated color integer value |    Int    |

#### [**RippleColor**](android.md#ripplecolor)

| Property Name  |             Description             | Data Type |
| -------------- | :---------------------------------: | :-------: |
| `color`        |       Main color integer value      |    Int    |
| `rippleColor`  |      Ripple color integer value     |    Int    |
| `colorDisaled` | Disabled stated color integer value |    Int    |

#### [**Text**](android.md#text)

| Property Name |        Description       |         Data Type         |
| ------------- | :----------------------: | :-----------------------: |
| `textColor`   | Main color integer value | [Color](android.md#color) |
| `fontType`    |     Font resource ID     |            Int            |

#### [**TextField**](android.md#textfield)

|  Property Name | Description                    |         Data Type         |
| :------------: | ------------------------------ | :-----------------------: |
|  `background`  | Background color integer value | [Color](android.md#color) |
| `primaryColor` | Text color                     | [Color](android.md#color) |
| `focusedColor` | Selected text color            | [Color](android.md#color) |
|     `text`     | Text value                     |  [Text](android.md#text)  |
|     `error`    | Text value                     |  [Text](android.md#text)  |

#### [Button](android.md#button)

| Property Name |       Description       |               Data Type               |
| ------------- | :---------------------: | :-----------------------------------: |
| `rippleColor` | Button background color | [RippleColor](android.md#ripplecolor) |
| `fontType`    |   Button text font ID   |                  Int                  |
| `textColor`   |    Button text color    |       [Color](android.md#color)       |

#### [Switch](android.md#switch-1)

| Property Name                   |             Description             | Data Type |
| ------------------------------- | :---------------------------------: | :-------: |
| `checkedThumbTintColor`         |    Toggle color in checked state    |    Int    |
| `uncheckedThumbTintColor`       |   Toggle color in unchecked state   |    Int    |
| `checkedTrackTintColor`         |     Track color in checked state    |    Int    |
| `uncheckedTrackTintColor`       |    Track color in unchecked state   |    Int    |
| `checkedTrackDecorationColor`   |  Decoration color in checked state  |    Int    |
| `uncheckedTrackDecorationColor` | Decoration color in unchecked state |    Int    |

#### [**Margins**](android.md#margins-1)

| Property Name | Data Type |
| ------------- | :-------: |
| `left`        |    Int    |
| `top`         |    Int    |
| `right`       |    Int    |
| `bottom`      |    Int    |

## [Example](android.md#example.1) <a href="#example.1" id="example.1"></a>

To build the `theme`, the user must follow steps similar to those outlined in the **test app file**.

**Code Snippet:**

```swift
val appearanceLight = CheckoutTheme.Appearance(
    mainTitleText = CheckoutTheme.Text(
        textColor = CheckoutTheme.Color(Color.WHITE),
        R.font.roboto_bold
    ),
    titleText = CheckoutTheme.Text(textColor = CheckoutTheme.Color(Color.BLACK)),
    subtitleText = CheckoutTheme.Text(textColor = CheckoutTheme.Color(Color.BLACK)),

    button = CheckoutTheme.Button(
        rippleColor = CheckoutTheme.RippleColor(
            color = Color.WHITE,
            rippleColor = Color.BLACK,
            colorDisabled = Color.GRAY
        ),
        textColor = CheckoutTheme.Color(color = Color.BLACK),
        fontType = R.font.roboto_bold
    ),

    margins = Margins(left = 12, top = 4, right = 12, bottom = 4),

    sdkBackgroundColor = CheckoutTheme.Color(color = Color.WHITE)
)

return CheckoutTheme(
    uiMode = CheckoutTheme.UiMode.AUTO,
    showPaymentDetails = showPaymentDetails,
    appearanceLight = appearanceLight,
    appearanceDark = appearanceDark,
)
```

## [Payment Options Display Mode](android.md#payment-options-display-mode) <a href="#payment-options-display-mode" id="payment-options-display-mode"></a>

The display of payment options can be adjusted using the SDK with the following settings:

* `mode` (`BottomSheet` or List)
* `visibleItemsCount` (default is 5)
* `defaultSelectedPgCode` (default is none)

By default, **BottomSheet mode** is used, as was implemented in previous releases. **List mode** is a new option that allows the list of payment methods to be displayed above the **Payment Details** section and the **Pay** button.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

**A view with a selected item:**

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

**A view with an expanded list:**

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

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

To view the full function call, please refer to the [Ottu SDK - Android | Example](android.md#example) chapter in the documentation.

## [STC Pay](android.md#stc-pay) <a href="#stc-pay" id="stc-pay"></a>

Once the STC Pay integration between Ottu and STC Pay has been completed, the necessary checks are automatically handled by the Checkout SDK to ensure the seamless display of the STC Pay button.

Upon initialization of the Checkout SDK with the [session\_id](android.md#sessionid-string-required) and payment gateway codes ([pg\_codes](../checkout-api.md#pg_codes-array-required)), the following condition is automatically verified:

* The `session_id` and pg\_codes provided during SDK initialization must be linked to the STC Pay Payment Service. This verification ensures that the STC Pay option is made available for selection as a payment method.

Regardless of whether a mobile number has been entered by the customer during transaction creation, the STC Pay button is displayed by the Android SDK.

## [Onsite Checkout](android.md#onsite-checkout)

This payment option facilitates direct payments within the mobile SDK. A user-friendly interface allows users to securely input their CHD. If permitted by the backend, the card can be stored as a tokenized payment for future transactions.

**Example:**

<figure><img src="../../.gitbook/assets/image (96).png" alt="" width="371"><figcaption></figcaption></figure>

The SDK supports multiple instances of onsite checkout payments. Therefore, for each payment method with a PG code of `ottu_pg`, the card form (as shown above) will be displayed.

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Fees are not displayed for onsite checkout instances due to the support of multiple card types through omni PG (multi-card) configurations. The presence of multiple payment icons also indicates this multi-card functionality.
{% endhint %}

## [Error Reporting](android.md#error-reporting) <a href="#error-reporting" id="error-reporting"></a>

The SDK utilizes Sentry for error logging and reporting, with initialization based on the configuration provided by SDK Studio.

Since the SDK is embedded within the merchant's application, conflicts may arise if Sentry is also integrated into the app. To prevent such conflicts, Sentry can be disabled within the Checkout SDK by setting the `is_enabled` flag to `false` in the configuration.

## [Cyber Security Measures](android.md#cyber-security-measures) <a href="#cyber-security-measures" id="cyber-security-measures"></a>

### [Rooting Detection](android.md#rooting-detection)

The SDK prevents execution on rooted devices.

To enforce this restriction, rooting checks are performed during SDK initialization. If a device is detected as rooted, a modal alert dialog is displayed, providing an explanation. The message shown is as follows:

{% hint style="danger" %}
This device is not secure for payments. Transactions are blocked for security reasons.
{% endhint %}

After dismissing the alert, the app crashes unexpectedly

{% hint style="info" %}
&#x20;`Checkout.init` function needs to be called in a coroutine.
{% endhint %}

### [Screen Capture Prevention](android.md#screen-capture-prevention)

The SDK is designed to protect sensitive data by restricting screen capture functionalities. These restrictions apply to the entire Activity that contains the SDK and operate as follows:

* **Screenshot Attempts:**\
  If a user attempts to take a screenshot, a toast message will appear stating:\
  &#xNAN;_"This app doesn’t allow screenshots."_
* **Screen Recording After SDK Initialization:**\
  If screen recording is initiated **after** the SDK has been initialized, the following toast message is displayed:\
  &#xNAN;_"Can't record screen due to security policy."_
* **Screen Recording Before SDK Initialization:**\
  If screen recording begins **before** the SDK is initialized, the entire Activity containing the SDK will appear as a black screen in the recorded video.

## [FAQ](android.md#faq) <a href="#faq" id="faq"></a>

#### :digit\_one: [What forms of payments are supported by the SDK?](android.md#id-1.-what-forms-of-payments-are-supported-by-the-sdk) <a href="#id-1.-what-forms-of-payments-are-supported-by-the-sdk" id="id-1.-what-forms-of-payments-are-supported-by-the-sdk"></a>

The SDK accommodates various payment forms including`tokenPay`, `redirect`, `StcPay` and `cardOnsite`.&#x20;

Merchants have the flexibility to showcase specific methods based on their requirements.&#x20;

For instance, if you wish to exclusively display the STC Pay button, you can achieve this by setting `formsOfPayment` = `[StcPay]`, which will result in only the STC Pay button being displayed. This approach is applicable to other payment methods as well.

#### :digit\_two: [What are the minimum system requirements for the SDK integration?](android.md#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration) <a href="#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration" id="id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration"></a>

It is required to have a device running Android 8 or higher (Android API level 26 or higher).

#### :digit\_three: [Can I customize the appearance beyond the provided themes?](android.md#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes) <a href="#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes" id="id-3.-can-i-customize-the-appearance-beyond-the-provided-themes"></a>

Yes, check the [Customization theme](android.md#customization-theme) section.
