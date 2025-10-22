# Native UI

The SDK UI is embedded as a Fragment within any part of an Activity in the merchant's application.

**Example:**

<figure><img src="../../../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

If a wallet is the only available payment option, the UI is minimized automatically.

<figure><img src="../../../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
To avoid style issues and potential crashes, the parent application's theme must be `Theme.AppCompat` or one of its descendant classes. This theme is specified in the `themes.xml` file located in the `values` directory of the project.
{% endhint %}
