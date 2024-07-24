# Android

The [Checkout SDK](./) from Ottu is a Kotlin-based library designed to simplify the integration of an Ottu-powered [checkout process](./#ottu-checkout-sdk-flow) into your Android app. This SDK enables you to tailor the appearance and functionality of your checkout process, including the selection of accepted payment methods.

The Checkout SDK must be incorporated into your Android application, and it must be initialized using your Ottu [merchant\_id](web.md#merchant\_id-string), [session\_id](web.md#session\_id-string), and [API public key](../authentication.md#public-key). Various settings, such as the accepted payment methods and the [theme](https://docs.ottu.com/developer/checkout-sdk/web#theme-object) styling for the checkout interface, may also be specified.

{% hint style="warning" %}
The [API private key](../authentication.md#private-key-api-key) should never be utilized on the client side; instead, use the [API public key](../authentication.md#public-key). This is essential for maintaining the security of your application and safeguarding sensitive data.
{% endhint %}

## [Installation](android.md#installation) <a href="#installation" id="installation"></a>

### [Minimum Requirements](android.md#minimum-requirements) <a href="#minimum-requirements" id="minimum-requirements"></a>

The SDK can be used on a device running Android 8 or higher (API version 26 or higher).

### [Installation with dependency](android.md#installation-with-dependency) <a href="#installation-with-dependency" id="installation-with-dependency"></a>

```swift
allprojects {
    repositories {
        ...
        maven {
            url '<https://jitpack.io>'
        }
    }
}

dependencies {
    implementation 'com.github.ottuco:ottu-android-checkout:1.0.4'
}
```

## [Native UI](android.md#native-ui) <a href="#native-ui" id="native-ui"></a>

The SDK UI is a `Fragment` embedded in any part of any `Activity` of the merchant's app.\
Here is the example:

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

However, if there’s only one payment option available and it it a wallet, the UI is minified:

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

## [SDK Configuration](android.md#sdk-configuration) <a href="#sdk-configuration" id="sdk-configuration"></a>

### [Language](android.md#language) <a href="#language" id="language"></a>

The SDK supports two languages, English and Arabic, with English set as the default.

It automatically adopts the language configured in the device settings, requiring no in-app adjustments. However, if a transaction is initiated in a different language and setup preload is utilized, the backend-generated text (such as fee descriptions) will appear in the language of the transaction. Therefore, it is important to ensure that the language code passed to the [Checkout API](../checkout-api.md)'s transaction creation request matches the currently selected language on the device or current selected app language.

### [Light and dark theme](android.md#light-and-dark-theme) <a href="#light-and-dark-theme" id="light-and-dark-theme"></a>

The SDK also supports UI customization to match the device theme—light or dark. This adjustment is applied during the SDK initialization, based on the device's settings. Similarly, for language, no adjustments are made within the app.

## [Functions](android.md#functions) <a href="#functions" id="functions"></a>

Currently, the SDK offers a single function that serves as the entry point for the merchant's app. It also includes callbacks that must be managed by the parent app, which are detailed in the following chapter.

### [**Checkout.init**](https://docs.ottu.com/developer/checkout-sdk/web#checkout.init)

The function initiates the checkout process and configures the required settings for the Checkout SDK. It should be invoked once by the parent app to start the checkout sequence, called with set of configuration fields that encapsulate all essential options for the process.

When you call `Checkout.init`, the SDK manages the setup of key components for the checkout, like generating a form for customers to input their payment information, and facilitating the communication with Ottu's servers to process the payment.

This function returns a `Fragment` object, which is a native Android UI component that can be integrated into any part of an `Activity` instance (also native to Android).

### [Properties](android.md#properties) <a href="#properties" id="properties"></a>

#### [**merchantId**](android.md#merchantid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `merchant_id` identifies your Ottu merchant domain. It should be the root domain of your Ottu account, excluding the "https://" or "http://" prefix.

For instance, if your Ottu URL is https://example.ottu.com, then your `merchant_id` would be example.ottu.com. This attribute is utilized to determine which Ottu merchant account to associate with the checkout process.

#### [**apiKey**](android.md#apikey-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `apiKey` is your Ottu [API public key](../authentication.md#public-key), which is essential for authenticating communications with Ottu's servers during the checkout process. As outlined in the REST API documentation, the `apiKey` property should be assigned your Ottu API public key.

Make sure to use the public key and avoid using the private key. The [API private key](../authentication.md#private-key-api-key) must be kept confidential at all times and should never be shared with any clients.

#### [**sessionId**](android.md#sessionid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The `session_id` serves as the unique identifier for the payment transaction linked to the checkout process.

This identifier is automatically generated at the creation of the payment transaction. For additional details on how to utilize the `session_id` parameter in the Checkout API, refer to the [session\_id](broken-reference) section.&#x20;

#### [**formsOfPayment**](android.md#formsofpayment-string-required) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `formsOfPayment`  allows you to select which payment methods will appear during your checkout process. By default, all available payment options are enabled.

The options for `formsOfPayment` include:

* `ottuPG`: This method redirects customers to a page where they can enter their credit or debit card details to make a payment.
* `tokenPay`: This payment method utilizes [tokenization](../tokenization.md) to securely store and process customers' payment details.
* `redirect`: This method redirects customers to a payment gateway or a third-party payment processor for payment completion.
* `stcPay`: In this method, customers use their mobile number and confirm the transaction with an OTP sent to their mobile to finalize their payment.

#### [**setupPreload**](android.md#setuppreload-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The `ApiTransactionDetails` class object contains the transaction details. If this object is provided, the SDK will not need to retrieve the transaction details from the backend, thereby saving time.

#### [**theme**](android.md#theme-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

`Theme` class object for UI customization. All the fields are optional. Can contain values for background colors, text colors, fonts for various components.. See [Customization Theme](android.md#customization-theme) section for more details.

Please note that `theme` is optional. If not provided, the default UI settings will be used.

#### [**successCallback, errorCallback and successCallback**](android.md#successcallback-errorcallback-and-successcallback-uint-required) _<mark style="color:blue;">`Uint`</mark>_ _<mark style="color:red;">`required`</mark>_

The callback functions used for getting payment status. They should be provided directly to the Checkout initialization function. See [Callacks](android.md#callbacks) for more information.

## [Callbacks](android.md#callbacks) <a href="#callbacks" id="callbacks"></a>

In the Checkout SDK, callback functions are essential for delivering real-time updates on the status of payment transactions. These callbacks improve the user experience by facilitating smooth and effective management of different payment scenarios, including errors, successful transactions, and cancellations.&#x20;

In the Checkout SDK, callback functions are essential for delivering real-time updates on the status of payment transactions. These callbacks improve the user experience by facilitating smooth and effective management of different payment scenarios, including errors, successful transactions, and cancellations.&#x20;

{% hint style="info" %}
The callbacks outlined below are applicable to any type of payment.
{% endhint %}

### [**errorCallback**](android.md#errorcallback)

The `errorCallback` is a callback function triggered when issues occur during a payment process. Properly handling these errors is essential for maintaining a smooth user experience.&#x20;

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

### [**successCallback**](android.md#successcallback)

The `successCallback` is a function that is triggered when the payment process is successfully completed. This callback receives a `JSONObject` containing a `data.status` value of "success."

**Params Available in** `data` **`JSONObject` for** `successCallback`

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
    .formsOfPayments(formsOfPayment)
    .theme(theme)
    .logger(Checkout.Logger.INFO)
    .build()

// Actual `init` function calling, returning a `Fragment` object  
checkoutFragment = Checkout.init(
    context = this @CheckoutSampleActivity,
    builder = builder,
    setupPreload = setupPreload,
    successCallback = {
        Log.e("TAG", "successCallback: $it")
        showResultDialog(it)
    },
    cancelCallback = {
        Log.e("TAG", "cancelCallback: $it")
        showResultDialog(it)
    },
    errorCallback = {
        errorData,
        throwable - >
        Log.e("TAG", "errorCallback: $errorData")
        showResultDialog(errorData, throwable)
    },
)
```

## [Customization Theme](android.md#customization-theme) <a href="#customization-theme" id="customization-theme"></a>

The class describing theme is called `CheckoutTheme`.

Here’s how the customization theme class looks like:

```swift
class CheckoutTheme(
    val uiMode: UiMode = UiMode.AUTO,
    val appearanceLight: Appearance ? = null,
    val appearanceDark : Appearance ? = null,
    val showPaymentDetails : Boolean = true,
)
```

#### [uiMode](android.md#uimode)

Determines device theme: light, dark or auto.

#### [**appearanceLight & appearanceDark**](android.md#appearancelight-and-appearancedark)

These are optional instances of `Appearance` class (see description below). They allow UI customization for device light and dark mode respectively.

`Appearance` class is the main inner class of `CheckoutTheme`. It contains objects describing different UI components. The names of the components mostly correspond to described in this document:

{% embed url="https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG/Ottu-SDK---Components-Documentation?node-id=1-624&t=VdaigMvefTae0naX-1" %}

#### [**showPaymentDetails**](android.md#showpaymentdetails)

Is a boolean field determining whether “Payment details” section should be displayed or hidden.

### [Properties description](android.md#properties-description) <a href="#properties-description" id="properties-description"></a>

**Important Note:**All properties are optional and customizable by the user. If a property is not specified, the default value, as outlined in the above Figma design, will be used.

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

| Property Name      |                   Description                   |        Data Type        |
| ------------------ | :---------------------------------------------: | :---------------------: |
| `errorMessageText` | Font and color of error message text in pop-ups | [Text](android.md#text) |

#### [**Text Fields**](android.md#text-fields)

| Property Name    |                              Description                             |             Data Type             |
| ---------------- | :------------------------------------------------------------------: | :-------------------------------: |
| `inputTextField` | Font and color of text in any input field (including disabled state) | [TextField](android.md#textfield) |

#### [**Colors**](android.md#colors)

| Property Name                |                       Description                      |         Data Type         |
| ---------------------------- | :----------------------------------------------------: | :-----------------------: |
| `sdkbackgroundColor`         |      The main background of the SDK view component     | [Color](android.md#color) |
| `modalBackgroundColor`       |           The background of any modal window           | [Color](android.md#color) |
| `paymentItemBackgroundColor` |    The background of an item in payment options list   | [Color](android.md#color) |
| `selectorIconColor`          |          The color of the icon of the payment          | [Color](android.md#color) |
| `savePhoneNumberIconColor`   | The color of “Diskette” button for saving phone number | [Color](android.md#color) |

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

To construct the theme, the user needs to carry out actions similar to those detailed in this test app file.

Below is the code snippet:

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

## [STC Pay](android.md#stc-pay) <a href="#stc-pay" id="stc-pay"></a>

Once the STC Pay [integration](web.md#stc-pay) between Ottu and STC Pay has been completed, the Checkout SDK will automatically handle the necessary checks to seamlessly display the STC Pay button. Upon initialization of the Checkout SDK with your `session_id` and payment gateway codes (`pg_codes`), the following conditions are verified automatically:

The `session_id` and `pg_codes` supplied during SDK initialization must be linked to the STC Pay Payment Service. This verification ensures that the STC Pay option is available for the customer to select as a payment method.

Regardless of whether a mobile number has been provided by the customer during transaction creation, the STC Pay button is displayed by the Android SDK.

## [Error Reporting](android.md#error-reporting) <a href="#error-reporting" id="error-reporting"></a>

The SDK uses `Sentry` for error logging and reporting. It is initialized based on the config coming from SDK Studio. However, since the SDK is a framework been embedded to the merchant app it can cause conflicts in case the app also uses Sentry. So the merchant can disable Sentry in the Checkout SDK by setting `is_enabled` flag to `false` in the config.

## [FAQ](android.md#faq) <a href="#faq" id="faq"></a>

#### :digit\_one: [What forms of payments are supported by the SDK?](android.md#id-1.-what-forms-of-payments-are-supported-by-the-sdk) <a href="#id-1.-what-forms-of-payments-are-supported-by-the-sdk" id="id-1.-what-forms-of-payments-are-supported-by-the-sdk"></a>

The SDK accommodates various payment forms including tokenPay, ottuPG, redirect, and stcPay. Merchants have the flexibility to showcase specific methods based on their requirements.&#x20;

**For instance**, if you wish to exclusively display the STC Pay button, you can achieve this by setting `formsOfPayment = [stcPay]`, which will result in only the STC Pay button being displayed. This approach is applicable to other payment methods as well.

#### :digit\_two: [What are the minimum system requirements for the SDK integration?](android.md#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration) <a href="#id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration" id="id-2.-what-are-the-minimum-system-requirements-for-the-sdk-integration"></a>

It is required to have a device running Android 8 or higher (Android API level 26 or higher).

#### :digit\_three: [Can I customize the appearance beyond the provided themes?](android.md#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes) <a href="#id-3.-can-i-customize-the-appearance-beyond-the-provided-themes" id="id-3.-can-i-customize-the-appearance-beyond-the-provided-themes"></a>

Yes, check the [Customization theme](android.md#customization-theme) section.
