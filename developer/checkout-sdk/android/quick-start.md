# Quick Start

This guide outlines the essential steps for integrating **Ottu Android Checkout SDK** into your mobile application. It provides a concise flow from setup to callback handling, helping developers achieve a seamless checkout experience.

## [Video Tutorial: Android SDK Integration](quick-start.md#video-tutorial-android-sdk-integration)

This video guides you step-by-step through the **Android SDK integration process**. Watch it to quickly learn how to set up, configure, and see the key features in action.

{% embed url="https://drive.google.com/file/d/1UkZQSrF8rPJDguU85akAdsfkQ1sPY-rF/view?usp=sharing" %}

## [SDK Installation](quick-start.md#sdk-installation)

{% stepper %}
{% step %}
#### **Add JitPack Repository**

In your **`settings.gradle.kts`** file, add [JitPack](http://jitpack.io/) to the repositories section under `dependencyResolutionManagement`:

```swift
maven { url = uri("https://jitpack.io") }
```
{% endstep %}

{% step %}
#### **Add the SDK Dependency**

Include the Ottu Checkout SDK in your **app-level** `build.gradle.kts` file:

```swift
implementation("com.github.ottuco:ottu-android-checkout:2.1.6")
```
{% endstep %}
{% endstepper %}

## [Integrate the Checkout SDK](quick-start.md#integrate-the-checkout-sdk)

{% stepper %}
{% step %}
Add FrameLayout to the Layout File

Inside `activity_main.xml` (located in the `res/layout` folder), add a `FrameLayout` with the ID `ottuPaymentView`:  `android:id="@+id/ottuPaymentView"`
{% endstep %}

{% step %}
Import Required Modules

Usually, Android Studio automatically imports the necessary modules.\
However, for reference, the Ottu SDK requires the following imports:

```swift
import com.ottu.checkout.Checkout
import com.ottu.checkout.network.model.payment.ApiTransactionDetails
import com.ottu.checkout.ui.base.CheckoutSdkFragment
```
{% endstep %}

{% step %}
Extend the Activity Class

Ensure that the Activity integrating with the Checkout SDK inherits from `AppCompatActivity`:

```swift
class MainActivity : AppCompatActivity() {
```
{% endstep %}

{% step %}
Declare Checkout Fragment Variable

Add a member variable for the `CheckoutSdkFragment` object:

```swift
private var checkoutFragment: CheckoutSdkFragment? = null
```
{% endstep %}

{% step %}
Initialize Checkout SDK

Inside the `onCreate` function, add the SDK initialization code:

{% code overflow="wrap" %}
```swift
// populate the fields below with correct values
val merchantId = "" // your merchant base URL
val sessionId = "" // the transaction ID
val apiKey = "" // merchant public API Key
val amount = 10.0 // decimal number

// List mode just to look better
val paymentOptionsDisplaySettings = Checkout.PaymentOptionsDisplaySettings(
            Checkout.PaymentOptionsDisplaySettings.PaymentOptionsDisplayMode.List(5))

// Builder class is used to construct an object passed to the SDK initializing function
val builder = Checkout
  .Builder(merchantId!!, sessionId, apiKey!!, amount!!)
    .paymentOptionsDisplaySettings(paymentOptionsDisplaySettings)
    .logger(Checkout.Logger.INFO)
    .build()

if (Checkout.isInitialized) {
  Checkout.release()
}

lifecycleScope.launch {
  runCatching {
    Checkout.init(
      context = this@MainActivity,
        builder = builder,
        successCallback = {
          Log.e("TAG", "successCallback: $it")
          //showResultDialog(it)
        },
        cancelCallback = {
          Log.e("TAG", "cancelCallback: $it")
          //showResultDialog(it)
        },
        errorCallback = { errorData, throwable ->
          Log.e("TAG", "errorCallback: $errorData")
          //showResultDialog(errorData, throwable)
        },
    )
  }.onSuccess {
    checkoutFragment = it
    supportFragmentManager
      .beginTransaction()
      .replace(R.id.ottuPaymentView, it)
      .commit()
  }.onFailure {
    //showErrorDialog(it)
  }
}
```
{% endcode %}
{% endstep %}

{% step %}
(Optional) Implement Result and Error Dialogs

Uncomment `showResultDialog` and `showErrorDialog` in the code above and add their implementations as methods inside the same `MainActivity`:

{% code overflow="wrap" %}
```swift
private fun showResultDialog(result: JSONObject?, throwable: Throwable? = null) {
    val sb = StringBuilder()

    result?.let {
        sb.apply {
            append(("Status : " + result.opt("status")) + "\n")
            append(("Message : " + result.opt("message")) + "\n")
            append(("Session id : " + result.opt("session_id")) + "\n")
            append(("operation : " + result.opt("operation")) + "\n")
            append(("Reference number : " + result.opt("reference_number")) + "\n")
            append(("Challenge Occurred : " + result.opt("challenge_occurred")) + "\n")
            append(("Form of payment: " + result.opt("form_of_payment")) + "\n")
        }
    } ?: run {
        sb.append(throwable?.message ?: "Unknown Error")
    }

    AlertDialog.Builder(this)
        .setTitle("Order Information")
        .setMessage(sb)
        .setPositiveButton(
            android.R.string.ok
        ) { dialog, which ->
            dialog.dismiss()
        }
        .show()
}

private fun showErrorDialog(throwable: Throwable? = null) {
    if (throwable is SecurityException) return

    AlertDialog.Builder(this)
        .setTitle("Failed")
        .setMessage(throwable?.message ?: "Unknown Error")
        .setPositiveButton(
            android.R.string.ok
        ) { dialog, which ->
            finish()
            dialog.dismiss()
        }
    .show()
}
```
{% endcode %}
{% endstep %}

{% step %}
Clean Up Memory

To ensure proper resource management, override the `onDestroy` method:

```swift
override fun onDestroy() {
  super.onDestroy()
  if (isFinishing) {
    Checkout.release()
  }
}
```
{% endstep %}

{% step %}
Verify Imports

Finally, confirm that all required imports are present.\
Android Studio usually detects and adds them automatically.
{% endstep %}
{% endstepper %}

#### **Notes on Initialization Parameters**

Not all parameters in `Checkout.init` are mandatory.\
The **required** parameters are:

* `sessionId` – ID of the created transaction
* `merchantId` – Same as the domain used in HTTP requests
* `apiKey` – Public API key used for authorization

**Recommended optional parameters:**

* `formsOfPayment`
* `setupPreload` (to reduce initialization time)

`setupPreload` is an object taken from the transaction creation response. When passed to the SDK, it prevents it from requesting the transaction details on its own and therefore speed-ups the initialization process by several seconds. This `setupPreload` is a decoded JSON object to a `TransactionDetails` \
For reference, check  the example: [here](https://github.com/ottuco/ottu-android/blob/main/Example/app/src/main/java/com/ottu/CheckoutSampleActivity.kt)&#x20;

## [Callbacks](quick-start.md#callbacks)

The SDK triggers three main callbacks:

* **`successCallback`** – Invoked upon successful payment.
* **`cancelCallback`** – Triggered when the user cancels the transaction.
* **`errorCallback`** – Activated on errors during the checkout process.

Developers should customize the logic within these callbacks to handle transaction results appropriately.

## [Useful Resources](quick-start.md#useful-resources)

* [Forms of Payment](./#formsofpayment-array-optional)&#x20;
* [Customization Theme](./#customization-theme)&#x20;
* [Setup Preload](./#setuppreload-object-optional)&#x20;
