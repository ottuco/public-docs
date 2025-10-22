# Quick Start

This guide explains how to integrate the **Ottu Checkout SDK** into a Flutter application from scratch.\
It walks you through[ project setup](quick-start.md#project-setup), SDK installation, and configuration steps for both [Android](quick-start.md#android-integration) and [iOS](quick-start.md#ios-integration), ensuring a smooth and efficient checkout experience within your Flutter app.

## [Installation](quick-start.md#pre-requisites) <a href="#pre-requisites" id="pre-requisites"></a>

{% stepper %}
{% step %}
**Create a New Flutter Project**

Here are tutorials on how to create a new Flutter project from scratch for different IDEs:\
&#x20;[Create a new Flutter app](https://docs.flutter.dev/reference/create-new-app)

You can use any IDE of your choice; however, it is **recommended** to use **Android Studio** and select **“New Flutter Project”**, ensuring the selected programming language is **Kotlin**.

Once the project is created, you can proceed to add the **Ottu Checkout SDK**.
{% endstep %}

{% step %}
**Add the Ottu Checkout SDK Dependency**

Open the `pubspec.yaml` file, navigate to the **dependencies** section, and add the following block:

```yaml
ottu_flutter_checkout:
  git:
    url: https://github.com/ottuco/ottu-flutter.git
    ref: 2.1.10
```

Then, run the following command to download the dependencies:

```bash
flutter pub get
```
{% endstep %}
{% endstepper %}

## [SDK Configuration](quick-start.md#sdk-configuration)

Integrate the Ottu Checkout SDK into your Flutter app for **Android** and **iOS** to enable seamless in-app payment processing.

### [Android SDK Configuration](quick-start.md#android-sdk-configura)

To integrate the SDK with **Android**, follow these steps.

{% stepper %}
{% step %}
**Add JitPack Repository**

The **Ottu SDK** is a Flutter plugin that includes a native platform view.

From the root directory of your project, open the `android` folder and locate the Gradle file:\
`app/build.gradle` (for Groovy) or `app/build.gradle.kts` (for Kotlin).

At the bottom of this file, add the following lines:

```kotlin
allprojects {
  repositories {
    ...
    maven { url = uri("https://www.jitpack.io") }
  }
}
```
{% endstep %}

{% step %}
**Update Android Build Properties**

The Ottu SDK requires specific Android build configurations.\
Ensure the following values are set in your Gradle script:

* `compileSdk` version: **36 or newer**
* `minSdk` version: **26**

Update these values if they differ from your current setup.
{% endstep %}

{% step %}
**Verify** `MainActivity` **Inheritance**

Go to the Android part of your project and open `MainActivity` (or your custom Android activity).\
Ensure that it extends **`FlutterFragmentActivity`**:

```kotlin
import io.flutter.embedding.android.FlutterFragmentActivity
class MainActivity : FlutterFragmentActivity()
```
{% endstep %}

{% step %}
**Apply Material Theme**

Check if your app theme uses a **Material theme**.\
If not, extend it with `Theme.Material3.DayNight`.\
If you define a night theme, make sure to update it accordingly.

```xml
<style name="LaunchTheme" parent="Theme.Material3.DayNight">
    <item name="android:windowBackground">@drawable/launch_background</item>
</style>
```
{% endstep %}

{% step %}
**Run the App**

Run the application using the command line with the following command:

```bash
flutter run
```

> Running the app via command line is mandatory.\
> If all dependencies are configured correctly, the app should launch without errors.
{% endstep %}
{% endstepper %}

### [iOS SDK Configuration](quick-start.md#ios-sdk-con)

To integrate the SDK into an **iOS** app, follow these steps:

{% stepper %}
{% step %}
**Open the iOS Project**

1. Open **Xcode**.
2. Select **“Open Project”** and choose the `.xcworkspace` file located inside the `ios` directory within your Flutter app’s root directory.
{% endstep %}

{% step %}
**Configure iOS Deployment Target**

1. Select the **Runner** target.
2. Go to the **General** tab.
3. Find the **Minimum Deployment** section and set its value to **15**.
{% endstep %}

{% step %}
**Run the App**

Run the application for the first time from the terminal using the same command:

```bash
flutter run
```

> This command ensures that all necessary iOS plugin registration files are created automatically.
{% endstep %}
{% endstepper %}

## [**Add OttuCheckoutWidget**](quick-start.md#add-ottucheckoutwidget)

Open the screen or widget where you plan to add the **Ottu SDK widget**.

{% stepper %}
{% step %}
**Define** `ValueNotifier`

In the **State class** of your screen widget, define a new member of type `ValueNotifier<int>`:

```dart
final _checkoutHeight = ValueNotifier(300);
```

Here, the default value of **300** represents the most suitable height for the `OttuCheckoutWidget`.
{% endstep %}

{% step %}
**Add and Wrap** `OttuCheckoutWidget`

Add the `OttuCheckoutWidget` to the desired location in your layout.

Wrap the `OttuCheckoutWidget` inside a **`SizedBox`**,\
then wrap this `SizedBox` within a **`ValueListenableBuilder<int>`** to dynamically adjust the widget height based on the `ValueNotifier` value.

Set the `valueListenable` parameter of the builder to the value you created earlier in previous step

```dart
ValueListenableBuilder(
  valueListenable: _checkoutHeight,
  builder: (context, height, child) {
    return SizedBox(
      height: height.toDouble(),
      child: OttuCheckoutWidget(
          arguments: CheckoutArguments(
          merchantId: "alpha.ottu.net",
          apiKey: 'your Api key',
          sessionId: 'your session id',
          amount: 20.0,
          showPaymentDetails: true,
          paymentOptionsListMode: PaymentOptionsListMode.LIST,
          paymentOptionsListCount: 5,
          defaultSelectedPgCode: '',
        ),
      ),
    );
  },
),
```
{% endstep %}

{% step %}
**Define** `MethodChannel` **and Constant**

Next, define your `MethodChannel` as a top-level member of the file.\
It listens to messages from the native Android or iOS view:

```dart
const _methodChannel = MethodChannel("com.ottu.sample/checkout");
```

Also, define a constant string member to identify the channel’s method name:

```dart
const _methodCheckoutHeight = "METHOD_CHECKOUT_HEIGHT";
```
{% endstep %}

{% step %}
**Register** `MethodChannel` **Handler**

Register a handler for this `MethodChannel`.\
It is recommended to do this inside the `didChangeDependencies()` widget state callback method:

```dart
  @override
  void didChangeDependencies() {
    _methodChannel.setMethodCallHandler((call) async {
      switch (call.method) {
        case _methodCheckoutHeight:
          int height = call.arguments as int;
          _checkoutHeight.value = height;
      }
    });
    super.didChangeDependencies();
  }
```
{% endstep %}

{% step %}
**Run the App**

This is the final step to complete the integration.\
Run your Flutter app using the following command or by pressing the **Run/Launch** icon in your IDE:

```bash
flutter run
```

If all configurations and dependencies are correctly set, the app should build successfully and display the **Ottu Checkout Widget**.
{% endstep %}
{% endstepper %}
