# Payment Gateway Compliance & Information

## [Apple Pay](payment-gateway-compliance-and-information.md#apple-pay) <a href="#apple-pay" id="apple-pay"></a>

When the [integration ](../web.md#apple-pay)between Ottu and Apple for Apple Pay is completed, the necessary checks to display the Apple Pay button are handled automatically by the Checkout SDK.

1. **Initialization**: Upon initialization of the Checkout SDK with the provided [session\_id](payment-gateway-compliance-and-information.md#sessionid-string-required) and payment gateway codes ([pg\_codes](../../checkout-api.md#pg_codes-array-required)), several conditions are automatically verified:
   * It is confirmed that a `session_id` and `pg_codes` associated with the Apple Pay Payment Service have been supplied.
   * It is ensured that the customer is using an Apple device that supports Apple Pay. If the device is not supported, the button will not be shown, and an error message stating `This device doesn't support Apple Pay` will be displayed to inform the user of the compatibility issue.
   * It is verified that the customer has a wallet configured on their Apple Pay device. if the wallet is not configured (i.e., no payment cards are added), the Setup button will  appear. Clicking on this button will prompt the Apple Pay wallet on the user's device to open, allowing them to configure it by adding payment cards.
2. **Displaying the Apple Pay Button**: If all these conditions are met, the Apple Pay button is displayed and made available for use in the checkout flow.
3. **Restricting Payment Options**: To display only the Apple Pay button, `applePay` should be passed within the `formsOfPayment` parameter. The `formsOfPayment` property instructs the Checkout SDK to render only the Apple Pay button. If this property is not included, all available payment options are rendered by the SDK.

This setup ensures a seamless integration and user experience, allowing customers to easily set up and use Apple Pay during the checkout process.

## [STC Pay](payment-gateway-compliance-and-information.md#stc-pay) <a href="#stc-pay" id="stc-pay"></a>

When the [integration](../web.md#stc-pay) between Ottu and STC Pay is completed, the necessary checks to display the STC Pay button are handled seamlessly by the Checkout SDK.

**Initialization**: Upon initialization of the Checkout SDK with the provided [session\_id](payment-gateway-compliance-and-information.md#sessionid-string-required) and payment gateway codes ([pg\_codes](../../checkout-api.md#pg_codes-array-required)), several conditions are automatically verified:

* It is confirmed that the `session_id` and `pg_codes` provided during SDK initialization are associated with the STC Pay Payment Service. This ensures that the STC Pay option is available for the customer to choose as a payment method.
* It is ensured that the STC Pay button is displayed by the iOS SDK, regardless of whether the customer has provided a mobile number while creating the transaction.

This setup ensures a seamless integration and user experience, allowing customers to easily set up and use STC Pay during the checkout process.

## [KNET - Apple Pay](payment-gateway-compliance-and-information.md#knet-apple-pay) <a href="#knet-apple-pay" id="knet-apple-pay"></a>

Due to compliance requirements, KNET necessitates a popup displaying the payment result after each failed payment. This functionality is available only in the `cancelCallback` when there is a response from the payment gateway. As a result, the user must click on the Apple Pay button again to retry the payment.

{% hint style="info" %}
The popup notification requirement is specific to the KNET payment gateway. Other payment gateways may have different requirements or notification mechanisms, so it is essential to follow the respective documentation for each payment gateway integration.
{% endhint %}

To properly handle the popup notification for KNET, the following code snippet should be implemented into your payment processing flow:

```swift
func cancelCallback(_ data: [String: Any] ? ) {
    var message = ""

    if let paymentGatewayInfo = data ? ["payment_gateway_info"] as ? [String: Any],
        let pgName = paymentGatewayInfo["pg_name"] as ? String,
            pgName == "kpay" {
                message = paymentGatewayInfo["pg_response"].debugDescription
            } else {
                message = data?.debugDescription ?? ""
            }

    navigationController?.popViewController(animated: true)
    let alert = UIAlertController(title: "Canсel", message: message, preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
}
}
```

The above code performs the following checks and actions:

1. **Verification**: It first checks if the cancel object contains information about the payment gateway `payment_gateway_info`.
2. **Payment Gateway Identification**: It then verifies if the `pg_name` property in `payment_gateway_info` is equal to "kpay", confirming that the payment gateway used is KNET.
3. **Response Handling**: If the conditions are met, it retrieves the payment gateway's response from the `pg_response` property. If not available, it uses a default "Payment was cancelled." error message.
4. **Popup Notification**: Finally, it displays the error message in a popup using `self.present(alert, animated: true)` to notify the user about the failed payment.

This setup ensures compliance with KNET's requirements and provides a clear user experience for handling failed payments.

## [Onsite Checkout](payment-gateway-compliance-and-information.md#onsite-checkout)

This payment option enables direct payments through the mobile SDK. The SDK provides a user interface where the Cardholder Data (CHD) is entered by the user. If permitted by the backend, the card can be tokenized and saved for future payments.

Below is an example of how the onsite checkout screen appears:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

The SDK supports multiple instances of onsite checkout payments. Therefore, for each payment method with a PG code of `ottu_pg`, the card form (as described above) will be displayed.

<figure><img src="../../../.gitbook/assets/image (102).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
Fees are not shown for onsite checkout instances because the system supports payments through multiple cards (omni PG). The multiple payment icons indicate the availability of different card options.
{% endhint %}
