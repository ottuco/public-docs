# Payment Status-Inquiry

The `Payment Status Inquiry API` endpoint is a part of Ottu's Check Status API designed to check the status of a specific payment transaction. This is especially useful when your system may not have received notifications about changes to a transaction's status.&#x20;

`The Payment Status Inquiry API` effectively acts as a manual status confirmation mechanism, reflecting the structure of a [payment webhook notification](webhooks/payment-notification.md). The endpoint can be triggered for payment transactions in the following states: `pending`, `attempted`, `failed`, or `expired`. If the transaction state is already `paid` or `authorized`, the status is returned immediately without needing to re-confirm with third-party [Payment Gateways](../user-guide/payment-gateway.md) (PGs).&#x20;

However, if the transaction state is not up-to-date and is still listed in one of the aforementioned states, Ottu will trigger an API call to the PG to update the transaction state. In cases where multiple payment options were `attempted` using different PGs, all PGs that support payment status checks will be called, ensuring that you receive the most updated status for the payment.

## [Automatic Inquiry](payment-status-inquiry.md#automatic-inquiry)

The payment process can be intricate, and numerous unforeseen events might disrupt the smooth flow of a transaction. Ottu’s Automatic Inquiry feature is designed to mitigate such issues and ensure that merchants receive accurate payment statuses. This feature is enabled by default for every payment gateway and one that’s non-negotiable.

#### [How It Works](payment-status-inquiry.md#how-it-works)

1. **Scheduled Inquiry Job:** For every Payment Gateway (PG) that supports the inquiry feature, Ottu schedules an automatic inquiry job. This job is set to trigger after a predetermined period, which is defined based on the session expiration time of each specific PG. The exact timing for each PG can be found [here](../user-guide/payment-gateway.md#available-operations).
2. **Purpose:** The objective behind this automatic inquiry job is to account for scenarios where a payer might abruptly close their browser tab, face internet connectivity issues, or in instances where the PG fails to notify Ottu (or Ottu misses the notification due to temporary downtime). The job ensures that Ottu reconciles with the PG and updates the payment status accordingly.
3. **Execution:** This job calls the PG’s inquiry API three times to guarantee accurate reconciliation. However, if a successful response (indicating a payment success) is received during any of these calls, the subsequent calls are halted.
4. **Exceptions:** Certain aggregator integrations, such as MyFatoorah, may encounter scenarios where the payment status remains `pending` even after redirection to Ottu. This issue typically arises from the absence of a definitive response from the actual payment gateway (PG). \
   \
   To address this, Ottu does not trigger a [webhook notification](webhooks/payment-notification.md) to the merchant for the `pending` status. Instead, Ottu proactively schedules an additional inquiry job to clarify the payment status.\
   Ottu will continue monitoring the payment status without sending webhook notifications to the merchant. Once the inquiry job reaches a definitive end state—either `paid` or `failed`, Ottu will then send a webhook notification to the merchant. This approach ensures that merchants receive accurate and final payment statuses, enhancing the reliability of the transaction process.

## [Setup](payment-status-inquiry.md#setup)

#### [Prerequisites](payment-status-inquiry.md#prerequisites)

For this API to work efficiently, here are a few things you need to be familiar with:

1. **Payment Gateway:** At least one Payment Gateway that supports status checks should be available. You can find more about Payment Gateways [here](../user-guide/payment-gateway.md).
2. **Webhook:** The Payment Webhook response, as this is the response format which `Payment Status Inquiry API` returns. More details can be found [here](webhooks/payment-notification.md).

#### [Recommendations for Using Ottu’s Inquiry API](payment-status-inquiry.md#recommendations-for-using-ottus-inquiry-api)

If you’ve set up [Webhook Payment Notifications](webhooks/payment-notification.md) with Ottu, it’s best to rely on the responses from these notifications after Ottu’s automatic inquiry. Only consider integrating the inquiry API if you haven’t enabled webhook notifications.

* **Timing is Crucial:** Scheduling your inquiry calls with precision is essential. Otherwise, you might miss the most recent transaction status.
* **Example with MPGS:** The PG [MPGS](../user-guide/payment-gateway.md#mpgs) typically requires an inquiry after approximately 11 minutes. If you initiate your inquiry call prematurely, say around the 8-minute mark, you might miss out on the most recent status. It’s advisable to add a margin of **2-3 minutes**, making your inquiry after about 13-14 minutes for MPGS.
* **Differing PG Times:** With PGs like [KNET](../user-guide/payment-gateway.md#knet), the inquiry is scheduled for 8 minutes post-payment initiation. When you integrate with multiple PGs through Ottu, it’s beneficial to identify the PG with the longest inquiry time, add a 2-3 minute margin to it, and use this extended timeframe as a standard for all inquiries.

In essence, the Automatic Inquiry feature is Ottu’s commitment to providing consistent and updated payment statuses, ensuring you never miss a payment update. Always remember to time your inquiries wisely and stay in sync with Ottu’s schedule for the best results.

### [Boost Your Integration](payment-status-inquiry.md#boost-your-integration)

Enhance your integration with the `Payment Status Inquiry API` using Ottu official SDK packages, specifically designed for payment status management. This SDK streamlines the process of querying and managing payment statuses, ensuring efficient and simplified interactions.

**Available Package:**

* **Python SDK:** Provides an object-oriented approach to querying payment statuses, allowing you to check the status of transactions with ease. This SDK abstracts the complexities of API calls, focusing on efficiency and user-friendliness. [Learn more](https://github.com/ottuco/ottu-py?tab=readme-ov-file#ottu-py)
* **Django SDK:** Facilitates the integration of payment methods into Django projects by providing Django-specific tools and utilities that streamline payment processing. [Find out more](https://github.com/ottuco/ottu-py?tab=readme-ov-file#django-integration)

Although these packages provide significant advantages in speed and convenience, it is crucial to understand the foundational concepts and structures detailed in the documentation. This knowledge ensures that you can integrate and utilize the packages effectively, maintaining robustness and sustainability in their application.

## [Authentication](payment-status-inquiry.md#authentication)

This API endpoint uses both [API-Key](authentication.md#private-key-api-key) and [Basic Authentication](authentication.md#basic-authentication). No special permissions are required for Basic Authentication.

## [How it Works](payment-status-inquiry.md#how-it-works-1)

1.  Payment Status Inquiry for `pending`, `attempted`, `failed`, or `expired` states\
    See [Payment Transaction State](../user-guide/payment-tracking/payment-transactions-states.md).<br>

    <figure><img src="../.gitbook/assets/ideas - Inquiry API (&#x60;pending&#x60;, &#x60;attempted&#x60;, &#x60;failed&#x60;, or &#x60;expired&#x60;) (5).png" alt="" width="375"><figcaption></figcaption></figure>
2. Payment Status Inquiry for `paid` or `authorized` states

<figure><img src="../.gitbook/assets/ideas - Inquiry API for &#x60;paid&#x60; or &#x60;authorized&#x60; (1).png" alt="" width="375"><figcaption></figcaption></figure>

### [Limitations](payment-status-inquiry.md#limitations)

The Payment Status Inquiry API implements a throttling mechanism to prevent potential system abuse. \
**Here are the rules:**

1. **Initial Grace Period (10 minutes):** If the Inquiry API is called within 10 minutes from when the payment transaction is created, the request will be throttled.
2. **First Request:** After the initial grace period, the first request is permitted. Any subsequent requests for the same transaction within the next 30 minutes will be throttled.
3. **Second Request** After the first 30-minute throttle period, a second request is allowed. Further requests for the same transaction within another 30 minutes will be throttled.
4. **Subsequent Requests:** If the number of requests for the same transaction exceeds three within a single day, any further requests will be denied.

{% hint style="info" %}
These rules are per transaction. Additionally, the endpoint allows a maximum of 30 requests per minute across all transactions
{% endhint %}

To request a status inquiry, you must provide at least one of the following identifiers: \
[session\_id](checkout-api.md#session_id-string-mandatory) or [order\_no](checkout-api.md#order_no-string-optional). For a more detailed technical understanding and the implementation specifics of these operations, please refer to the Open API schema in the [API Schema Reference](payment-status-inquiry.md#api-schema-reference).

## [API Schema References](payment-status-inquiry.md#api-schema-references)

{% openapi-operation spec="inquiry-sandbox" path="/b/pbl/v2/inquiry/" method="post" %}
[OpenAPI inquiry-sandbox](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/92e1cf2f9254df51c2ae6282f7deb4c1d44e8a76cf8e4f016c83e93b01ed6edc.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20251126%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20251126T074649Z&X-Amz-Expires=172800&X-Amz-Signature=54be6f554ea942b4f35092bc44c2fa985876095d8e20eb1fead5ee5dc22c4e28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [Guide](payment-status-inquiry.md#guide)

This guide is your go-to resource for effectively using the Status Inquiry API in your online payment management system. It's all about timing and integration, ensuring you never miss the latest transaction updates. Whether you're new to the system or looking to fine-tune your payment processes, this guide has you covered.

**Webhook Payment Notifications:** If you've configured Webhook Payment Notifications with Ottu, it is advisable to rely on the responses from these notifications following Ottu's automatic inquiry process. Only consider integrating the inquiry API if you have not enabled webhook notifications.

**Timing is Crucial:** Timing plays a crucial role in using the Status Inquiry API effectively. It's essential to schedule your inquiry calls with precision to ensure you receive the most recent transaction status. Failure to do so may result in missing important updates.

**Example with MPGS:** For example, the Payment Gateway (PG) MPGS typically requires an inquiry approximately 11 minutes after payment initiation. Initiating your inquiry call prematurely, around the 8-minute mark, may lead to missing the most recent status. To avoid this, we recommend adding a margin of 2-3 minutes, making your inquiry after about 13-14 minutes for MPGS.

**Differing PG Times:** Different Payment Gateways (PGs) may have varying inquiry times. For PGs like KNET, the inquiry is scheduled for 8 minutes post-payment initiation. When integrating with multiple PGs through Ottu, it's beneficial to identify the PG with the longest inquiry time, add a 2-3 minute margin to it, and use this extended timeframe as a standard for all inquiries.

In essence, the Automatic Inquiry feature is Ottu’s commitment to providing consistent and updated payment statuses, ensuring you never miss a payment update. Always remember to time your inquiries wisely and stay in sync with Ottu’s schedule for the best results.

## [Best Practices](payment-status-inquiry.md#best-practices)

* **Stay within the Request Limits:** Be mindful of our API’s built-in throttling mechanisms to ensure smooth operation.
* **Understand the Webhook Response:** Knowing how to interpret the Payment Webhook response is crucial for accurate results. Check [Payment Notification](webhooks/payment-notification.md).
* **Use the Correct Identifier:** Provide either the [session\_id](checkout-api.md#session_id-string-mandatory) or [order\_no](checkout-api.md#order_no-string-optional) when requesting a status inquiry.
* **Consider the Transaction State:** The states `paid` and `authorized` will return the status immediately, while others will trigger a status check with the [Payment Gateway](../user-guide/payment-gateway.md). Please refer to the [Operation Available](../user-guide/payment-gateway.md#available-operations) table to explore the processes supported by each Payment Gateway.

## [FAQ](payment-status-inquiry.md#faq)

#### :digit\_one: [**What are the prerequisites to using the Payment Status Inquiry API?**](payment-status-inquiry.md#what-are-the-prerequisites-to-using-the-payment-status-inquiry-api)

You should have at least one [Payment Gateway](../user-guide/payment-gateway.md) that supports status checks, and you should be familiar with the [Payment Webhook response](webhooks/payment-notification.md). Refer to [available operation](../user-guide/payment-gateway.md#available-operations) table to explore the PGs support status checks.

#### :digit\_two: [**What types of authentication does the Payment Status Inquiry API support?**](payment-status-inquiry.md#what-types-of-authentication-does-the-payment-status-inquiry-api-support)

The API endpoint supports both [API-Key](authentication.md#private-key-api-key) and [Basic Authentication](authentication.md#basic-authentication).

#### :digit\_three: [**Which payment transaction states can I inquire using the Payment Status Inquiry API?**](payment-status-inquiry.md#which-payment-transaction-states-can-i-inquire-using-the-payment-status-inquiry-api)

You can trigger the endpoint for payment transactions in the `pending`, `attempted`, `failed`, or `expired` states. If the transaction state is `paid` or `authorized`, the status is returned immediately. See [Payment Transactions State](../user-guide/payment-tracking/payment-transactions-states.md).

#### :digit\_four: [**How does Ottu handle inquiries for transactions with outdated states?**](payment-status-inquiry.md#how-does-ottu-handle-inquiries-for-transactions-with-outdated-states)

If a transaction state is not up-to-date and is still listed as `pending`, `attempted`, `failed`, or `expired`, Ottu will trigger an API call to the [Payment Gateway](../user-guide/payment-gateway.md) to update the transaction state.

#### :digit\_five: [**What if multiple payment options were attempted using different Payment Gateways?**](payment-status-inquiry.md#what-if-multiple-payment-options-were-attempted-using-different-payment-gateways)

If multiple payment options were `attempted` using different [Payment Gateways](../user-guide/payment-gateway.md), all Gateways that support payment status checks will be called, ensuring that you receive the most updated status for the payment.

#### :digit\_six: [**Are there any limitations or throttling rules for the Payment Status Inquiry API?**](payment-status-inquiry.md#are-there-any-limitations-or-throttling-rules-for-the-payment-status-inquiry-api)

Yes, the `Payment Status Inquiry API` has a throttling mechanism. It includes an initial grace period, a limit on the number of requests within certain time intervals, and a limit on the total requests in a single day. Please refer to the [Limitations](payment-status-inquiry.md#limitations) section for detailed information.

#### :digit\_seven: [**What information do I need to provide to request a status inquiry?**](payment-status-inquiry.md#what-information-do-i-need-to-provide-to-request-a-status-inquiry)

To request a status inquiry, you need to provide either the [session\_id](checkout-api.md#session_id-string-mandatory) or [order\_no](checkout-api.md#order_no-string-optional) for the transaction.

From [authentication](payment-status-inquiry.md#authentication) to [best practices](payment-status-inquiry.md#best-practices), this document has equipped users with the knowledge needed to maximize the potential of the `Status Inquiry API` in their online payment systems. Whether users are new to the platform or experienced, this tool can streamline payment processes and keep them informed about transaction statuses. If there are any questions or a need for further guidance, the [FAQ section](payment-status-inquiry.md#faq) is available. The team appreciates the trust placed in their payment management solutions and looks forward to supporting seamless and efficient transactions.
