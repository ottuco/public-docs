# SDK Configuration

## [Language](sdk-configuration.md#language) <a href="#language" id="language"></a>

The SDK supports two languages, English and Arabic, with English set as the default.

It automatically adopts the language configured in the device settings, requiring no in-app adjustments. However, if a transaction is initiated in a different language and setup preload is utilized, the backend-generated text (such as fee descriptions) will appear in the language of the transaction. Therefore, it is important to ensure that the language code passed to the [Checkout API](../../checkout-api.md)'s transaction creation request matches the currently selected language on the device or current selected app language.

## [Light and dark theme](sdk-configuration.md#light-and-dark-theme) <a href="#light-and-dark-theme" id="light-and-dark-theme"></a>

The SDK supports UI customization to match the device theme—light or dark. This adjustment is applied during the SDK initialization, based on the device's settings. Similarly, for language, no adjustments are made within the app.
