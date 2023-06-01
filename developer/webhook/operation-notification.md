# Operation Notification

### [Overview](operation-notification.md#overview)

This webhook is a valuable feature offered by Ottu that enables merchants to stay up-to-date with transactional activity. With this feature, Ottu can trigger a call to a designated webhook URL whenever a specific operation such as capture, refund, void, or cancel is performed, providing real-time transactional data. By receiving the relevant information, merchants can promptly respond to events and ensure the accuracy of their records.

Note that Operation Notifications configuration can be checked at [Webhook configuration document](../../user-guide/configuration.md#webhook-configuration). This documentation provides information on how to set up and configure the webhook URL for receiving the notifications, as well as how to manage and troubleshoot any issues that may arise during the configuration process.

## [Payload example (void)](operation-notification.md#payload-example-void)

```json
{
   "amount":"14.00",
   "initiator":{
      "email":"initiator@example.com",
      "id":83,
      "username":"initiator"
   },
   "is_sandbox":false,
   "operation":null,
   "pg_code":"mpgs-ksa",
   "pg_response":{
      "It will contain the raw pg response sent by the pg to Ottu."
   },
   "reference_number":"stageDV37C",
   "result":"success",
   "source":"input",
   "success":true,
   "timestamp_utc":"2022-09-07 06:21:46",
   "txn":{
      "amount":"14.00",
      "currency_code":"SAR",
      "customer_email":"customer@example.com",
      "extra":{
         "flight-number":"1234",
         "full_name":"customer"
      },
      "order_no":"",
      "reference_number":"LEQCJ",
      "state":"voided"
      }
}
```

## [Response parameters](operation-notification.md#response-parameters)

#### [amount](operation-notification.md#amount-string) _<mark style="color:blue;">`string`</mark>_

The merchant should always check if the amount he received from Ottu is the amount of the order, to avoid user changing the cart amount in between

#### [initiator](operation-notification.md#initiator-dict) _<mark style="color:blue;">`dict`</mark>_&#x20;

Payment operation creator details, it will be populated only if the operation was triggered from the dashboard or using api with basic auth and not api key

#### [is\_sandbox ](operation-notification.md#is\_sandbox-bool)_<mark style="color:blue;">`bool`</mark>_

If true, sandbox environment used for this PG settings.

#### [operation ](operation-notification.md#operation-string)_<mark style="color:blue;">`string`</mark>_

Choice from ["purchase","authorize".](../../user-guide/payment-gateway.md#configure-payment-gateway) Depending on how the PG is being selected.

#### [pg\_code ](operation-notification.md#pg\_code-string)_<mark style="color:blue;">`string`</mark>_

It is being generated according to user payment gateway [code](../rest-api/checkout-api.md#pg\_codes-array-required) choice from [pg\_codes](../rest-api/checkout-api.md#pg\_codes-array-required) list

#### [gateway\_response ](operation-notification.md#gateway\_response-dict)_<mark style="color:blue;">`dict`</mark>_

It will contain the raw [payment gateway](../../user-guide/payment-gateway.md) response sent by the [payment gateway](../../user-guide/payment-gateway.md) to Ottu.

#### [reference\_number](operation-notification.md#reference\_number-string) _<mark style="color:blue;">`string`</mark>_

It is a unique identifier, assigned by Ottu to any [parent payment transaction.](../../user-guide/payment-tracking.md#states-of-parent-payment-transaction)

#### [result](operation-notification.md#result-string) _<mark style="color:blue;">`string`</mark>_

**Since** it states if the operation was success or not, and webhook operations are not triggered if the operation has failed, so It is a **Fixed value:** success.&#x20;

#### [source ](operation-notification.md#source-string)_<mark style="color:blue;">`string`</mark>_

can be input or pg:\
**Input** means it was triggered by Ottu side via API or dashboard. PG means it was triggered by bank **PG** dashboard and Ottu was informed via webhook.\
**Note:** Not all PGs are informing Ottu when operations are happening on their side, so Ottu might not be aware of all operations on all PGs, only on those which are offering webhook feature.

#### [timestamp\_utc](operation-notification.md#timestamp\_utc-format-yyyy-mm-dd-hh-mm-ss) _<mark style="color:blue;">`format YYYY-MM-DD / HH:MM:SS`</mark>_&#x20;

Time and date of operation creation.

#### [txn ](operation-notification.md#txn-dict)_<mark style="color:blue;">`dict`</mark>_

A dictionary will be generated including a short summary of the [child payment transaction](../../user-guide/payment-tracking.md#states-of-child-payment-transaction) which was created

#### :digit\_one: [amount](operation-notification.md#amount-string-1) _<mark style="color:blue;">`string`</mark>_

Requested amount of the payment operation&#x20;

#### :digit\_two: [currency\_code](operation-notification.md#currency\_code-string) _<mark style="color:blue;">`string`</mark>_

The currency code of the payment operation.\
More details [https://en.wikipedia.org/wiki/ISO\_4217](https://en.wikipedia.org/wiki/ISO\_4217)\
3 letters code

#### :digit\_three: [customer\_email](operation-notification.md#customer\_email-string) _<mark style="color:blue;">`string`</mark>_

Email address of the customer.

#### :digit\_four: [extra ](operation-notification.md#extra-dict)_<mark style="color:blue;">`dict`</mark>_

The extra information for the payment details, which the merchant has sent it in key value form. For example:

```
"flight-number": "1234",
"full_name": "customer
```

#### :digit\_five: [order\_no ](operation-notification.md#order\_no-string)_<mark style="color:blue;">`string`</mark>_

Merchant unique identifier for the transaction. ABC123\_1, ABC123\_2, Max length: 128.

#### :digit\_six: [reference\_number ](operation-notification.md#reference\_number-string-1)_<mark style="color:blue;">`string`</mark>_

It is a unique identifier, assigned by Ottu to any child payment transaction , namely the [payment attempt.](../../user-guide/payment-tracking.md#payment-transaction)

#### :digit\_seven: [state ](operation-notification.md#state-string)_<mark style="color:blue;">`string`</mark>_

[Payment operation state](../../user-guide/payment-tracking.md#states-of-child-payment-transaction).
