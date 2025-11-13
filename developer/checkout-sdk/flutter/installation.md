# Installation

To install the **Flutter SDK plugin**, the following section must be added to the **`pubspec.yaml`** file:

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

{% hint style="info" %}
If the development is being done on Windows or Linux, and iOS support is not required, you need to adjust the following line, [Access it here](https://github.com/ottuco/ottu-flutter/blob/main/Sample/pubspec.yaml#L40), in the `pubspec.yaml` file, by replacing the `ref` value `2.1.12` with `release-no-ios`.

Then, run the following two commands:\
`flutter clean`\
`flutter pub get`
{% endhint %}

## [Android](installation.md#android)

#### **Minimum Requirements**

The SDK can be used on devices running **Android 8 (Android SDK 26)** or higher.

{% hint style="warning" %}
To prevent application crashes, it must be ensured that the correct parent theme is applied to the Android application. The theme configuration is defined in the `themes.xml` and/or `styles.xml` files located within the `res/values` directory. The `parent` attribute of the `style` tag should be set to `Theme.Material3.DayNight.NoActionBar`.

For reference, the configuration file can be reviewed at the following link: [themes.xml on GitHub](https://github.com/ottuco/ottu-flutter/blob/main/Sample/android/app/src/main/res/values/themes.xml#L3C58-L3C94)
{% endhint %}

## [iOS](installation.md#ios)

#### **Minimum Requirements**

The SDK can be used on devices running **iOS 15** or higher.
