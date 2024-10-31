# SMS Notifications

SMS is a fast and convenient channel for sending payment-related notifications to customers. With high open rates and immediate delivery, SMS is ideal for sending concise, time-sensitive payment details such as payment confirmations, status updates, or critical actions performed on a transaction (e.g., refunds, voids).&#x20;

## [Prerequisites](sms-notifications.md#prerequisites)

To ensure SMS notifications work seamlessly, certain prerequisites must be met:

*   **Customer Phone Number:** The customer’s phone number must be required and provided when creating a payment transaction. To set the phone number as a mandatory field, please contact [Ottu support team](mailto:support@ottu.com).\


    <figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
* **SMS Template Configuration:** Ensure that the SMS related templates are fully configured and reviewed for accuracy and completeness before enabling SMS notifications. To configure SMS templates, please contact [Ottu support team](mailto:support@ottu.com).
*   **Enabling SMS Templates:** Merchant can enable these templates by selecting the corresponding checkboxes during the payment transaction creation process, either before or after the payment, depending on the specific requirement.\


    <figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
* **SMS Provider:** A dedicated SMS provider should be added and configured through [Ottu support team](mailto:support@ottu.com).&#x20;

## [SMS Notification Availability](sms-notifications.md#sms-notification-availability)

Ottu sends SMS notifications to customers based on key states in the payment lifecycle. These states ensure that customers are informed about important transaction states in a timely manner.

### [**Pre/Post Payment States**](sms-notifications.md#pre-post-payment-states)

The following states trigger SMS notifications for payment events:

* `created`
* `paid`
* `authorized`
* `canceled`
* `failed`
* `expired`

For more information about payment states, refer to the [Parent Payment States](../payment-tracking/payment-transactions-states.md#parent-states) section.

### [**Operation States**](sms-notifications.md#operation-states)

SMS notifications are triggered for key operation States, including:

* `captured`
* `refunded`
* `voided`

For further details on operation  states, see the [Operation Clauses](../payment-gateway.md#operations-clauses) section.

{% hint style="info" %}
Pre payment SMS notification is not available for the [E-Commerce ](../plugins/e-commerce.md)plugin
{% endhint %}
