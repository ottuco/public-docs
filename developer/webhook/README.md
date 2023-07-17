# Webhook

## [Getting started](./#getting-started)

Webhooks are automated messages sent from apps when something happens. \
They have either payload or message which are sent to a unique URL, essentially the app's phone number or address.

{% hint style="info" %}
<mark style="color:blue;">**Webhook vs API**</mark>\
**Webhook** is an event-based.\
It will run when a specific event occurs in the source app.\
**API** is request-based.\
It operates when requests come from other 3rd party apps.\

{% endhint %}

## [Ottu webhook](./#ottu-webhook)

Ottu has auto-trigger HTTP call to [operation webhook\_url](../../user-guide/configuration.md#operations-webhook\_url) when payment operation gets happened.\
For payment event, [webhook\_url](../rest-api/checkout-api.md#webhook\_url-string-optional)[ ](https://docs-ottu.gitbook.io/o/developer/rest-api/checkout-api#webhook\_url-url-optional)should be included in the payload, so call to [webhook\_url](../rest-api/checkout-api.md#webhook\_url-string-optional)[ ](https://docs-ottu.gitbook.io/o/developer/rest-api/checkout-api#webhook\_url-url-optional)will be triggered when the payment has been completed and BEFORE redirecting the customer back to merchant website.

## [Webhook mechanism](./#webhook-mechanism)

1- Payment event [authorized, paid, attempted, and failed](../../user-guide/payment-tracking.md#states-of-parent-payment-transaction), the transactional data will disclose to [webhook\_url](../rest-api/checkout-api.md#webhook\_url-string-optional)\
2- Payment transaction operation [void, capture, refund, and cancel](../../user-guide/payment-tracking.md#states-of-child-payment-transaction), the transactional data will disclose to [operation webhook\_url](../../user-guide/configuration.md#operations-webhook\_url)

### [Payload example](./#payload-example)

#### [Payment event (attempted)](./#payment-event-attempted)

```json
{
   "amount":"15.00",
   "currency_code":"SAR",
   "extra":{
      "flight-number":"43667",
      "full_name":"abc"
   },
   "gateway_account":"credit-card-test",
   "gateway_name":"mpgs",
   "gateway_response":{
      "It will contain the raw pg response sent by the pg to Ottu"
   },
   "message":"Unable to redirect to the PG",
   "reference_number":"stage45F3C",
   "result":"error",
   "session_id":"8eea795ead6d7cda7d12414bbae6a396adfe4758",
   "signature":"ee4a6bb00a75c7541b67d26ebf60243ca5652ef2b459f57c84d5586040952e9c",
   "state":"attempted",
   "total_authorized_amount":"0.00",
   "total_paid_amount":"0.00",
   "total_refunded_amount":"0.00",
   "total_voided_amount":"0.00"
}
```

#### [Payment operation (void)](./#payment-operation-void)

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

{% hint style="info" %}
**The main differences between the two payloads is: In payment event the result parameter is “error” since it is payload for attempted means it failed at first time of paying trial due to error and triggered another trial for paying, while in payment operation is “success”.**
{% endhint %}
