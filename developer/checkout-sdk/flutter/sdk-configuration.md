# SDK Configuration

## [**Language**](sdk-configuration.md#language)

The SDK supports two languages: **English** and **Arabic**, with **English** set as the default.

The SDK automatically applies the language based on the device settings, eliminating the need for manual adjustments within the application.

However, if the transaction is created in a different language and setup preload is enabled, texts retrieved from the backend (such as fee descriptions) will be displayed in the transaction language, regardless of the device’s language settings.

To ensure consistency, the current device language should be taken into account when specifying a language code in the transaction creation request of the [Checkout API](../../checkout-api.md).

## [**Light and Dark Theme**](sdk-configuration.md#light-and-dark-theme)

The SDK supports automatic UI adjustments based on the device's theme settings (light or dark mode).

The appropriate theme is applied during [SDK initialization](sdk-configuration.md#sdk-initialization), aligning with the device's configuration. Similar to language settings, no manual adjustments are required within the application.
