# Webhooks Configuration

[Webhooks](../../developer/webhooks/) is an HTTP endpoint that is used to receive notifications about events that occur in the Ottu system. For example, if a payment is created, Ottu can send a webhook notification to the merchant's server with the details of the payment. The merchant can then use this information to update their systems. Enhance your Ottu experience with our powerful Webhook Configuration. Take advantage of API payloads, SSL certificate verification options, error notifications, and more.

## [**Webhooks Configuration Walkthrough**](webhooks-configuration.md#webhooks-configuration-walkthrough)

To access the Webhook Configuration, navigate to Ottu Dashboard > Administration Panel > Webhook > Webhook Config

<figure><img src="../../.gitbook/assets/webhook.png" alt=""><figcaption></figcaption></figure>

### [General](webhooks-configuration.md#general)

<figure><img src="../../.gitbook/assets/webhook config.png" alt=""><figcaption></figcaption></figure>

#### **Description of Fields:**

* **HMAC key:** This key is used to [generate signatures](../../developer/webhooks/signing-mechanism.md#signature-generation).
* **Ignore SSL:** If checked, the SSL certificate will not be verified when calling the [webhook URL](../../developer/checkout-api.md#webhook\_url-string-optional).
* **Notify on Error:** If checked, an email will be sent if an error occurs while calling the [webhook URL](../../developer/checkout-api.md#webhook\_url-string-optional).
* **Email List:** Specify the list of email addresses where the [webhook URL](../../developer/checkout-api.md#webhook\_url-string-optional) error notification should be sent.
* **Timeout:** The amount of time that the Ottu server will wait for a response from the merchant server.
* **Retries:** The number of retry attempts the Ottu server will make to resend the request to the merchant server if the first attempt fails. Note that the **Enable retry webhook mechanism** option should be checked to activate this feature.
* **Backoff factor:** The amount of time the Ottu server will wait before retrying the request (i.e., the time between two attempts).

#### [Example:](webhooks-configuration.md#example)&#x20;

Imagine a scenario where the merchant’s server experiences **downtime for 30 seconds**. If the **timeout is set to 20 seconds**, the **retries are set to 3**, and the **backoff factor is set to 5 seconds**, then the following will happen:\
Keep in mind that the merchant’s server will take 30 seconds to respond, and the number of attempts is 3.

* **First Try:**

1. Ottu's server will send a request to the merchant's server.
2. Ottu's server will wait 20 seconds for a response (timeout = 20), and this attempt will fail.
3. Then Ottu's server will wait 5 seconds for the backoff factor (backoff factor = 5).

**Note that the first attempt took 25 seconds.**

* **Second Try:**

1. Ottu's server will retry the request, i.e., send another request.
2. After 5 seconds, the merchant's server will respond since the server downtime will be over (30 seconds), and the request will be successful.

* **Version:** The version of the webhook API.
* **Enable webhook notifications:** If checked, webhook notifications will be activated.

{% hint style="info" %}
**Redirect behavior:** The redirect behavior is determined by the [webhook URL](../../developer/checkout-api.md#webhook\_url-string-optional) response to payment events and payment operations.

* If the webhook URL returns a status code of 200, the customer will be redirected to the [redirect\_url](../../developer/checkout-api.md#redirect\_url-string-optional).
* If the webhook URL returns a status code of 201, the customer will be redirected to the **Ottu payment summary page**.
* If the webhook URL returns any other status code, the customer will be redirected to the **Ottu payment summary page**. \
  In this case, Ottu can send an email notification if the **Enable webhook notifications** option is checked.
{% endhint %}

* **Enable retry webhook mechanism**: If checked, Ottu will retry the request if the first attempt fails. See the [example scenario](webhooks-configuration.md#example) above for further clarity.
* **Operations webhook\_url:** The URL where transaction data will be disclosed once an operation transaction flow is triggered. See [Operation Notification](../../developer/webhooks/operation-notification.md).
* **Enable webhook notifications if transaction initiated from API:** If checked, [Payment Webhooks](../../developer/webhooks/payment-webhooks.md) will be activated even if the transaction is created via the API.

### [Webhook Plugin Configs](webhooks-configuration.md#webhook-plugin-configs)

In this tab, the merchant can define the desired webhook behavior for specific plugins.

<figure><img src="../../.gitbook/assets/Webhook plugin configs.png" alt=""><figcaption></figcaption></figure>

**Description of Fields:**

* **Webhook plugin:** The plugin that the webhook works for. See [Plugins](../plugins/)
* **Webhook UrL:** When a [payment event](../../developer/webhooks/payment-webhooks.md) or [payment operation](../../developer/webhooks/operation-notification.md) occurs, Ottu sends an HTTP request to this URL to disclose transactional data.
* **Enable transaction state webhook notifications:** If checked, webhook notifications will be sent for the defined Notification status.
* **Notification status:** Define the transaction status that will trigger the [Payment Webhook,](../../developer/webhooks/payment-webhooks.md) including `paid`, `failed`, `authorized`, and `canceled`. Review the [payment transaction states](../payment-tracking/#states-of-parent-payment-transaction) for more information.
* **Delete:** Deletes the defined plugin webhook configuration.

{% hint style="info" %}
The **webhook\_Url** specified in the [webhook plugin configuration](webhooks-configuration.md#webhook-plugin-configs) serves as the endpoint for receiving notifications related to both [payments](../../developer/webhooks/payment-webhooks.md) and [operations](../../developer/webhooks/operation-notification.md). If we provide values for both the operation webhook\_url and the webhook\_Url in the plugin configuration, the system will transmit data to both URLs.
{% endhint %}

For more information about how and where webhook works in Ottu see [Webhooks](../../developer/webhooks/).
