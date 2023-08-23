# Payment Webhooks

## [Introduction](payment-webhooks.md#introduction)

Payment webhooks are specific to payment events and are triggered on multiple occasions:

1.  #### Post-Payment Completion

    Once a payer has completed the payment process and awaits redirection. To get notified for this event, the [webhook\_url](../checkout-api.md#webhook\_url-string-optional) must either be sent via the [Checkout API ](../checkout-api.md)when the payment transaction is created or set as the default [webhook\_url](../../user-guide/configuration.md#webhook-configuration) in the Ottu dashboard to apply for all transactions.
2.  #### Automatic Inquiry by Ottu

    If a payment transaction has an associated [webhook\_url](../checkout-api.md#webhook\_url-string-optional), it can be notified during the automatic inquiry process. This can happen immediately after the payer completes the payment process or later if the payment encounters an error. More details about the timings for automatic inquiry can be found [here](../payment-status-inquiry.md#automatic-inquiry).
3.  #### Manual Inquiry by Staff

    When a staff member initiates a manual inquiry from the Ottu dashboard.
4.  #### Manual Notification by Staff

    When a staff manually triggers a notification to the [webhook\_url](../checkout-api.md#webhook\_url-string-optional) from the Ottu dashboard.
5.  #### Merchant-Initiated Inquiry

    When an [inquiry API](../payment-status-inquiry.md#b-pbl-v2-inquiry) call is initiated by the merchant. Optionally, a notification can be sent to the [webhook\_url](../checkout-api.md#webhook\_url-string-optional) associated with the payment transaction or to a new one specified during the inquiry API call.

## [Webhook Parameters](payment-webhooks.md#webhook-parameters)

#### [amount](payment-webhooks.md#amount-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The initial amount of the payment transaction. See [amount](../checkout-api.md#amount-string-required)\
Max length: 24\
Min value: 0.01

{% hint style="info" %}
The merchant should always check if the received amount from Ottu side is the amount of the order, to avoid user changing the cart amount in between.
{% endhint %}

#### [amount\_details](payment-webhooks.md#amount\_details-object-mandatory) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Payment transaction due amount details&#x20;

<details>

<summary>amount_details child parameters</summary>

#### :digit\_one:[currency\_code](payment-webhooks.md#currency\_code-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The code represents the used currency\
3 letters

#### :digit\_two:[amount](payment-webhooks.md#amount-string-mandatory-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Payment transaction original amount. See amount\
Max length: 24\
Min value: 0.01

#### :digit\_three: [total](payment-webhooks.md#total-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It represents the whole payment amount ([amount](payment-webhooks.md#amount-string-mandatory)+[fee](payment-webhooks.md#fee-string-mandatory))\
Max length: 24\
Min value: 0.01

#### :digit\_four: [fee](payment-webhooks.md#fee-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It represents a markup amount on the original amount\
Max length: 24\
Min value: 0.01

</details>

#### [capture\_delivery\_address](payment-webhooks.md#capture\_delivery\_address-bool-conditional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Indicates whether to capture delivery address.

**Presence condition:**

* The merchant should include it during the creation of the transaction.

#### [capture\_delivery\_location ](payment-webhooks.md#capture\_delivery\_location-bool-conditional)_<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Indicates whether to capture delivery location.

**Presence condition:**

* The merchant should include it during the creation of the transaction.

#### [currency\_code](payment-webhooks.md#currency\_code-string-mandatory-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The currency code of the payment transaction \
For more details, [https://en.wikipedia.org/wiki/ISO\_4217](https://en.wikipedia.org/wiki/ISO\_4217)\
3 letters code

<table><thead><tr><th>Billing address information</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td><p>Customer billing address data</p><p><br><strong>Presence condition:</strong> </p><ul><li>The customer should include the billing address data while making the payment of the transaction.</li></ul></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0031">1</span><a href="payment-webhooks.md#customer_address_city-string-conditional">customer_address_city</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>The city where the customer is living and registered<br>Max length: 40</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0032">2</span><a href="payment-webhooks.md#customer_address_country-string-conditional">customer_address_country</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>The country where the customer is living and registered<br>Based on ISO 3166-1 Alpha-2 code<br>Validation will be performed against existing countries<br>Max length: 2</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0033">3</span><a href="payment-webhooks.md#customer_address_line1-string-conditional">customer_address_line1</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>Customer's street &#x26; house data<br>Max length: 255</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0034">4</span><a href="payment-webhooks.md#customer_address_line2-string-conditional">customer_address_line2</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>Additional data for accuracy purpose for <a href="payment-webhooks.md#customer_address_line1-string-conditional">line1</a><br>Max length: 255</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0035">5</span> <a href="payment-webhooks.md#customer_address_postal_code-string-conditional">customer_address_postal_code</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>Postal code.<br>Max length 12 (it may have different length for different countries)</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0036">6</span><a href="payment-webhooks.md#customer_address_state-string-conditional">customer_address_state</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>State of the customer’s <a href="payment-webhooks.md#customer_address_city-string-conditional">city</a> (sometimes the same as the <a href="payment-webhooks.md#customer_address_city-string-conditional">city</a>)<br>Max length 40</p></td><td></td><td></td></tr></tbody></table>

#### [customer\_email](payment-webhooks.md#customer\_email-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Where to pass the customer’s email address\
Have to be a valid email address\
Max length 128

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [**customer\_first\_name**](payment-webhooks.md#customer\_first\_name-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

For the customer's first name\
Max length 64

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [**customer\_id**](payment-webhooks.md#customer\_id-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_&#x20;

Customer ID is created by a merchant, and stored in the merchant database\
Max length 64

**Presence condition:**

* The merchant should include it during the creation of the transaction.

#### [**customer\_last\_name**](payment-webhooks.md#customer\_last\_name-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

For the customer's last name\
Max length 64

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [**customer\_phone**](payment-webhooks.md#customer\_phone-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Where to pass the customer’s phone number\
Max length 32

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [extra](payment-webhooks.md#extra-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

The extra information for the payment details, which the merchant has sent it in key value form.

**Presence condition:**

* The presence of the element will depend on whether the merchant includes it during transaction creation by adding each element from the plugin field configuration.

For example:

```json
   "extra":{
      "flight-number":"43667",
      "full_name":"abc"
   },
```

#### [fee](payment-webhooks.md#fee-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It represents a markup amount on the original amount.\
Max length: 24\
Min value: 0.01

**Presence condition:**

* The merchant should add it in the [currency configuration](../../user-guide/currencies.md#currency-configuration) and include it during the transaction creation.

#### [gateway\_account](payment-webhooks.md#gateway\_account-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The [code](../checkout-api.md#pg\_codes-array-required) of the payment gateway used to proceed the payment\
Max length 16

#### [gateway\_name](payment-webhooks.md#gateway\_name-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The name of the [payment gateway](../../user-guide/payment-gateway.md) used to proceed the payment\
Max length 64

#### [gateway\_response](payment-webhooks.md#gateway\_response-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It will contain the raw payment gateway response sent by the payment gateway to Ottu.

**Presence condition:**

* It will only be present in response to the PG's callback request for the transaction.

#### [initiator](payment-webhooks.md#initiator-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

This object contains information about the user who created the transaction from Ottu side, specifically, the user who generated the payment URL

**Presence condition:**

* It is present only when [Basic Authentication](../authentication.md#basic-authentication) is used, because [API Key Authentication](../authentication.md#api-key) is not associated with any user.
* Merchant includes the initiator ID in the payload when creating the transaction

#### [is\_sandbox](payment-webhooks.md#is\_sandbox-bool-conditional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

Whether the transaction was carried out in a sandbox environment.

**Presence conditions:**

* It will only be present when PG's setting configured as sandbox

#### [message](payment-webhooks.md#message-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

A message indicating the cause of a [payment attempt](../../user-guide/payment-tracking.md#payment-attempt) failure., which is directly related to the payment attempt itself\
Max length 255.

**Presence condition:**

* It will only be present if a payment attempt records an error.

#### [order\_no](payment-webhooks.md#order\_no-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It is a specific code that is assigned to a transaction by the merchant.\
By assigning a unique identifier to each transaction, the merchant can easily retrieve and review transaction details in the future, as well as identify any issues or discrepancies that may arise.\
such like : ABC123\_1, ABC123\_2\
Max length 128\


**Presence condition:**

* It will be present only if order\_no has been provided in the request payload.

#### [paid\_amount](payment-webhooks.md#paid\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It is the amount that is credited to the merchant's bank account\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be present if a capture action is being processed on the transaction and the paid amount is recorded.

#### [reference\_number](payment-webhooks.md#reference\_number-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It is a unique identifier for the payment attempt, which can be used as a tracking identifier\
Max length 128

#### [refunded\_amount](payment-webhooks.md#refunded\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_&#x20;

The payment amount paid back from the merchant to the customer.\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be present if a refund action is being processed on the transaction and the refunded amount is recorded.

#### [remaining\_amount](payment-webhooks.md#remaining\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_&#x20;

The amount remaining to be paid in the transaction. ([amount](payment-webhooks.md#amount-string-required) – [settled amount](payment-webhooks.md#settled\_amount-string-conditional))\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be sent if the editable amount option is turned on.

#### [result](payment-webhooks.md#result-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Could be "success", "pending", "failed", "canceled", "error", and "cod". See [payment transaction states](../../user-guide/payment-tracking.md#payment-transactions-states)\
Max length 50\


#### [session\_id](payment-webhooks.md#session\_id-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Ottu unique identifier which gets generated when the transaction is created.\
It can be used to perform subsequent operations, like [retrieve, acknowledge, refund, capture, and cancelation](../operations.md).\
Max length 128

#### [settled\_amount](payment-webhooks.md#settled\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

**Is the amount with the same currency of the initiating amount,**

* **For editable amount:** It is the amount that the customer enters at the checkout page
* **For on-editable amount:** The settled amount is the same value as the original payment amount

**Presence condition:**

* It will be present only if the transaction is paid, authorized or cod.

#### [signature](payment-webhooks.md#signature-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

A cryptographic hash used to guarantee data integrity and authenticity during client-server exchanges. This hash ensures that the API payload has not been tampered with, and can only be verified by authorized parties.

#### [state](payment-webhooks.md#state-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It is one of the [Payment transaction state.](../../user-guide/payment-tracking.md#payment-transactions-states) And could one of the below: \
**created, pending, attempted, authorized, paid, failed, canceled, expired, invalided, or cod.**\
Max length 50

#### [transaction\_log\_id](payment-webhooks.md#transaction\_log\_id-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The ID of the transaction log associated with the transaction.\
Max length 32-bit String (2^31 - 1)

**Presence condition:**

* It will be sent only if the transaction type is BULK as it's a bulk identifier.

#### [transaction  ](payment-webhooks.md#transaction-array-conditional)_<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

<details>

<summary>transaction  child parameters</summary>

Transactions resulted to the PG operations performed on the parent transaction\
See [child transaction sate](../../user-guide/payment-tracking.md#child-table-transaction)

**Presence conditions:**

* It will be sent only if operations processed on transaction and resulted child transaction records.

#### :digit\_one:[amount](payment-webhooks.md#amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The amount of child transaction object represented in transactions Array\
Must be positive\
Max length: 24\
Min value: 0.01

#### :digit\_two:[currency\_code](payment-webhooks.md#currency\_code-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The code represents the used currency.\
3 letters

#### :digit\_three:[order\_no](payment-webhooks.md#order\_no-stringconditional) _<mark style="color:blue;">`string`</mark><mark style="color:blue;background-color:blue;">`conditional`</mark>_

The order\_no of child transaction object represented in transactions Array

#### :digit\_four: [session\_id](payment-webhooks.md#session\_id-stringconditional) _<mark style="color:blue;">`string`</mark><mark style="color:blue;background-color:blue;">`conditional`</mark>_

The unique session identifier of child transaction object represented in transactions Array

#### :digit\_five:[state](payment-webhooks.md#state-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The state of a child transaction object represented in transactions Array

</details>

#### [token](payment-webhooks.md#token-object-conditional)  _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

<details>

<summary>token child parameters</summary>

Represents token details.

* **Presence conditions:**

When user pays with a tokenized card, Ottu will include the token details in the response

#### :digit\_one:[brand](payment-webhooks.md#brand-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The card brand (e.g., Visa, Mastercard) associated with the card. Display this information for customer reference.

#### :digit\_two:[auto\_debit\_enabled](payment-webhooks.md#auto\_debit\_enabled-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Define if provided card token can be used to initiate auto debit requests.

#### :digit\_three: [customer\_id](payment-webhooks.md#customer\_id-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The unique identifier for the customer who owns the card\
Max length: 36

#### :digit\_four: [cvv\_required](payment-webhooks.md#cvv\_required-bool-mandatory) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Specifies if the card requires the submission of a CVV for transactions. A card without CVV requirement can be used for auto-debit or recurring payments

#### :digit\_five:[ expiry\_month](payment-webhooks.md#expiry\_month-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The card's expiration month. Provide this information for transaction processing and validation.\
Max length: 2

#### :digit\_six: [expiry\_year](payment-webhooks.md#expiry\_yearstring-mandatory)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The card's expiration year. Provide this information for transaction processing and validation.\
Max length: 2

#### :digit\_seven: [is\_expired](payment-webhooks.md#is\_expired-bool-mandatory) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

A boolean field indicating whether the card has expired. Use this information to determine if the card is valid for transactions and to notify the customer if necessary.

</details>

#### [voided\_amount](payment-webhooks.md#voided\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The payment amount resulted by performing [void](../../user-guide/payment-gateway.md#available-operations) operation\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be present if a void action is being processed on the transaction and the voided amount is recorded.

## [**Event Types**](payment-webhooks.md#event-types)

Ottu notifies the [webhook\_url](../checkout-api.md#webhook\_url-string-optional) for all payment event types, not just success. This includes statuses like `error`, `failed`, `pending`, `rejected`, etc. The payload provides enough context to identify the status of the event.&#x20;

{% hint style="info" %}
&#x20;Events like **Refund**, **Void**, or **Capture** are considered operation events and not payment events. If you’re looking for information on these, please refer to the [Operation Webhook page](operation-notification.md).
{% endhint %}

#### [**Setup & Configuration**](payment-webhooks.md#setup-and-configuration)

Ensure you meet all the endpoint requirements mentioned in the Webhooks Overview [here](./#endpoint-requirements). Additionally, if you want the payer to be finally redirected to your website, ensure your endpoint returns an HTTP status of 200. Any other status will retain the payer on the Ottu payment details page. If you intentionally wish to keep the payer on the Ottu page, return a status code of 201. This will be interpreted by Ottu as a successful notification, and the payer won’t be redirected. Otherwise, Ottu will consider the notification as failed. In cases where you want to redirect the payer to a specific URL after the payment process, ensure you provide the [redirect\_url](../checkout-api.md#redirect\_url-string-optional) during the payment setup. This URL will be used by Ottu to guide the payer back to your platform or any desired page after the payment process is completed. See [Webhook Configuration](../../user-guide/configuration.md#webhook-configuration).

## [Redirectional Diagram](payment-webhooks.md#redirectional-diagram)

To ensure a smooth redirection of the payer back to the designated [redirect\_url](../checkout-api.md#redirect\_url-string-optional), it is essential that the `redirect_url` is accurately provided during the payment setup. Additionally, the [webhook\_url](../checkout-api.md#webhook\_url-string-optional) must respond with a status code of 200. This specific status code serves as a confirmation of successful interaction between the involved systems, ultimately guaranteeing the seamless progression of the redirection process as originally intended.

**Redirect behavior based on webhook\_url response:**\
\-[ status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)  200,  the customer will be redirected to [redirect\_url](../checkout-api.md#redirect\_url-string-optional).\
\-[ status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)  201,  the customer will be redirected to Ottu payment summary page.\
\-[ status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)  any other code, the customer will be redirected to Ottu payment summary page. For this particular case, Ottu can notify on the email, when Enable webhook notifications?  Activated

<figure><img src="../../.gitbook/assets/chr2 (1).png" alt=""><figcaption></figcaption></figure>

## [Payload example (attempted)](payment-webhooks.md#payload-example-attempted-1)

```json
{
    "amount":"0.01",
    "amount_details":{
       "currency_code":"KWD",
       "amount":"0.01",
       "total":"0.01",
       "fee":"0.00"
    },
    "currency_code":"KWD",
    "customer_email":"test@ottu.com",
    "customer_phone":"96511111111",
    "fee":"0.00 KWD",
    "gateway_account":"test",
    "gateway_name":"test",
    "message":"test",
    "gateway_response":{
        "It will contain the raw pg response sent by the pg to Ottu"
    },
    "initiator": {
        "id": 1, "first_name":"test", "last_name":"test", "username":"test", "email":"test@ottu.com", "phone":"+911111111111"
    },
    "is_sandbox":true,
    "order_no":"t-1111",
    "paid_amount":"0.01",
    "reference_number":"test111",
    "result":"success",
    "session_id":"111111111111111111111111111111111111",
    "settled_amount":"0.01",
    "signature":"11111111111111111111111111111111111111111111111111111111",
    "state":"paid",
    "transactions":[
       {
          "amount":"0.01",
          "currency_code":"KWD",
          "order_no":"None",
          "session_id":"1111111111111111111111111111111111111111111",
          "state":"refund_queued"
       }
    ]
 }
```

## [Acknowledging a Payment](payment-webhooks.md#acknowledging-a-payment)

When you receive a payment notification, it’s crucial to understand and acknowledge the payment’s status and details. Here’s how you can interpret the information:

#### **1. Payment Events Types**

There are several types of payment events you might encounter:

* **Payment:** This indicates a direct payment transaction.
* **Authorization:** This signifies a payment authorization, which might be captured later.
* **Cash (Offline):** This represents an offline payment, often referred to as Cash on Delivery (**COD**).

#### **2. Interpreting the** `result` fi**eld**

The [result](payment-webhooks.md#result-string-mandatory) field is your primary indicator of the payment’s outcome:

* If the `result` is `success`, it means the payment was successful.
* If the `result` is `failure`, it indicates an unsuccessful payment attempt.
* For cash payments, the `result` field will be `cod`, indicating Cash on Delivery.

#### **3. Understanding the operation Field**

The operation field provides insight into the type of transaction:

* If the operation is `payment`, it indicates a direct payment.
* If the operation is `authorization`, it signifies a payment that’s been authorized but not yet captured.

#### **4. Verifying the Amount**

* The [amount](payment-webhooks.md#amount-string-mandatory) field provides the value the customer has paid in the currency set during the payment setup.
* However, the actual amount captured by the [Payment Gateway](../../user-guide/payment-gateway.md) (PG) might differ. This can be due to additional fees, currency conversion, or other factors. To get the exact amount captured by the PG, refer to [amount\_details.amount](payment-webhooks.md#amount\_details-object-mandatory). The currency in which this amount is denominated can be found in [amount\_details.currency\_code](payment-webhooks.md#currency\_code-string-mandatory).
* In most scenarios, cross-referencing with the amount field should suffice. But if there are discrepancies or if you’ve set up fees or currency conversions, it’s advisable to verify with `amount_details`.

#### **5. Cash Payments**

For Cash on Delivery transactions, the [result](payment-webhooks.md#result-string-mandatory) field will specifically be `cod`. This helps differentiate offline payments from online ones.

By understanding and interpreting these fields correctly, you can ensure accurate and timely acknowledgment of all your payments, be they online or offline.

## [**FAQ**](payment-webhooks.md#faq)

#### :digit\_one:[What are webhooks and why would I use them?](payment-webhooks.md#what-are-webhooks-and-why-would-i-use-them)

AWebhooks are automated messages sent from Ottu to a specified endpoint when a particular event occurs. They’re useful for real-time notifications, allowing you to automate reactions to events like payment completions or gateway [operations](../operations.md#external-operations) without constantly polling our API.

#### :digit\_two:[I’ve set up my webhook, but I’m not receiving any notifications. What could be the issue?](payment-webhooks.md#ive-set-up-my-webhook-but-im-not-receiving-any-notifications.-what-could-be-the-issue)

There could be several reasons:

1. Ensure that the webhook is enabled, either via the Ottu dashboard or the [Checkout API](../checkout-api.md).
2. Check that your endpoint meets all the [requirements](./#endpoint-requirements), such as being secure (HTTPS) and having a response time under 25 seconds.
3. Verify that your endpoint returns an HTTP status of 200 or 201 to acknowledge receipt.
4. If you’re still facing issues, reach out to our support team for assistance.

#### :digit\_three:[How can I ensure the webhook notifications I receive are genuinely from Ottu?](payment-webhooks.md#how-can-i-ensure-the-webhook-notifications-i-receive-are-genuinely-from-ottu)

Ottu uses the **SHA-256** [signing mechanism](signing-mechanism.md) to sign all webhook payloads. By verifying the signature attached to each payload, you can ensure its authenticity and integrity. Detailed steps on how to verify the signature can be found [here](signing-mechanism.md#hmac-generation).

#### :digit\_four:[What should I do if I receive the same webhook notification multiple times?](payment-webhooks.md#what-should-i-do-if-i-receive-the-same-webhook-notification-multiple-times)

Webhooks can be retried, especially in cases of timeouts. It’s essential to make your webhook processing **idempotent**, meaning processing the same webhook notification more than once should not have a different effect. Always check the event ID or transaction ID to ensure you’re not processing duplicates.

#### :digit\_five:[How long will Ottu retry a webhook if my server doesn’t respond?](payment-webhooks.md#how-long-will-ottu-retry-a-webhook-if-my-server-doesnt-respond)

By default, Ottu will retry a webhook three times with a backoff factor of 5 seconds between retries. This behavior can be adjusted in the webhook settings.

Feel free to ask any other questions or provide more context, and we can craft more FAQs accordingly!
