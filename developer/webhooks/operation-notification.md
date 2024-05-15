# Operation Notification

Ottu’s operation webhooks provide real-time insights into post-transaction activities, specifically focusing on `refund`, `capture`, and `void`. Operating asynchronously, they ensure merchants are promptly informed about these crucial subsequent payment gateway operations without disrupting the main transaction flow. By utilizing these webhooks, merchants gain a clearer view of their transactional landscape, enhancing decision-making and customer interactions.

## [Setup](operation-notification.md#setup)

To ensure you receive notifications for subsequent payment gateway operations, it’s essential to configure the operation webhooks correctly. Here’s a brief guide on setting it up:

1. **Webhook URL Configuration:** \
   There are two ways to configure `webhook_url` for operations&#x20;

* Configuring the operation `webhook_url` within webhook general configuration. For more details, click [here](../../user-guide/configuration/webhooks-configuration.md#general).
* Configuring the operation `webhook_url` through payment webhook plugin configuration. For further information, click [here](../../user-guide/configuration/webhooks-configuration.md#webhook-plugin-configs).

2. **Enable API-initiated Transaction Notifications:**\
   If you want to receive webhook notifications for transactions initiated via the API, ensure you check the box labeled “Enable webhook notifications if transaction initiated from API.” For instructions on how to do this, please consult the following [reference](../../user-guide/configuration/webhooks-configuration.md#general).
3. **Webhook Configuration Details**: \
   For a more in-depth understanding and additional configuration options, refer to the dedicated [webhook config section](../../user-guide/configuration/webhooks-configuration.md) in the documentation.
4. **Webhook Setup Requirements:** \
   It’s worth noting that the requirements for setting up operation webhooks align with those detailed in the “webhooks overview” page. Ensure you’re familiar with these requirements to guarantee a smooth setup process. For details on these requirements, click [here](./#endpoint-requirements).

By following these steps and ensuring your webhook is correctly configured, you’ll be well-equipped to receive timely updates on all your payment gateway operations.

## [Params](operation-notification.md#params)

#### [amount](operation-notification.md#amount-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The specific amount for which the operation was performed.

#### [initiator](operation-notification.md#initiator-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_&#x20;

Payment operation creator details.

**Presence Condition:**

* &#x20;It will be populated only if the operation was triggered from the dashboard or using API with [Basic Authentication](../authentication.md#basic-authentication) and not [API-Key Authentication](../authentication.md#private-key-api-key).

<details>

<summary>initiator child parameters</summary>

#### [id](operation-notification.md#id-integer-mandatory) _<mark style="color:blue;">`integer`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It represents the unique identifier of the user who performs the operation.

#### [first\_name](operation-notification.md#first\_name-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It represents the first name of the user who performs the operation.\
<= 32 characters

#### [last\_name](operation-notification.md#last\_name-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It represents the last name  of the user who performs the operation.\
<= 32 characters

#### [username](operation-notification.md#username-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It represents the username of the user who performs the operation.\
Required. 150 characters or fewer. Letters, digits and @/./+/-/\_ only.

#### [email](operation-notification.md#email-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The email address of the user who performs the operation.\
<= 254 characters

#### [phone](operation-notification.md#phone-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It represents the phone number of the user who performs the operation.\
<= 128 characters

</details>

#### [is\_sandbox](operation-notification.md#is\_sandbox-bool-mandatory) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Indicates whether the operation was performed in a test environment or not.

#### [operation ](operation-notification.md#operation-string-mandatory)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Identifies the operation that was executed.\
Could be "`capture`",  "`refund`", or  "`void`". For more infromation about operation, please refer to [External Operations documentations](../operations.md#external-operations).

#### [order\_no](operation-notification.md#order\_no-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It indicates the unique order number of the [Parent Transaction](../../user-guide/payment-tracking/payment-transactions-states.md#parent-payment-transaction). This identifier is crucial for tracking and managing the related order within its entire lifecycle. For more information about `order_no`, please refer to [order\_no](../checkout-api.md#order\_no-string-optional).

**Presence Condition:**

* Should be included during the creation of the parent transaction.

#### [pg\_code](operation-notification.md#pg\_code-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Represents the `pg_code` in the [Payment Gateway](../../user-guide/payment-gateway.md) setting utilized for the [operation](../operations.md#external-operations).

#### [pg\_response](operation-notification.md#pg\_response-object-mandatory) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It will contain the raw [payment gateway](../../user-guide/payment-gateway.md) response sent by the payment gateway to Ottu.\
It will always be a valid JSON.

#### [reference\_number](operation-notification.md#reference\_number-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

A unique `reference_number` assigned by Ottu for the performed operation. It's also sent to the [Payment Gateway](../../user-guide/payment-gateway.md) and can be used as a reconciliation parameter.

#### [result](operation-notification.md#result-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

**Since** it states if the operation was success or not, and webhook operations are not triggered if the operation has failed, so It is a **Fixed value:** success.&#x20;

#### [session\_id](operation-notification.md#session\_id-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The session ID of the [Parent Transaction](../../user-guide/payment-tracking/payment-transactions-states.md#parent-payment-transaction) will be included in the webhook payload. This session ID is crucial for associating the webhook event with the original transaction, allowing for accurate tracking and processing. For more details about `session_id`, please refer to [session\_id](../checkout-api.md#session\_id-string-mandatory).

#### [signature](operation-notification.md#signature-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

A cryptographic hash used to guarantee data integrity and authenticity during client-server exchanges. This hash ensures that the API payload has not been tampered with, and can only be verified by authorized parties. Please check [Signing Mechanism](signing-mechanism.md) for more information.

#### [source](operation-notification.md#source-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Can have one of two values - `input` or `pg`. &#x20;

* &#x20;`input`, it means the operation was performed in an API call triggered by the merchant. For more information about exteranl operation API, Please refer to [External Operations documentation](../operations.md#external-operations).
* `pg`, it means the operation was done on the PG management dashboard, and the PG notified Ottu via [webhook](./). The `pg` value will always be notified to the webhook, never in an API call.

#### [success](operation-notification.md#success-bool-mandatory) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Indicates whether the operation was successful or not (`success= true` or `success= false`).

#### [timestamp\_utc](operation-notification.md#timestamp\_utc-string-datetime-mandatory)  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`datetime`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Specifies the time when the operation was performed, in the UTC timezone.

#### [txn](operation-notification.md#txn-object-mandatory) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

An object will be generated including a short summary of the [child payment transaction](../../user-guide/payment-tracking/payment-transactions-states.md#child-payment-transaction) which was created.

<details>

<summary>txn  child parameters</summary>

#### [amount](operation-notification.md#amount-string-mandatory-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Requested amount of the payment operation.

#### [currency\_code](operation-notification.md#currency\_code-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The specified currency represents the denomination of the transaction. Nevertheless, it doesn't necessarily mandate payment in this exact currency. Due to potential currency conversions or exchanges, the final charge may be in a different currency. For more information, please refer to [Currencies documentation](../../user-guide/currencies.md).

#### [order\_no](operation-notification.md#order\_no-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Merchant unique identifier for the transaction. ABC123\_1, ABC123\_2, Max length: 128.

#### [session\_id](operation-notification.md#session\_id-string-mandatory-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

A unique identifier for each payment transaction, used to maintain the session state during the payment process.

#### [state](operation-notification.md#state-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The current state of the payment transaction, it helps to understand the progress of the payment. It can take on one of the following values: `refunded`, `refund-queued, refund-rejected, voided`, or `paid`. For additional information, please refer to [Payment operation state](../../user-guide/payment-tracking/payment-transactions-states.md#child-payment-states).

</details>

## [Event Types](operation-notification.md#event-types)

Operation webhooks are activated under the following scenarios:

1. **Ottu Dashboard Trigger:** When a payment operation (`refund`, `void`, or `capture`) is initiated directly from the Ottu dashboard, the system will automatically send a webhook notification to the subscriber’s registered endpoint.
2. **REST API Trigger:** If a payment operation is executed via Ottu’s REST API, [external operations API](../operations.md#external-operations), the webhook system will again ensure that a notification is dispatched to the subscriber’s endpoint. This ensures that even automated or system-driven operations are communicated in real-time.
3. **Payment Gateway Dashboard Trigger:** Some [Payment Gateways](../../user-guide/payment-gateway.md) (PG) have their own webhook systems in place. If a payment operation is performed on the Payment Gateway’s dashboard and that PG has enabled webhooks, it will notify Ottu. Ottu, in turn, will relay this information to the subscriber by triggering the operation webhook. This cascading notification ensures that even if operations are performed outside of Ottu’s immediate ecosystem, subscribers remain in the loop. To access further details regarding the available operations for each payment gateway, please click [here](../../user-guide/payment-gateway.md#available-operations).

## [Payload example (refund)](operation-notification.md#payload-example-refund)

```json
{
   "amount":"9.000",
   "initiator":{
      "email":"initaitor@example.com",
      "first_name":"example",
      "id":35,
      "last_name":"",
      "phone":"",
      "username":"username_example"
   },
   "is_sandbox":true,
   "operation":null,
   "order_no":"Y3ODg",
   "pg_code":"credit-card",
   "pg_response":{
      "It will contain the raw pg response sent by the pg to Ottu"
   },
   "reference_number":"staging4AQ64A",
   "result":"success",
   "session_id":"bb7fc280827c2f177a9690299cfefa4128dbbd60",
   "signature":"65f655d2161*************",
   "source":"input",
   "success":true,
   "timestamp_utc":"2023-11-02 09:02:06",
   "txn":{
      "amount":"9.000",
      "currency_code":"KWD",
      "order_no":"",
      "session_id":"43ae8773f2c61f2ef41e3024e3b8f8bf45667d44",
      "state":"refunded"
   }
}
```

## [Acknowledging an Operation](operation-notification.md#acknowledging-an-operation)

Upon receiving an operation notification, it’s essential to discern and acknowledge the operation’s status and specifics. Here’s a guide on how to interpret the provided details:\
\
**Identifying the Transaction:**\
Every operation is a subsequent action performed on a specific payment transaction, identifiable by its [session\_id](../checkout-api.md#session\_id-string-mandatory) or [order\_no](../checkout-api.md#order\_no-string-optional). These operations spawn [child payment transactions](../../user-guide/payment-tracking/payment-transactions-states.md#child-payment-states), each with its distinct payment [attempt](../../user-guide/payment-tracking/payment-transactions-states.md#payment-attempt) and [state](../../user-guide/payment-tracking/payment-transactions-states.md#child-payment-states), without altering the primary transaction. The child transaction details are housed in the [txn](operation-notification.md#txn-object-mandatory) field of the [webhook payload](operation-notification.md#payload-details). You can retrieve all child transactions from the Payment webhook under the transactions parameter or by invoking the [Payment Status API](../payment-status-inquiry.md).

1. **Types of Operations:**

* **Real-time Operations:** Immediate actions where, for instance, a refund request results in an instant refund. [Child transactions](../../user-guide/payment-tracking/payment-transactions-states.md#child-payment-transaction) are created for successful real-time operations, while failures aren’t stored.
* **Queued Operations:** The Payment Gateway (PG) might not approve the request instantly, placing it in a queue for later processing. Such operations initially bear a `<operation>_queued` state. Once processed, the state either transitions to the specific operation state (like `refunded`, `captured`, `voided`) or `<operation>_rejected`.

2. **Understanding Real-time and Queued Payment Gateways:**To determine which Payment Gateways operate in real-time and which ones use the queued system, you can consult the list provided [here](../../user-guide/payment-gateway.md#available-operations).
3. **Additional Information:** Upon receiving a `<operation>_queued` status, such as `refund_queued`, it’s imperative to archive this response. Ottu will subsequently notify the operation webhook URL with the conclusive result.
4. **Understanding the result Field:** [result](operation-notification.md#result-string) this field elucidates the operation’s outcome:

* `success`: The operation was executed successfully.
* `rejected`: The operation was declined. This status invariably succeeds a queued status.
* `queued`: The operation awaits processing and will be updated in due course.

5. **Verifying the Amount:** As operations exclusively function in the original payment transaction currency, inspecting the amount field ensures the accurate amount is either deducted or appended.
6. **Interpreting the Transaction State (Optional):** The transaction’s state can be discerned using the [txn.state](operation-notification.md#state-string-mandatory) field:

* `txn.state = paid`: The transaction was captured.
* `txn.refunded`: The transaction was refunded.
* `txn.refund_queued`: The refund operation is pending.
* `txn.refund_rejected`: The refund operation was declined.
* `txn.voided`: The transaction was voided.

By accurately interpreting these fields and states, you can ensure precise acknowledgment of all your subsequent payment operations.

**In Conclusion,** Navigating Ottu's operation webhooks can be a complex task, so it's essential to familiarize yourself with the overarching principles. We highly advise you to explore our extensive [Webhooks page](./) to gain a comprehensive understanding. Furthermore, should you encounter any inquiries or doubts, you might discover the solutions you need in our [FAQ section](./#faq). Our dedication to providing a smooth experience is greatly dependent on your in-depth grasp of our systems.
