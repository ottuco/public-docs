# Payment Gateway Compliance & Information

## [STC Pay](payment-gateway-compliance-and-information.md#stc-pay)

If the STC Pay integration between Ottu and STC Pay has been completed, the Checkout SDK will automatically handle the necessary checks to display the STC Pay button seamlessly.

When the Checkout SDK is initialized with the [session\_id](../../checkout-api.md#session_id-string-mandatory) and payment gateway codes ([pg\_codes](../../checkout-api.md#pg_codes-array-required)), the SDK will verify the following conditions:

* The `session_id` and `pg_codes` provided during initialization must be associated with the STC Pay Payment Service. This ensures that the STC Pay option is available for the customer.
* In the Android SDK, the STC Pay button is displayed regardless of whether the customer has entered a mobile number while creating the transaction.

## [**KNET - Apple Pay** ](payment-gateway-compliance-and-information.md#knet-apple-pay)

Due to compliance requirements, for iOS, the KNET payment gateway requires a popup notification displaying the payment result after each failed payment. This notification is triggered only in the cancelCallback, but only if a response is received from the payment gateway.

As a result, the user cannot retry the payment without manually clicking on Apple Pay again.

{% hint style="info" %}
The popup notification mentioned above is specific to the KNET payment gateway.Other payment gateways may have different requirements or notification mechanisms, so it is essential to follow the respective documentation for each integration.
{% endhint %}

To properly **handle** the **KNET popup notification**, the following **Swift** code snippet must be implemented into the **payment processing flow**:

{% hint style="info" %}
This is only iOS-related stuff, so the callbacks are native and so they are in Swift language.
{% endhint %}

{% code overflow="wrap" %}
```swift
func cancelCallback(_ data: [String: Any]?) {
    var message = ""
    
    if let paymentGatewayInfo = data?["payment_gateway_info"] as? [String: Any],
       let pgName = paymentGatewayInfo["pg_name"] as? String,
       pgName == "kpay" {
        message = paymentGatewayInfo["pg_response"].debugDescription
    } else {
        message = data?.debugDescription ?? ""
    }
    
    navigationController?.popViewController(animated: true)
    
    let alert = UIAlertController(
        title: "Cancel",
        message: message,
        preferredStyle: .alert
    )
    
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
}

```
{% endcode %}

#### **Function Breakdown**

The above code performs the following checks and actions:

1. Checks if the `cancelCallback` object contains payment gateway information
   * It verifies whether the `payment_gateway_info` field is available in the response.
2. Identifies if the payment gateway used is `KNET`
   * It checks if the `pg_name` property equals `kpay`, confirming that the transaction was processed using KNET.
3. Check the above two conditions are met by retrieving the payment gateway response
   * If the gateway response (`pg_response`) is available, it is displayed; otherwise, a default message (`Payment was cancelled.`) is used.
4. Navigates back and displays an alert
   * The user is returned to the previous screen (`navigationController?.popViewController(animated: true)`).
   * A popup notification is displayed using `self.present(alert, animated: true)`, informing the user about the failed payment.

## [**Onsite Checkout**](payment-gateway-compliance-and-information.md#onsite-checkout)

This payment option enables direct payments through the mobile SDK. The SDK presents a user interface where the customer can enter their cardholder details (CHD). If supported by the backend, the user can also save the card for future payments—stored as a tokenized payment method.

The onsite checkout screen appears identical to the native platform version.

#### Android

<figure><img src="../../../.gitbook/assets/image (2) (1) (1).png" alt="" width="247"><figcaption></figcaption></figure>

#### iOS&#x20;

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

The SDK supports multiple instances of onsite checkout payments. Therefore, for each payment method with a PG code equal to `ottu_pg`, the card form (as shown above) will be displayed.

**Android**

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

**iOS**

<figure><img src="../../../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
No fees are displayed for onsite checkout instances. This is due to the support for multiple multi-card (omni PG) configurations. The presence of multiple payment icons is used to indicate this multi-card feature.
{% endhint %}
