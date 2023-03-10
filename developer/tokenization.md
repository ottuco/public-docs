# Tokenization

## [Getting started](tokenization.md#getting-started)

By using tokenization, Ottu safeguards customer's sensitive card data from potential hacks or unauthorized access.

**In comparison** with other methods such as encryption, tokenization offers a higher level of security due to no way to reverse the process of generating tokens.

## Ottu Tokenization

* Ottu tokenization could be used with [Web](broken-reference),[ iOS](checkout-sdk/ios.md), [Android](checkout-sdk/android.md), and [Flutter](checkout-sdk/flutter.md) SDKs.

**In order to generate token:**

1. At the merchant side:
   * [cusotmer\_id](rest-api/checkout-api.md#customer\_id-string-optional) parameter should be sent by payment checkout request payload.
   * &#x20;Save card option should be enabled in the payment gateway configuration **(contact Ottu technical team).**
2. At the customer side on the checkout page:
   * &#x20;**Save Your Card** option should be checked.
   * The payment should be proceeded **successfully**.

{% hint style="info" %}
Depending on payment gateway configuration, CCV could be required by enabling CCV option in the payment gateway configuration **(contact Ottu technical team)**.
{% endhint %}

## [Ottu Tokenization showcase](tokenization.md#ottu-tokenization-showcase)

Merchant creating payment link including [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional) within the request payload.

```json
{
    "type": "payment_request",
    "pg_codes": ["ottu_pg_kwd_tkn"],
    "amount": "19",
    "customer_id":"user_example",
    "order_no": "token_showcase",
    "currency_code": "KWD"
}
```

The generated payment link, will redirect the customer to the payment checkout page, where the customer would enter his card details and check the **Save Your Card** option, then click **Pay Now**.

<figure><img src="../.gitbook/assets/Checkout page (1).png" alt=""><figcaption></figcaption></figure>

Payment completed **successfully**.

<figure><img src="../.gitbook/assets/Payment completed successfully (1).png" alt=""><figcaption></figcaption></figure>

When the merchant creates another payment link with the same [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional), the checkout page at the customer end will be shown as below figure. \
**The customer has two options:**&#x20;

* To enter new card details.
* To use the card saved by the tokenization process, the customer only needs to enter CCV.



<figure><img src="../.gitbook/assets/Saved card (1).png" alt=""><figcaption></figcaption></figure>

