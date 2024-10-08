# Integrated WhatsApp Channel

This feature streamlines the process of notifying the customer for pre- / post-payment state via WhatsApp without any manual intervention.

## [Prerequisites](integrated-whatsapp-channel.md#prerequisites)

*   **Customer Phone Number**: The customer's phone number should be provided during the transaction creation phase. To add the customer phone number, please contact [Ottu support team](mailto:support@ottu.com)\
    \


    <figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
* **WhatsApp Business Account and Template Approval:** The merchant must have a WhatsApp Business account. Additionally, all templates and their content must be approved by Meta/WhatsApp before they can be used.
* **WhatsApp Integration Authenticator**: It acts as the link between WhatsApp and Ottu. \
  To configure it, please contact [Ottu support team](mailto:support@ottu.com).
* **WhatsApp Template Configuration**: Merchants create templates on the WhatsApp server. These templates should be linked through the **WhatsApp Integration Authenticator**. Merchants must provide the Ottu support team with the **Template Name** and **Namespace** used during the creation of their templates on the WhatsApp server. The [Ottu support team](mailto:support@ottu.com) will then configure these templates within the Ottu system.
*   **Enabling WhatsApp Templates**: WhatsApp templates can be enabled during the payment transaction creation process by selecting the checkbox designated for enabling WhatsApp templates. Templates can be enabled either before or after the payment, depending on the requirement.\


    <figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## [Integrated WhatsApp Notification Availability ](integrated-whatsapp-channel.md#integrated-whatsapp-notification-availability)

Ottu sends WhatsApp notifications to customers at key stages of the payment process, ensuring timely updates on important transaction states.

### [**Pre/Post Payment Notifications** ](integrated-whatsapp-channel.md#pre-post-payment-notifications)

WhatsApp notifications are triggered by the following payment states:

* `created`
* `paid`
* `authorized`
* `canceled`
* `failed`
* `expired`

For more details on these payment states, please refer to the [Parent Payment States](../../payment-tracking/payment-transactions-states.md#parent-states) section.

{% hint style="info" %}
WhatsApp channel does not support notifications for operational states.
{% endhint %}

{% hint style="info" %}
&#x20;The Pre-Payment notification is not available for [E-Commerce](../../plugins/e-commerce.md) plugin.
{% endhint %}
