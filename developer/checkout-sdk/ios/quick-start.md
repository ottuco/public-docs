# Quick Start

This guide walks you through installing and integrating the Ottu Checkout iOS SDK into your iOS project.\
Choose your installation **path** and follow the journey step by step.

## [Video Tutorial: iOS SDK Integration](quick-start.md#video-tutorial-ios-sdk-integration)

This video walks you step-by-step through the iOS SDK integration process. Watch it to quickly understand setup, configuration, and key features in action.

{% embed url="https://drive.google.com/file/d/1XBbI_vF_ArBbVqYEsIyXERkaQtPi702k/view?usp=sharing" %}

## [Prerequisites](quick-start.md#prerequisites)

Before integrating the Checkout SDK, please review the following requirements:

### [Apple Pay Project Settings](quick-start.md#apple-pay-project-settings)

If your app should support **Apple Pay**:

1. Enable **Apple Pay capability** in:\
   `Xcode > Targets > Signing & Capabilities`
2. Add **Apple Pay Merchant IDs (MID)** in project settings

### [iOS Supported Versions](quick-start.md#ios-supported-versions)

* Since the **minimum supported iOS version** is **14**, the **primary demo application** is implemented using **UIKit framework** (SwiftUI is recommended starting from iOS 15).\
  &#x20;[ottu-ios/Example at main · ottuco/ottu-ios](https://github.com/ottuco/ottu-ios/tree/main/Example?utm_source=chatgpt.com)
* However, there’s also a **minimalistic SwiftUI-based demo app**:\
  [ottu-ios/Example\_SwiftUI at main · ottuco/ottu-ios](https://github.com/ottuco/ottu-ios/tree/main/Example_SwiftUI?utm_source=chatgpt.com)

## [Installation](quick-start.md#installation)

You can install the SDK via two paths:

* **Path One:** Swift Package Manager (Recommended)
* **Path Two:** CocoaPods (Deprecated)

### [Path One – Install via Swift Package Manager (Recommended)](quick-start.md#path-one-install-via-swift-package-manager-recommended)

Add Ottu SDK as a dependency in `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/ottuco/ottu-ios.git", from: "2.1.4")
]
```

Or in Xcode:

1. Open your project
2. Go to **Project > Targets > Package Dependencies**
3. Add `ottu-ios` as a dependency

### [Path Two – Install via CocoaPods (Deprecated)](quick-start.md#path-two-install-via-cocoapods-deprecated)

Add the following line to your **Podfile**:

{% code overflow="wrap" %}
```ruby
pod 'ottu_checkout_sdk', :git => 'https://github.com/ottuco/ottu-ios.git', :tag => '2.1.4'
```
{% endcode %}

**Run:**

```bash
pod install
```

{% hint style="success" %}
When ottu\_checkout\_sdk is added to the Podfile, the GitHub repository must also be specified as follows:

{% code overflow="wrap" %}
```bash
pod 'ottu_checkout_sdk', :git => 'https://github.com/ottuco/ottu-ios'
```
{% endcode %}
{% endhint %}

{% hint style="warning" %}
If you see _“could not find compatible versions for pod”_, run:

```bash
pod repo update
```
{% endhint %}

## [Integrate Checkout SDK to the Source Code](quick-start.md#integrate-checkout-sdk-to-the-source-code)

Once the SDK is installed (via [Path One](quick-start.md#path-one-install-via-swift-package-manager-recommended) or [Path Two](quick-start.md#path-two-install-via-cocoapods-deprecated)), follow these steps to integrate it into your app.

{% stepper %}
{% step %}
**Import the SDK**

In your `ViewController.swift` (or any file responsible for presenting the SDK):

```swift
import ottu_checkout_sdk
```
{% endstep %}

{% step %}
**Conform to** `OttuDelegate`

Your view controller must implement the `OttuDelegate` protocol:

```swift
class ViewController: UIViewController, OttuDelegate 
```
{% endstep %}

{% step %}
**Declare a Checkout Member**

Inside your `ViewController`

```swift
private var checkout: Checkout?
```
{% endstep %}

{% step %}
**Initialize Checkout**

Inside `viewDidLoad`, initialize the Checkout SDK:

```swift
do {
    self.checkout = try Checkout(
        displaySettings: PaymentOptionsDisplaySettings(
          mode: PaymentOptionsDisplaySettings.PaymentOptionsDisplayMode.list,
        ),
        sessionId: sessionId,
        merchantId: merchantId,
        apiKey: apiKey,
        delegate: self
    )
} catch let error as LocalizedError {
    print(error)
    return
} catch {
    print("Unexpected error: \(error)")
    return
}
```

**Required parameters**

* `sessionId` → ID of the created transaction
* `merchantId` → Same as the domain address in API requests
* `apiKey` → Public API key for authorization
* `delegate` → Callback listener (usually `self`)

**Recommended parameters**

* `formsOfPayment` → Defines available payment methods
* `setupPreload` → Preloads transaction details for faster initialization

> `setupPreload` comes from the transaction creation response.\
> Passing it prevents the SDK from re-fetching transaction details, reducing initialization time by **several seconds**. It is a decoded JSON object to a `TransactionDetails` object.\
> See an example: [MainViewController.swift](https://github.com/ottuco/ottu-ios/blob/main/Example/OttuApp/MainViewController.swift#L259)

The simplest version of the initialization looks like this:

```swift
do {
    self.checkout =
        try Checkout(
            sessionId: sessionId,
            merchantId: merchantId,
            apiKey: apiKey,
            delegate: self
        )
} catch
let error as LocalizedError {
    // display an error here
    return
} catch {
    print("Unexpected error: \(error)")
    return
}

```
{% endstep %}

{% step %}
**Add the Payment View**

Still inside `viewDidLoad`, embed the payment view into your container:

{% code overflow="wrap" %}
```swift
if let paymentVC = self.checkout?.paymentViewController(),
    let paymentView = paymentVC.view {

        self.addChild(paymentVC)
        paymentVC.didMove(toParent: self)
        view.addSubview(paymentView)
    }
```
{% endcode %}
{% endstep %}
{% endstepper %}

> This is a **basic example** that adds the Checkout view without handling dimensions.\
> For proper layout with constraints, refer to the demo app: [OttuPaymentsViewController.swift](https://github.com/ottuco/ottu-ios/blob/main/Example/OttuApp/OttuPaymentsViewController.swift#L91)

## [Callbacks](quick-start.md#callbacks)

In order `ViewController` to be compliant to `OttuDelegate`, add the following functions to `ViewController` class:

{% code overflow="wrap" %}
```swift
func errorCallback(_ data: [String : Any]?) {
    navigationController?.popViewController(animated: true)
    let alert = UIAlertController(title: "Error", message: data?.debugDescription ?? "", preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
}
    
func cancelCallback(_ data: [String : Any]?) {
    var message = ""
       
    if let paymentGatewayInfo = data?["payment_gateway_info"] as? [String : Any],
       let pgName = paymentGatewayInfo["pg_name"] as? String,
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
   
func successCallback(_ data: [String : Any]?) {
    navigationController?.popViewController(animated: true)
    let alert = UIAlertController(title: "Success", message: data?.debugDescription ?? "", preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    present(alert, animated: true)
}
```
{% endcode %}

### **Callback behavior:**

* **Error** → Displays an error alert and navigates back
* **Cancel** → Displays cancel reason (custom handling for `kpay`)
* **Success** → Displays success confirmation

> This code describes callbacks to be handled by the parent app.

## Advanced Features

* [Forms of Payment](https://docs.ottu.com/developer/checkout-sdk/ios#formsofpayment-string-required)
* [Customization Theme](https://docs.ottu.com/developer/checkout-sdk/ios#customization-theme)
* [Setup Preload](https://docs.ottu.com/developer/checkout-sdk/ios#setuppreload-object-optional)
