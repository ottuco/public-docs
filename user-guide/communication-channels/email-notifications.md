# Email Notifications

Email is a widely used and reliable channel for sending payment-related notifications to customers. It provides an easy and secure way to communicate important payment details, such as payment confirmation, status updates, and any actions performed on the transaction (e.g., refunds, voids).&#x20;

## [**Prerequisites**](email-notifications.md#prerequisites)

To ensure Email notifications function smoothly, certain prerequisites must be met:

*   **Customer Email**: The customer’s email field must be mandatory and provided during the creation of the payment transaction. To add the customer email and set it as a mandatory field, please contact [Ottu support team](mailto:support@ottu.com). <br>

    <figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>
* **Template Configured**: Ensure the email template is fully configured and reviewed for accuracy and completeness before enabling email notifications. To configure Email template, please contact [Ottu support team](mailto:support@ottu.com).
*   **Enabling Email Templates:** Merchants can enable these templates by selecting the appropriate checkboxes during the payment transaction creation process, either before or after the payment, based on the specific requirement.<br>

    <figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

## [Email Notification Availability](email-notifications.md#email-notification-availability)

Ottu sends email notifications to customers based on specific states in the payment lifecycle.&#x20;

### [Pre/Post Payment States](email-notifications.md#pre-post-payment-states)

Email notifications are triggered for the following payment states:

* `created`
* `paid`
* `authorized`
* `canceled`
* `failed`
* `expired`

For more information about these payment states, please refer to the [Parent Payment States ](../payment-tracking/payment-transactions-states.md#parent-states)section.

### [**Operation States**](email-notifications.md#operation-states)

Email notifications are triggered for the following operation states:

* `captured`
* `refunded`
* `voided`

For more details on these operation states, please refer to the [Operation Clauses](../payment-gateway.md#operations-clauses) section.

{% hint style="info" %}
For the [E-Commerce](../plugins/e-commerce.md) plugin, the Pre payment email notification is not available.
{% endhint %}
