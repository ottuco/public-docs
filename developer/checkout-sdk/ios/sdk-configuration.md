# SDK Configuration

### [Language](sdk-configuration.md#language) <a href="#language" id="language"></a>

The SDK supports two languages: English and Arabic, with English set as the default.

The language applied in the device settings is automatically used by the SDK, requiring no manual adjustments within the application.

However, if the transaction is created in a different language and setup preload is enabled, texts retrieved from the backend (such as fee descriptions) will be displayed in the transaction language rather than the device's language.

Therefore, the currently selected device language or the app's selected language should be considered when specifying a language code in the transaction creation request of the [Checkout API](../../checkout-api.md).

### [Light and dark theme](sdk-configuration.md#light-and-dark-theme) <a href="#light-and-dark-theme" id="light-and-dark-theme"></a>

The SDK also supports UI adjustments based on the device's theme settings (light or dark mode).

The appropriate theme is applied automatically during SDK initialization, aligning with the device's settings. Similar to language settings, no manual adjustments are required within the application.
