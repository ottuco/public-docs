# UI

## [**Android**](ui.md#android-1)

The SDK UI is embedded as a `fragment` within any part of an `activity` in the merchant's application.

**Example:**

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt="" width="375"><figcaption></figcaption></figure>

If only one payment option is available and it is a wallet, the UI is automatically minimized.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
The parent application must use a theme based on `Theme.AppCompat` (or a subclass) to prevent crashes and styling problems. This requirement is defined in the `themes.xml` file within the `values` directory of the project.
{% endhint %}
