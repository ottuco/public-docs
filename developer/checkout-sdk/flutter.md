# Flutter

The Checkout SDK is a Flutter framework (library) developed by Ottu, designed to seamlessly integrate an Ottu-powered [checkout process](./#ottu-checkout-sdk-flow) into Flutter applications for both iOS and Android platforms. This framework functions as a wrapper over the corresponding native SDKs, ensuring a smooth and efficient payment experience.

With the Checkout SDK, both the visual appearance and the forms of payment available during the checkout process can be fully customized to meet specific requirements.

To integrate the Checkout SDK, the library must be added to the Flutter application and initialized with the following parameters:

* [merchant\_id](../checkout-api.md#session_id-string-mandatory)
* [session\_id](../checkout-api.md#session_id-string-mandatory)
* [API key](../authentication.md#public-key)

Additionally, optional configurations such as the [forms of payment](flutter.md#formsofpayment-array-optional) to be accepted and the [theme](flutter.md#customization-theme) styling for the checkout interface can be specified.

## [Installation](flutter.md#installation)

To install the Flutter SDK plugin, the following section must be added to the **`pubspec.yaml`** file:

```yaml
dependencies:
  flutter:
    sdk: flutter

  ottu_flutter_checkout:
    # To use ottu_flutter_checkout SDK from a local source, uncomment the line below 
    # and comment out the three lines specifying the Git repository.
    # path: ../ottu_flutter_checkout

    git:
      url: https://github.com/ottuco/ottu-flutter.git
      ref: main

```

And then run `flutter pub get` command in the terminal.

After adding the dependency, run the following command in the **terminal** to fetch the required packages: flutter pub get

### [Android](flutter.md#android)

#### **Minimum Requirements**

The SDK can be used on devices running **Android 8 (Android SDK 26)** or higher.

### [iOS](flutter.md#ios)

#### **Minimum Requirements**

The SDK can be used on devices running **iOS 15** or higher.

## [UI](flutter.md#ui)

### [**Android**](flutter.md#android-1)

The SDK UI is embedded as a `fragment` within any part of an `activity` in the merchant's application.

**Example:**

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="375"><figcaption></figcaption></figure>

If only one payment option is available and it is a wallet, the UI is automatically minimized.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

## [SDK Configuration](flutter.md#sdk-configuration)

### [**Language**](flutter.md#language)

The SDK supports two languages: **English** and **Arabic**, with **English** set as the default.

The SDK automatically applies the language based on the device settings, eliminating the need for manual adjustments within the application.

However, if the transaction is created in a different language and setup preload is enabled, texts retrieved from the backend (such as fee descriptions) will be displayed in the transaction language, regardless of the device’s language settings.

To ensure consistency, the current device language should be taken into account when specifying a language code in the transaction creation request of the [Checkout API](../checkout-api.md).

### [**Light and Dark Theme**](flutter.md#light-and-dark-theme)

The SDK supports automatic UI adjustments based on the device's theme settings (light or dark mode).

The appropriate theme is applied during [SDK initialization](flutter.md#sdk-initialization), aligning with the device's configuration. Similar to language settings, no manual adjustments are required within the application.

## [**SDK Initialization**](flutter.md#sdk-initialization)

The Checkout SDK is initialized using the `CheckoutArguments` class, which includes the [properties](flutter.md#properties) listed below.

To initialize the SDK, an instance of `CheckoutArguments` must be passed as an argument to the `OttuCheckoutWidget` object.

For a detailed implementation example, refer to the [Example](flutter.md#example) section.

### [**Properties**](flutter.md#properties)

#### [**merchantId**](flutter.md#merchantid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**`required`**</mark>_

It is used to define the Ottu merchant domain and must be set to the root domain of the Ottu account, excluding the `https://` or `http://` prefix.

For example, if the Ottu URL is `https://example.ottu.com`, the corresponding merchant\_id is `example.ottu.com`.

This property is required to ensure that the checkout process is correctly linked to the associated Ottu merchant account.

#### [**apiKey**](flutter.md#apikey-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**`required`**</mark>_

It is the Ottu [API public key](../authentication.md#public-key), used for authentication when communicating with **Ottu's servers** during the checkout process.

{% hint style="warning" %}
Ensure that only the **public key** is used. The [private key](../authentication.md#private-key-api-key) must remain confidential and must never be shared with any clients.
{% endhint %}

#### [**sessionId**](flutter.md#sessionid-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**`required`**</mark>_

It is a unique identifier for the payment transaction associated with the checkout process.

This identifier is automatically generated when a payment transaction is created. For further details on how to use the `session_id` parameter in the [Checkout API](../checkout-api.md), refer to the [session\_id](flutter.md#sessionid-string-required) documentation.

#### [**formsOfPayment**](flutter.md#formsofpayment-array-optional) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

The `formsOfPayment` parameter is used to customize the forms of payment displayed in the [checkout process](./#ottu-checkout-sdk-flow). By default, all forms of payment are enabled.

**Available options for formsOfPayment:**

* `applePay`: The Apple Pay payment method is supported, allowing purchases to be made using Apple Pay-enabled devices.
* `cardOnsite`: A direct (onsite) payment method, where customers are required to enter their card details directly within the SDK.
* `tokenPay`: A method utilizing [tokenization](../tokenization.md), ensuring that customer payment information is securely stored and processed.
* `redirect`: A payment method where customers are redirected to an external payment gateway or a third-party processor to complete the transaction.
* `stcPay`: A method where customers enter their mobile number and authenticate using an OTP sent to their mobile device.

#### [**setupPreload**](flutter.md#setuppreload-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

An `ApiTransactionDetails` class object is used to store transaction details. If provided, transaction details will not be requested from the backend, thereby reducing processing time.

#### [**theme**](flutter.md#theme-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

A `theme` class object is used for UI customization. All fields are optional and may include values for background colors, text colors, and fonts for various UI components.

For more details, refer to [Android Customization Theme](android.md#customization-theme).

#### [**successCallback, errorCallback, and successCallback**](flutter.md#successcallback-errorcallback-and-successcallback-unit-required) _<mark style="color:blue;">`unit`</mark>_ _<mark style="color:red;">**`required`**</mark>_

Callback functions are used to retrieve the payment status. These must be provided directly to the Checkout initialization function. For more information, please check [here](flutter.md#callbacks).

## [**Callbacks**](flutter.md#callbacks)

The callbacks are handled by the native frameworks. Please see the links here:

* [Android Callbacks](android.md#callbacks)
* [iOS Callbacks](ios.md#callbacks)

It is not necessary to modify anything for the callbacks, as they are managed by the native SDK.

However, the following examples demonstrate how they function on both platforms:

### [Android](flutter.md#android-2)

The `callbacks` are defined within the body of the `Checkout.init` function:

```swift
val sdkFragment = Checkout.init(
  context = checkoutView.context,
  builder = builder,
  setupPreload = apiTransactionDetails,
  successCallback = {
    Log.e("TAG", "successCallback: $it")
    showResultDialog(checkoutView.context, it)
  },
  cancelCallback = {
    Log.e("TAG", "cancelCallback: $it")
    showResultDialog(checkoutView.context, it)
  },
  errorCallback = { errorData, throwable ->
    Log.e("TAG", "errorCallback: $errorData")
    showResultDialog(checkoutView.context, errorData, throwable)
  },

```

### [iOS](flutter.md#ios.1) <a href="#ios.1" id="ios.1"></a>

Here is an example of a delegate:

{% code overflow="wrap" %}
```swift
extension CheckoutPlatformView: OttuDelegate {
    
    public func errorCallback(_ data: [String: Any]?) {
        debugPrint("errorCallback\n")
        DispatchQueue.main.async {
            self.paymentViewController?.view.isHidden = true
            self.paymentViewController?.view.setNeedsLayout()
            self.paymentViewController?.view.layoutIfNeeded()
            self._view.heightHandlerView.setNeedsLayout()
            self._view.heightHandlerView.layoutIfNeeded()
            self._view.setNeedsLayout()
            self._view.layoutIfNeeded()
            
            let alert = UIAlertController(
                title: "Error",
                message: data?.debugDescription ?? "",
                preferredStyle: .alert
            )
            alert.addAction(
                UIAlertAction(title: "OK", style: .cancel)
            )
            debugPrint("errorCallback, show alert\n")
            self.paymentViewController?.present(alert, animated: true)
        }
    }
    
    public func cancelCallback(_ data: [String: Any]?) {
        debugPrint("cancelCallback\n")
        DispatchQueue.main.async {
            var message = ""
            
            if let paymentGatewayInfo = data?["payment_gateway_info"] as? [String: Any],
               let pgName = paymentGatewayInfo["pg_name"] as? String,
               pgName == "kpay" {
                message = paymentGatewayInfo["pg_response"].debugDescription
            } else {
                message = data?.debugDescription ?? ""
            }
            
            self.paymentViewController?.view.isHidden = true
            self.paymentViewController?.view.setNeedsLayout()
            self.paymentViewController?.view.layoutIfNeeded()
            self._view.heightHandlerView.setNeedsLayout()
            self._view.heightHandlerView.layoutIfNeeded()
            self._view.setNeedsLayout()
            self._view.layoutIfNeeded()
            
            let alert = UIAlertController(
                title: "Cancel",
                message: message,
                preferredStyle: .alert
            )
            alert.addAction(
                UIAlertAction(title: "OK", style: .cancel)
            )
            debugPrint("cancelCallback, show alert\n")
            self.paymentViewController?.present(alert, animated: true)
        }
    }
    
    public func successCallback(_ data: [String: Any]?) {
        debugPrint("successCallback\n")
        DispatchQueue.main.async {
            self.paymentViewController?.view.isHidden = true
            self._view.paymentSuccessfullLabel.isHidden = false
            self.paymentViewController?.view.setNeedsLayout()
            self.paymentViewController?.view.layoutIfNeeded()
            self._view.heightHandlerView.setNeedsLayout()
            self._view.heightHandlerView.layoutIfNeeded()
            self._view.setNeedsLayout()
            self._view.layoutIfNeeded()
            
            let alert = UIAlertController(
                title: "Success",
                message: data?.debugDescription ?? "",
                preferredStyle: .alert
            )
            alert.addAction(
                UIAlertAction(title: "OK", style: .cancel)
            )
            debugPrint("successCallback, showing alert\n")
            self.paymentViewController?.present(alert, animated: true)
        }
    }
}

```
{% endcode %}

## [Example](flutter.md#example)

{% code overflow="wrap" %}
```swift
final checkoutArguments = CheckoutArguments(
  merchantId: state.merchantId,
  apiKey: state.apiKey,
  sessionId: state.sessionId ?? "",
  amount: amount,
  showPaymentDetails: state.showPaymentDetails,
  apiTransactionDetails: state.preloadPayload == true ? _apiTransactionDetails : null,
  formsOfPayment: formOfPayments?.isNotEmpty == true ? formOfPayments : null,
  theme: _theme,
);

Scaffold(
  appBar: AppBar(
    backgroundColor: Theme.of(context).colorScheme.inversePrimary,
    title: Text(widget.title),
  ),
  body: SingleChildScrollView(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        SizedBox(height: 46),
        Text(
          "Customer Application",
          textAlign: TextAlign.center,
          style: ts.TextStyle(fontSize: 24),
        ),
        // Start of Merchant content
        const Padding(
          padding: EdgeInsets.all(12.0),
          child: Text(
            "Some users UI elements, Some users UI elements, "
            "Some users UI elements, Some users UI elements, "
            "Some users UI elements",
          ),
        ),
        // End of Merchant content
        Padding(
          padding: const EdgeInsets.all(12.0),
          child: ValueListenableBuilder<int>(
            valueListenable: _checkoutHeight,
            builder: (BuildContext context, int height, Widget? child) {
              return SizedBox(
                height: height.toDouble(),
                child: OttuCheckoutWidget(arguments: widget.checkoutArguments),
              );
            },
          ),
        ),
        const SizedBox(height: 20),
      ],
    ),
  ),
);

```
{% endcode %}

The code samples can be found [here](https://github.com/ottuco/ottu-flutter/tree/main/Sample).

To access the actual **code samples**, navigate to the **`lib`** directory.

Additionally, the **`android`** and **`ios`** directories contain **platform-specific code** relevant to each respective environment.

## [Customization Theme](flutter.md#customization-theme)

The class responsible for defining the theme is called `CheckoutTheme`. It utilizes additional component classes, including:

* `ButtonComponent`
* `LabelComponent`
* `TextFieldComponent`

The `CheckoutTheme` class consists of objects representing various UI components. While the component names generally align with those listed above, they also contain platform-specific fields for further customization.

{% embed url="https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG?content-scaling=fixed&fuid=819188572429138268&kind=proto&node-id=1-624&scaling=scale-down" %}

Below is the structure of the Customization `theme` class:

```swift
class CheckoutTheme extends Equatable {
  @_UiModeJsonConverter()
  final CustomerUiMode uiMode;
  
  final TextStyle? mainTitleText;
  final TextStyle? titleText;
  final TextStyle? subtitleText;
  final TextStyle? feesTitleText;
  final TextStyle? feesSubtitleText;
  final TextStyle? dataLabelText;
  final TextStyle? dataValueText;
  final TextStyle? errorMessageText;

  final TextFieldStyle? inputTextField;

  final ButtonComponent? button;
  final RippleColor? backButton;
  final ButtonComponent? selectorButton;
  final SwitchComponent? switchControl;
  final Margins? margins;

  final ColorState? sdkBackgroundColor;
  final ColorState? modalBackgroundColor;
  final ColorState? paymentItemBackgroundColor;
  final ColorState? selectorIconColor;
  final ColorState? savePhoneNumberIconColor;
}

```

#### [**uiMode**](flutter.md#uimode)

Specifies the device `theme` mode, which can be set to one of the following:

* `light` – Forces the UI to use light mode.
* `dark` – Forces the UI to use dark mode.
* `auto` – Adapts the UI based on the device's system settings.

### [Properties Description](flutter.md#properties-description) <a href="#properties-description" id="properties-description"></a>

All properties in the `CheckoutTheme` class are optional, allowing users to customize any of them as needed.

If a property is not set, the default value (as specified in the Figma design [here](https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG?content-scaling=fixed\&kind=proto\&node-id=1-624\&scaling=scale-down)) will be applied automatically.

#### [**Texts**](flutter.md#texts)

#### **General**

| Property Name   |                            Description                            |        Data Type        |
| --------------- | :---------------------------------------------------------------: | :---------------------: |
| `mainTitleText` |                 Font and color for all “Captions”                 | [Text](flutter.md#text) |
| `titleText`     |           Font and color for payment options in the list          | [Text](flutter.md#text) |
| `subtitleText`  | Font and color for payment options details (like expiration date) | [Text](flutter.md#text) |

#### **Fees**

| Property Name      |                           Description                          |        Data Type        |
| ------------------ | :------------------------------------------------------------: | :---------------------: |
| `feesTitleText`    |    Font and color of fees value in the payment options list    | [Text](flutter.md#text) |
| `feesSubtitleText` | Font and color of fees description in the payment options list | [Text](flutter.md#text) |

#### **Data**

| Property Name   |                        Description                       |        Data Type        |
| --------------- | :------------------------------------------------------: | :---------------------: |
| `dataLabelText` | Font and color of payment details fields (like “Amount”) | [Text](flutter.md#text) |
| `dataValueText` |         Font and color of payment details values         | [Text](flutter.md#text) |

**Other**

| Property Name      |                   Description                   |        Data Type        |
| ------------------ | :---------------------------------------------: | :---------------------: |
| `errorMessageText` | Font and color of error message text in pop-ups | [Text](flutter.md#text) |

#### [**Text Fields**](flutter.md#text-fields)

| Property Name    |                              Description                             |             Data Type             |
| ---------------- | :------------------------------------------------------------------: | :-------------------------------: |
| `inputTextField` | Font and color of text in any input field (including disabled state) | [TextField](flutter.md#textfield) |

#### [**Colors**](flutter.md#colors)

| Property Name                |                       Description                      |         Data Type         |
| ---------------------------- | :----------------------------------------------------: | :-----------------------: |
| `sdkbackgroundColor`         |      The main background of the SDK view component     | [Color](flutter.md#color) |
| `modalBackgroundColor`       |           The background of any modal window           | [Color](flutter.md#color) |
| `paymentItemBackgroundColor` |    The background of an item in payment options list   | [Color](flutter.md#color) |
| `selectorIconColor`          |          The color of the icon of the payment          | [Color](flutter.md#color) |
| `savePhoneNumberIconColor`   | The color of “Diskette” button for saving phone number | [Color](flutter.md#color) |

#### [**Buttons**](flutter.md#buttons)

| Property Name    |                            Description                            |               Data Type               |
| ---------------- | :---------------------------------------------------------------: | :-----------------------------------: |
| `button`         |           Background, text color and font for any button          |      [Button](flutter.md#button)      |
| `backButton`     |               Color of the “Back” navigation button               | [RippleColor](flutter.md#ripplecolor) |
| `selectorButton` | Background, text color and font for payment item selection button |      [Button](flutter.md#button)      |

#### [**Switch**](flutter.md#switch)

| Property Name |                                        Description                                        |           Data Type           |
| ------------- | :---------------------------------------------------------------------------------------: | :---------------------------: |
| `switch`      | Colors of the switch background and its toggle in different states (on, off and disabled) | [Switch](flutter.md#switch-1) |

#### [**Margins**](flutter.md#margins)



| Property Name |                      Description                      |          Data Type          |
| ------------- | :---------------------------------------------------: | :-------------------------: |
| margins       | Top, left, bottom and right margins between component | [Margin](flutter.md#margin) |

### [Data types description](flutter.md#data-types-description) <a href="#data-types-description" id="data-types-description"></a>

#### [**Color**](flutter.md#color)

| Property Name   |             Description             | Data Type |
| --------------- | :---------------------------------: | :-------: |
| `color`         |       Main color integer value      |    Int    |
| `colorDisabled` | Disabled stated color integer value |    Int    |

#### [**RippleColor**](flutter.md#ripplecolor)

| Property Name  |             Description             | Data Type |
| -------------- | :---------------------------------: | :-------: |
| `color`        |       Main color integer value      |    Int    |
| `rippleColor`  |      Ripple color integer value     |    Int    |
| `colorDisaled` | Disabled stated color integer value |    Int    |

#### [**Text**](flutter.md#text)

| Property Name |        Description       |         Data Type         |
| ------------- | :----------------------: | :-----------------------: |
| `textColor`   | Main color integer value | [Color](flutter.md#color) |
| `fontType`    |     Font resource ID     |            Int            |

#### [**TextField**](flutter.md#textfield)

|  Property Name | Description                    |         Data Type         |
| :------------: | ------------------------------ | :-----------------------: |
|  `background`  | Background color integer value | [Color](flutter.md#color) |
| `primaryColor` | Text color                     | [Color](flutter.md#color) |
| `focusedColor` | Selected text color            | [Color](flutter.md#color) |
|     `text`     | Text value                     |  [Text](flutter.md#text)  |
|     `error`    | Text value                     |  [Text](flutter.md#text)  |

#### [Button](flutter.md#button)

| Property Name |       Description       |               Data Type               |
| ------------- | :---------------------: | :-----------------------------------: |
| `rippleColor` | Button background color | [RippleColor](flutter.md#ripplecolor) |
| `fontType`    |   Button text font ID   |                  Int                  |
| `textColor`   |    Button text color    |       [Color](flutter.md#color)       |

#### [Switch](flutter.md#switch-1)

| Property Name                   |             Description             | Data Type |
| ------------------------------- | :---------------------------------: | :-------: |
| `checkedThumbTintColor`         |    Toggle color in checked state    |    Int    |
| `uncheckedThumbTintColor`       |   Toggle color in unchecked state   |    Int    |
| `checkedTrackTintColor`         |     Track color in checked state    |    Int    |
| `uncheckedTrackTintColor`       |    Track color in unchecked state   |    Int    |
| `checkedTrackDecorationColor`   |  Decoration color in checked state  |    Int    |
| `uncheckedTrackDecorationColor` | Decoration color in unchecked state |    Int    |

#### [**Margin**](flutter.md#margin)

| Property Name | Data Type |
| ------------- | :-------: |
| `left`        |    Int    |
| `top`         |    Int    |
| `right`       |    Int    |
| `bottom`      |    Int    |

### [Example](flutter.md#example.1) <a href="#example.1" id="example.1"></a>

To build the `theme`, the user must follow similar steps as described in the corresponding file of the test app.

Here is a **code snippet** demonstrating the process:

```swift
final checkoutTheme = ch.CheckoutTheme(
  uiMode: ch.CustomerUiMode.dark,
  titleText: ch.TextStyle(),
  modalBackgroundColor: ch.ColorState(color: Colors.amber));
```

## [STC Pay](flutter.md#stc-pay)

If the STC Pay integration between Ottu and STC Pay has been completed, the Checkout SDK will automatically handle the necessary checks to display the STC Pay button seamlessly.

When the Checkout SDK is initialized with the [session\_id](../checkout-api.md#session_id-string-mandatory) and payment gateway codes ([pg\_codes](../checkout-api.md#pg_codes-array-required)), the SDK will verify the following conditions:

* The `session_id` and `pg_codes` provided during initialization must be associated with the STC Pay Payment Service. This ensures that the STC Pay option is available for the customer.
* In the Android SDK, the STC Pay button is displayed regardless of whether the customer has entered a mobile number while creating the transaction.

## [**KNET - Apple Pay** ](flutter.md#knet-apple-pay)

Due to compliance requirements, for iOS, the KNET payment gateway requires a popup notification displaying the payment result after each failed payment. This notification is triggered only in the cancelCallback, but only if a response is received from the payment gateway.

As a result, the user cannot retry the payment without manually clicking on Apple Pay again.

{% hint style="info" %}
The popup notification mentioned above is specific to the KNET payment gateway.Other payment gateways may have different requirements or notification mechanisms, so it is essential to follow the respective documentation for each integration.
{% endhint %}

To properly handle the KNET popup notification, the following `Swift` code snippet must be implemented into the payment processing flow:

{% hint style="info" %}
This is only iOS-related stuff, so the callbacks are native and so they are in Swift language.
{% endhint %}

{% code overflow="wrap" %}
```swift
func cancelCallback(_ data: [String: Any]?) {
    var message = ""
    
    if let paymentGatewayInfo = data?["payment_gateway_info"] as? [String: Any],
       let pgName = paymentGatewayInfo["pg_name"] as? String,
       pgName == "kpay" {
        message = paymentGatewayInfo["pg_response"].debugDescription
    } else {
        message = data?.debugDescription ?? ""
    }
    
    navigationController?.popViewController(animated: true)
    
    let alert = UIAlertController(
        title: "Cancel",
        message: message,
        preferredStyle: .alert
    )
    
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
}

```
{% endcode %}

#### **Function Breakdown**

The above code performs the following checks and actions:

1. Checks if the `cancelCallback` object contains payment gateway information
   * It verifies whether the `payment_gateway_info` field is available in the response.
2. Identifies if the payment gateway used is `KNET`
   * It checks if the `pg_name` property equals `kpay`, confirming that the transaction was processed using KNET.
3. Check the above two conditions are met by retrieving the payment gateway response
   * If the gateway response (`pg_response`) is available, it is displayed; otherwise, a default message (`Payment was cancelled.`) is used.
4. Navigates back and displays an alert
   * The user is returned to the previous screen (`navigationController?.popViewController(animated: true)`).
   * A popup notification is displayed using `self.present(alert, animated: true)`, informing the user about the failed payment.

## [**Onsite Checkout**](flutter.md#onsite-checkout)

The Onsite Checkout payment option enables direct payments to be performed within the mobile SDK.

The SDK provides a UI where the user can enter their Cardholder Data (CHD). Additionally, if allowed by the backend, the user has the option to save the card for future payments, storing it as a tokenized payment.

The onsite checkout screen appears identical to its counterpart on native platforms.

#### Android Example

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt="" width="247"><figcaption></figcaption></figure>

#### iOS&#x20;

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt="" width="375"><figcaption></figcaption></figure>

## [**Error Reporting**](flutter.md#error-reporting)

The SDK utilizes Sentry for error logging and reporting. It is initialized based on the configuration provided by SDK Studio.

However, since the SDK is a framework embedded within the merchant's app, conflicts may arise if the app also integrates Sentry.

To prevent conflicts, the merchant can disable Sentry within the Checkout SDK by setting the `is_enabled` flag to `false` in the configuration.

## [**Cyber Security Measures**](flutter.md#cyber-security-measures)

### [**Rooting and Jailbreak Detection**](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/~/changes/796/developer/checkout-sdk/flutter#rooting-and-jailbreak-detection)

The Flutter SDK does not perform rooting or jailbreak detection independently. Instead, these security checks are entirely handled by the native SDKs.

For more details, refer to the following links:

[Android  ](android.md#rooting-detection)

[iOS](ios.md#jailbreak-detection)

## [**FAQ**](flutter.md#faq)

#### :digit\_one:[**What forms of payment are supported by the SDK?**](flutter.md#what-forms-of-payment-are-supported-by-the-sdk)

The SDK supports the following [forms of payment](flutter.md#formsofpayment-array-optional):

* `tokenPay`
* `redirect`
* `stcPay`
* `cardOnsite`
* `applePay` _(iOS only)_

Merchants can configure the forms of payment displayed according to their needs.

For example, to **display only the STC Pay button**, use:

```dart
formsOfPayment = [stcPay]
```

This ensures that only the **STC Pay** button is shown. The same approach applies to other payment methods.

#### :digit\_two:[**What are the minimum system requirements for SDK integration?**](flutter.md#what-are-the-minimum-system-requirements-for-sdk-integration)

The SDK requires a device running:

* **Android 8** or higher (**API level 26** or higher)
* **iOS 14** or higher

#### :digit\_three:[**Can I customize the appearance beyond the provided themes?**](flutter.md#can-i-customize-the-appearance-beyond-the-provided-themes)

Yes, customization is supported. For more details, refer to the [Customization Theme](flutter.md#customization-theme) section.
