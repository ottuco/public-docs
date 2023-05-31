# Payment Notification

## [Overview](payment-notification.md#overview)

The Ottu payment system provides a webhook designed specifically for payment events, including authorization, purchase, failed, canceled, and error. Whenever any of these payment events occur, Ottu will automatically trigger the webhook, sending a JSON payload to the specified [webhook\_url](../rest-api/checkout-api.md#webhook\_url-string-optional) provided in the [Checkout API](../rest-api/checkout-api.md). This JSON payload includes all relevant payment event results, payment details, and payment gateway responses, allowing merchants to receive and process this information in real-time, and to update their own systems accordingly.

By integrating with Ottu's payment webhook, merchants can streamline their payment processes and enhance their payment workflows. With publicly documented details and straightforward integration instructions, developers can seamlessly integrate Ottu's payment system into their own applications and online stores. Ultimately, the Ottu payment webhook provides a reliable, secure, and efficient way for merchants to manage their payment processing, enabling them to focus on growing their business and serving their customers.

## [Payload structure](payment-notification.md#payload-structure)

#### [amount](payment-notification.md#amount-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The initial amount of the payment transaction. See [amount](../rest-api/checkout-api.md#amount-string-required)\
Max length: 24\
Min value: 0.01

{% hint style="info" %}
The merchant should always check if the received amount from Ottu side is the amount of the order, to avoid user changing the cart amount in between.
{% endhint %}

#### [amount\_details](payment-notification.md#amount\_details-object-mandatory) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Payment transaction due amount details&#x20;

<details>

<summary>amount_details child parameters</summary>

#### :digit\_one:[currency\_code](payment-notification.md#currency\_code-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The code represents the used currency\
3 letters

#### :digit\_two:[amount](payment-notification.md#amount-string-mandatory-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Payment transaction original amount. See amount\
Max length: 24\
Min value: 0.01

#### :digit\_three: [total](payment-notification.md#total-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It represents the whole payment amount ([amount](payment-notification.md#amount-string-mandatory)+[fee](payment-notification.md#fee-string-mandatory))\
Max length: 24\
Min value: 0.01

#### :digit\_four: [fee](payment-notification.md#fee-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It represents a markup amount on the original amount\
Max length: 24\
Min value: 0.01

</details>

#### [capture\_delivery\_address](payment-notification.md#capture\_delivery\_address-bool-conditional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Indicates whether to capture delivery address.

**Presence condition:**

* The merchant should include it during the creation of the transaction.

#### [capture\_delivery\_location ](payment-notification.md#capture\_delivery\_location-bool-conditional)_<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Indicates whether to capture delivery location.

**Presence condition:**

* The merchant should include it during the creation of the transaction.

#### [currency\_code](payment-notification.md#currency\_code-string-mandatory-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The currency code of the payment transaction \
For more details, [https://en.wikipedia.org/wiki/ISO\_4217](https://en.wikipedia.org/wiki/ISO\_4217)\
3 letters code

<table><thead><tr><th>Billing address information</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td><p>Customer billing address data</p><p><br><strong>Presence condition:</strong> </p><ul><li>The customer should include the billing address data while making the payment of the transaction.</li></ul></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0031">1</span><a href="payment-notification.md#customer_address_city-string-conditional">customer_address_city</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>The city where the customer is living and registered<br>Max length: 40</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0032">2</span><a href="payment-notification.md#customer_address_country-string-conditional">customer_address_country</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>The country where the customer is living and registered<br>Based on ISO 3166-1 Alpha-2 code<br>Validation will be performed against existing countries<br>Max length: 2</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0033">3</span><a href="payment-notification.md#customer_address_line1-string-conditional">customer_address_line1</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>Customer's street &#x26; house data<br>Max length: 255</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0034">4</span><a href="payment-notification.md#customer_address_line2-string-conditional">customer_address_line2</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>Additional data for accuracy purpose for <a href="payment-notification.md#customer_address_line1-string-conditional">line1</a><br>Max length: 255</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0035">5</span> <a href="payment-notification.md#customer_address_postal_code-string-conditional">customer_address_postal_code</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>Postal code.<br>Max length 12 (it may have different length for different countries)</p></td><td></td><td></td></tr><tr><td><h4><span data-gb-custom-inline data-tag="emoji" data-code="0036">6</span><a href="payment-notification.md#customer_address_state-string-conditional">customer_address_state</a> <em><mark style="color:blue;"><code>string</code></mark></em> <em><mark style="color:blue;background-color:blue;"><code>conditional</code></mark></em></h4><p>State of the customer’s <a href="payment-notification.md#customer_address_city-string-conditional">city</a> (sometimes the same as the <a href="payment-notification.md#customer_address_city-string-conditional">city</a>)<br>Max length 40</p></td><td></td><td></td></tr></tbody></table>

#### [customer\_email](payment-notification.md#customer\_email-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Where to pass the customer’s email address\
Have to be a valid email address\
Max length 128

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [**customer\_first\_name**](payment-notification.md#customer\_first\_name-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

For the customer's first name\
Max length 64

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [**customer\_id**](payment-notification.md#customer\_id-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_&#x20;

Customer ID is created by a merchant, and stored in the merchant database\
Max length 64

**Presence condition:**

* The merchant should include it during the creation of the transaction.

#### [**customer\_last\_name**](payment-notification.md#customer\_last\_name-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

For the customer's last name\
Max length 64

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [**customer\_phone**](payment-notification.md#customer\_phone-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Where to pass the customer’s phone number\
Max length 32

**Presence condition:**

* The customer should include it while making the payment of the transaction.

#### [extra](payment-notification.md#extra-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

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

#### [fee](payment-notification.md#fee-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It represents a markup amount on the original amount.\
Max length: 24\
Min value: 0.01

**Presence condition:**

* The merchant should add it in the [currency configuration](../../user-guide/currencies.md#currency-configuration) and include it during the transaction creation.

#### [gateway\_account](payment-notification.md#gateway\_account-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The [code](../rest-api/checkout-api.md#pg\_codes-array-required) of the payment gateway used to proceed the payment\
Max length 16

#### [gateway\_name](payment-notification.md#gateway\_name-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The name of the [payment gateway](../../user-guide/payment-gateway.md) used to proceed the payment\
Max length 64

#### [gateway\_response](payment-notification.md#gateway\_response-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It will contain the raw payment gateway response sent by the payment gateway to Ottu.

**Presence condition:**

* It will only be present in response to the PG's callback request for the transaction.

#### [initiator](payment-notification.md#initiator-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

This object contains information about the user who created the transaction from Ottu side, specifically, the user who generated the payment URL

**Presence condition:**

* It is pressent only when [Basic Authentication](../rest-api/authentication.md#basic-authentication) is used, because [API Key Authentication](../rest-api/authentication.md#api-keys) is not associated with any user.
* Merchant includes the initiator ID in the payload when creating the transaction

#### [is\_sandbox](payment-notification.md#is\_sandbox-bool-conditional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

Whether the transaction was carried out in a sandbox environment.

**Presence conditions:**

* It will only be present when PG's setting configured as sandbox

#### [message](payment-notification.md#message-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

A message indicating the cause of a [payment attempt](../../user-guide/payment-tracking.md#payment-attempt) failure., which is directly related to the payment attempt itself\
Max length 255.

**Presence condition:**

* It will only be present if a payment attempt records an error.

#### [order\_no](payment-notification.md#order\_no-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It is a specific code that is assigned to a transaction by the merchant.\
By assigning a unique identifier to each transaction, the merchant can easily retrieve and review transaction details in the future, as well as identify any issues or discrepancies that may arise.\
such like : ABC123\_1, ABC123\_2\
Max length 128\


**Presence condition:**

* It will be present only if order\_no has been provided in the request payload.

#### [paid\_amount](payment-notification.md#paid\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

It is the amount that is credited to the merchant's bank account\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be present if a capture action is being processed on the transaction and the paid amount is recorded.

#### [reference\_number](payment-notification.md#reference\_number-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It is a unique identifier for the payment attempt, which can be used as a tracking identifier\
Max length 128

#### [refunded\_amount](payment-notification.md#refunded\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_&#x20;

The payment amount paid back from the merchant to the customer.\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be present if a refund action is being processed on the transaction and the refunded amount is recorded.

#### [remaining\_amount](payment-notification.md#remaining\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_&#x20;

The amount remaining to be paid in the transaction. ([amount](payment-notification.md#amount-string-required) – [settled amount](payment-notification.md#settled\_amount-string-conditional))\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be sent if the editable amount option is turned on.

#### [result](payment-notification.md#result-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Could be "success", "pending", "failed", "canceled", "error", and "cod". See [payment transaction states](../../user-guide/payment-tracking.md#payment-transactions-states)\
Max length 50\


#### [session\_id](payment-notification.md#session\_id-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Ottu unique identifier which gets generated when the transaction is created.\
It can be used to perform subsequent operations, like [retrieve, acknowledge, refund, capture, and cancelation](../rest-api/operations.md)\
Max length 128

#### [settled\_amount](payment-notification.md#settled\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

**Is the amount with the same currency of the initiating amount,**

* **For editable amount:** It is the amount that the customer enters at the checkout page
* **For  non-editable amount:** The settled amount is  the same value as the original payment amount

**Presence condition:**

* It will be present only if the transaction is paid, authorized or cod.

#### [state](payment-notification.md#state-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It is  one of the [Payment transaction state.](../../user-guide/payment-tracking.md#payment-transactions-states) And could one of the  below: \
**created, pending, attempted, authorized, paid, failed, canceled, expired, invalided, or cod.**\
Max length 50

#### [transaction\_log\_id](payment-notification.md#transaction\_log\_id-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The ID of the transaction log associated with the transaction.\
Max length 32-bit String (2^31 - 1)

**Presence condition:**

* It will be sent only if the transaction type is BULK as it's a bulk identifier.

#### [transaction  ](payment-notification.md#transaction-array-conditional)_<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

<details>

<summary>transaction  child parameters</summary>

Transactions resulted to the PG operations performed on the parent transaction\
See [child transaction sate](../../user-guide/payment-tracking.md#child-table-transaction)

**Presence conditions:**

* It will be sent only if operations processed on transaction and resulted child transaction records.

#### :digit\_one:[amount](payment-notification.md#amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The amount of child transaction object represented in transactions Array\
Must be positive\
Max length: 24\
Min value: 0.01

#### :digit\_two:[currency\_code](payment-notification.md#currency\_code-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The code represents the used currency.\
3 letters

#### :digit\_three:[order\_no](payment-notification.md#order\_no-stringconditional) _<mark style="color:blue;">`string`</mark><mark style="color:blue;background-color:blue;">`conditional`</mark>_

The order\_no of child transaction object represented in transactions Array

#### :digit\_four: [session\_id](payment-notification.md#session\_id-stringconditional) _<mark style="color:blue;">`string`</mark><mark style="color:blue;background-color:blue;">`conditional`</mark>_

The unique session identifier of child transaction object represented in transactions Array

#### :digit\_five:[state](payment-notification.md#state-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The state of a child transaction object represented in transactions Array

</details>

#### [voided\_amount](payment-notification.md#voided\_amount-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The payment amount resulted by performing [void](../../user-guide/payment-gateway.md#available-operations) operation\
Max length: 24\
Min value: 0.01

**Presence condition:**

* It will only be present if a void action is being processed on the transaction and the voided amount is recorded

## [Payload example (attempted)](payment-notification.md#payload-example-attempted-1)

<details>

<summary><a href="payment-notification.md#payload-example-attempted-1">Payload example (attempted)</a></summary>

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

</details>
