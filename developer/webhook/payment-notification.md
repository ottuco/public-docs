# Payment Notification

###

## [Overview](payment-notification.md#overview)

The Ottu payment system provides a webhook designed specifically for payment events, including authorization, purchase, failed, canceled, and error. Whenever any of these payment events occur, Ottu will automatically trigger the webhook, sending a JSON payload to the specified [webhook\_url](../rest-api/checkout-api.md#webhook\_url-url-optional) provided in the [Checkout API](../rest-api/checkout-api.md). This JSON payload includes all relevant payment event results, payment details, and payment gateway responses, allowing merchants to receive and process this information in real-time, and to update their own systems accordingly.

By integrating with Ottu's payment webhook, merchants can streamline their payment processes and enhance their payment workflows. With publicly documented details and straightforward integration instructions, developers can seamlessly integrate Ottu's payment system into their own applications and online stores. Ultimately, the Ottu payment webhook provides a reliable, secure, and efficient way for merchants to manage their payment processing, enabling them to focus on growing their business and serving their customers.

## [Payload structure](payment-notification.md#payload-structure)

#### [amount](payment-notification.md#amount-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The initial amount of the payment transaction. See [amount](../../user-guide/payment-tracking.md#amount)\
Must be positive\
Max length: 24\
Min value: 0.01

{% hint style="info" %}
The merchant should always check if the received amount from Ottu side is the amount of the order, to avoid user changing the cart amount in between.
{% endhint %}

#### [amount\_detials](payment-notification.md#details) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

<details>

<summary>Details</summary>

_<mark style="color:blue;">It will be present only if the</mark>_ `currency_config` _<mark style="color:blue;">has been configured in the PG</mark>_ _<mark style="color:blue;">settings from the Ottu admin panel. See</mark>_ [currency configuration](../../user-guide/currencies.md#currency-configuration) & [payment gateway configuration](../../user-guide/payment-gateway.md#configure-payment-gateway)

#### :digit\_one:[currency\_code](payment-notification.md#currency\_code-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The code represents the used currency\
3 letters

#### :digit\_two:[amount](payment-notification.md#amount-string-required-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Payment transaction original amount. See [amount](../../user-guide/payment-tracking.md#amount)\
Must be positive\
Max length: 24\
Min value: 0.01

#### :digit\_three: [total](payment-notification.md#total-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It represents the whole payment amount ([amount](payment-notification.md#amount-string-required-1)+[fee](payment-notification.md#fee-string-optional))\
Must be positive\
Max length: 24\
Min value: 0.01

#### :digit\_four: [fee](payment-notification.md#fee-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

It represents a markup amount on the original amount\
Must be positive\
Max length: 24\
Min value: 0.01

</details>

#### [capture\_delivery\_address](payment-notification.md#capture\_delivery\_address-bool-optional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Indicates whether to capture delivery address\
_<mark style="color:blue;">It will be present only if the merchant includes it during the creation of the transaction.</mark>_

#### <mark style="color:blue;"><mark style="background-color:blue;"><mark style="background-color:blue;"></mark>[capture\_delivery\_location](payment-notification.md#capture\_delivery\_location-bool-optional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Indicates whether to capture delivery location\
_<mark style="color:blue;">It will be present only if the merchant includes it during the creation of the transaction.</mark>_

#### <mark style="color:purple;"></mark>[currency\_code](payment-notification.md#currency\_code-string-required-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The currency code of the payment transaction \
For more details, [https://en.wikipedia.org/wiki/ISO\_4217](https://en.wikipedia.org/wiki/ISO\_4217)\
3 letters code

#### [Billing address information](payment-notification.md#details-1)

<details>

<summary>Details</summary>

_<mark style="color:blue;">It will be present only if the customer includes it while making the payment of the transaction</mark>._

#### :digit\_one:[customer\_address\_city](payment-notification.md#customer\_address\_city-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The city where the customer is living and registered\
Max length: 40

#### :digit\_two:[customer\_address\_country](payment-notification.md#customer\_address\_country-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The country where the customer is living and registered\
Based on ISO 3166-1 Alpha-2 code\
Validation will be performed against existing countries\
Max length: 2

#### :digit\_three:[customer\_address\_line1](payment-notification.md#customer\_address\_line1-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Customer's street & house data\
Max length: 255

#### :digit\_four:[customer\_address\_line2](payment-notification.md#customer\_address\_line2-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Additional data for accuracy purpose for [line1](payment-notification.md#customer\_address\_line1-string-optional)\
Max length: 255

#### :digit\_five: [customer\_address\_postal\_code](payment-notification.md#customer\_address\_postal\_code-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Postal code.\
Max length 12 (it may have different length for different countries)

#### :digit\_six:[customer\_address\_state](payment-notification.md#customer\_address\_state-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

State of the customer’s [city](payment-notification.md#customer\_address\_city-string-optional) (sometimes the same as the [city](payment-notification.md#customer\_address\_city-string-optional))\
Max length 40

</details>

#### [customer\_email](payment-notification.md#customer\_email-string-optional) _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where to pass the customer’s email address\
Have to be a valid email address\
_<mark style="color:blue;">It will be present only if the customer includes it while making the payment of the transaction.</mark>_\
Max length 128

#### ****[**customer\_first\_name**](payment-notification.md#customer\_first\_name-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

For the customer's first name\
_<mark style="color:blue;">It will be present only if the customer includes it while making the payment of the transaction.</mark>_\
Max length 64

#### ****[**customer\_id**](payment-notification.md#customer\_id-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Customer ID is created by a merchant, and stored in the merchant database\
_<mark style="color:blue;">It will be present only if the merchant includes it during the creation of the transaction.</mark>_\ <mark style="color:blue;"></mark>Max length 64

#### ****[**customer\_last\_name**](payment-notification.md#customer\_last\_name-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

For the customer's last name\
_<mark style="color:blue;">It will be present only if the customer includes it while making the payment of the transaction</mark>._\
Max length 64

#### ****[**customer\_phone**](payment-notification.md#customer\_phone-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where to pass the customer’s phone number\
_<mark style="color:blue;">It will be present only if the customer includes it while making the payment of the transaction</mark>_\
_<mark style="color:blue;"></mark>_Max length 32

#### [extra](payment-notification.md#extra-dict) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The extra information for the payment details, which the merchant has sent it in key value form.\
_<mark style="color:blue;">The presence of the element will depend on whether the merchant includes it during transaction creation by adding each element from the plugin field configuration.</mark>_\
__For example:

```json
   "extra":{
      "flight-number":"43667",
      "full_name":"abc"
   },
```

#### [fee](payment-notification.md#fee-string-optional-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

It represents a markup amount on the original amount.\
_<mark style="color:blue;">It will only be present if the merchant adds it in the</mark>_ [_<mark style="color:blue;">currency configuration</mark>_](../../user-guide/currencies.md#currency-configuration) _<mark style="color:blue;">and includes it during the transaction creation.</mark>_\
Must be positive\
Max length: 24\
Min value: 0.01

#### [gateway\_account](payment-notification.md#gateway\_account-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The [code](../rest-api/checkout-api.md#pg\_codes-list-required) of the payment gateway used to proceed the payment\
Max length 16

#### [gateway\_name](payment-notification.md#gateway\_name-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

The name of the [payment gateway](../../user-guide/payment-gateway.md) used to proceed the payment\
Max length 64

#### [gateway\_response](payment-notification.md#gateway\_response-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

It will contain the raw payment gateway response sent by the payment gateway to Ottu\
_<mark style="color:blue;">It will only be present in response to the PG's callback request for the transaction.</mark>_

#### <mark style="color:blue;"></mark>[initiator](payment-notification.md#initiator-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

This object contains information about the user who created the transaction from Ottu side, specifically, the user who generated the payment URL\
_<mark style="color:blue;">It will only be present if the merchant includes the initiator ID in the payload when creating the transaction</mark>_

#### [is\_sandbox](payment-notification.md#is\_sandbox-bool-optional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Whether the transaction was carried out in a sandbox environment.\
_<mark style="color:blue;">It will only be present when PG's setting configured as sandbox</mark>_

#### __[message](payment-notification.md#message-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

A message indicating the cause of a [payment attempt](../../user-guide/payment-tracking.md#payment-attempt) failure., which is directly related to the payment attempt itself\
_<mark style="color:blue;">It will only be present if a payment attempt records an error.</mark>_\
Max length 255.

#### [order\_no](payment-notification.md#order\_no-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Merchant unique identifier for the transaction\
such like : ABC123\_1, ABC123\_2\
Max length 128

#### [paid\_amount](payment-notification.md#paid\_amount-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

It is the amount that is credited to the merchant's bank account\
_<mark style="color:blue;">It will only be present if a capture action is being processed on the transaction and the paid amount is recorded</mark>_\
Must be positive\
Max length: 24\
Min value: 0.01

#### [reference\_number](payment-notification.md#reference\_number-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It is a unique identifier for the payment attempt, which can be used as a tracking identifier\
Max length 128

#### [refunded\_amount](payment-notification.md#refunded\_amount-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_&#x20;

The payment amount paid back from the merchant to the customer.\
_<mark style="color:blue;">It will only be present if a refund action is being processed on the transaction and the refunded amount is recorded.</mark>_\
Must be positive\
Max length: 24\
Min value: 0.01

#### [remaining\_amount](payment-notification.md#remaining\_amount-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_&#x20;

The amount remaining to be paid in the transaction. ([amount](payment-notification.md#amount-string-required) – [settled amount](payment-notification.md#settled\_amount-string-optional))\
Must be positive\
_<mark style="color:blue;">It will only be sent if the editable amount option is turned on.</mark>_\
Max length: 24\
Min value: 0.01

#### [result](payment-notification.md#result-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

Could be "success", "pending", "failed", "canceled", "error", and "cod". See [payment transaction states](../../user-guide/payment-tracking.md#payment-transactions-states)\
Max length 50\


#### [session\_id](payment-notification.md#session\_id-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

Ottu unique identifier which gets generated when the transaction is created.\
It can be used to perform subsequent operations, like [retrieve, acknowledge, refund, capture, and cancelation](../rest-api/operations/)\
Max length 128

#### [settled\_amount](payment-notification.md#settled\_amount-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

**Is the amount with the same currency of the initiating amount,**

* **For editable amount:** It is the amount that the customer enters at the checkout page
* **For  non-editable amount:** The settled amount is  the same value as the original payment amount

_<mark style="color:blue;">It will be present only if transaction is paid, authorized or cod</mark>_

#### [state](payment-notification.md#state-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;background-color:blue;">`mandatory`</mark>_

It is  one of the [Payment transaction state.](../../user-guide/payment-tracking.md#payment-transactions-states) And could one of the  below: \
**created, pending, attempted, authorized, paid, failed, canceled, expired, invalided, or cod.**\
****Max length 50

#### [transaction\_log\_id](payment-notification.md#transaction\_log\_id-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The ID of the transaction log associated with the transaction.\
Max length 32-bit String (2^31 - 1)\
_<mark style="color:blue;">It will be sent only if transaction type is BULK as it's a bulk identifier.</mark>_&#x20;

#### [transaction ](payment-notification.md#details-2) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:blue;">`optional`</mark>_

<details>

<summary>Details</summary>

Transactions resulted to the PG operations performed on the parent transaction\
See [child transaction sate](../../user-guide/payment-tracking.md#child-table-transaction)\
_<mark style="color:blue;">It will be sent only if operations processed on transaction and resulted child transaction records</mark>_\


#### :digit\_one:[amount](payment-notification.md#amount-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The amount of child transaction object represented in transactions Array\
Must be positive\
Max length: 24\
Min value: 0.01

#### :digit\_one:[currency\_code](payment-notification.md#currency\_code-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The code represents the used currency.\
3 letters

#### :digit\_three:[order\_no](payment-notification.md#order\_no-string-optional-1) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The order\_no of child transaction object represented in transactions Array

#### :digit\_four: [session\_id](payment-notification.md#session\_id-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The unique session identifier of child transaction object represented in transactions Array

#### :digit\_five:[state](payment-notification.md#state-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The state of child transaction object represented in transactions Array



</details>

#### [voided\_amount](payment-notification.md#voided\_amount-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`optional`</mark>_

The payment amount resulted by performing [void](../../user-guide/payment-gateway.md#available-operations) operation\
_<mark style="color:blue;">It will only be present if a void action is being processed on the transaction and the voided amount is recorded</mark>_\
Must be positive\
Max length: 24\
Min value: 0.01

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
