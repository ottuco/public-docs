# Payment Gateway Compliance & Information

## [STC Pay](payment-gateway-compliance-and-information.md#stc-pay) <a href="#stc-pay" id="stc-pay"></a>

Once the STC Pay integration between Ottu and STC Pay has been completed, the necessary checks are automatically handled by the Checkout SDK to ensure the seamless display of the STC Pay button.

Upon initialization of the Checkout SDK with the [session\_id](payment-gateway-compliance-and-information.md#sessionid-string-required) and payment gateway codes ([pg\_codes](../../checkout-api.md#pg_codes-array-required)), the following condition is automatically verified:

* The `session_id` and pg\_codes provided during SDK initialization must be linked to the STC Pay Payment Service. This verification ensures that the STC Pay option is made available for selection as a payment method.

Regardless of whether a mobile number has been entered by the customer during transaction creation, the STC Pay button is displayed by the Android SDK.

## [Onsite Checkout](payment-gateway-compliance-and-information.md#onsite-checkout)

This payment option facilitates direct payments within the mobile SDK. A user-friendly interface allows users to securely input their CHD. If permitted by the backend, the card can be stored as a tokenized payment for future transactions.

**Example:**

<figure><img src="../../../.gitbook/assets/image (96).png" alt="" width="371"><figcaption></figcaption></figure>

The SDK supports multiple instances of onsite checkout payments. Therefore, for each payment method with a PG code of `ottu_pg`, the card form (as shown above) will be displayed.

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Fees are not displayed for onsite checkout instances due to the support of multiple card types through omni PG (multi-card) configurations. The presence of multiple payment icons also indicates this multi-card functionality.
{% endhint %}
