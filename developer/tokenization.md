# Tokenization

[Getting started](https://app.gitbook.com/s/dCuAiqM42FUw8xdNSjKa/\~/changes/EL1tOkSuLXf06V6iAVra/developer/rest-api/tokenization#getting-started)

With Ottu, you can significantly increase your checkout rate by activating tokenization on MIDs that support this feature. Currently, only [MPGS](test-cards.md#mpgs) is supported, but more options will be available in the future. \
Tokenization is the process of replacing sensitive card data with a unique identifier, called a token. This helps merchants process payments securely without having to be PCI DSS compliant. Let Ottu handle that for you!

{% hint style="info" %}
To get in touch with the Ottu team, KSA merchants can send an email to support@ottu.com.&#x20;
{% endhint %}

## [Implementation](tokenization.md#implementation)

1.  Use the [Checkout API](rest-api/checkout-api.md) to create a [session\_id](rest-api/checkout-api.md#session\_id-string-read-only) with a [pg\_code](rest-api/checkout-api.md#pg\_codes-list-required) associated with a MID that supports and has tokenization enabled. \
    Make sure the [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional) field is present in the request body, as it is essential for allowing customers to save cards. The `customer_id` is the primary way to associate saved cards with a given customer, regardless of whether it's for hosted or direct integration.\




    {% hint style="info" %}
    Depending on payment gateway configuration, CCV could be required by enabling CCV option in the payment gateway configuration **(contact Ottu technical team)**.
    {% endhint %}

    \


    #### [Create a Payment Transaction Using the Below Code](tokenization.md#create-a-payment-transaction-using-the-below-code)

    ```json
    curl --location 'https://sandbox.ottu.net/b/checkout/v1/pymt-txn/' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Api-Key vSUmxsXx.V81oYvOWFMcIywaOu57Utx6VSCmG11lo ' \
    --data-raw 
    {
       "type": "payment_request",
        "pg_codes": ["credit-card"],
        "amount": "1.000",
        "currency_code": "KWD",
        "customer_id": "example",
        "customer_email": "customer@ottu.com"
    }
    ```
2. Use the Checkout SDK to render the payment you just created.

### [Process](tokenization.md#process)

When using the [Checkout SDK](checkout-sdk/) (either web or mobile) and the [session\_id](rest-api/checkout-api.md#session\_id-string-read-only) created via the [Checkout API ](rest-api/checkout-api.md)meets all the conditions (MID is tokenizable, [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional) is present), the save card input will appear, allowing the customer to choose whether to save the card for future purchases or recurring payments. If the customer ticks the checkbox and performs a successful payment, the card will be tokenized and can be used for future payments.

When creating a new session\_id for the same customer and using the same customer\_id in the Checkout SDK, the saved card will appear, displaying only the last 4 digits. The customer can use it to make the payment. The card security code may or may not be required, depending on the acquiring bank's policy.

The 3DS challenge or OTP code is handled out-of-the-box by the Checkout SDK, as Ottu does the heavy lifting for processing payments.

If you wish to perform offline payments or auto debit (i.e., charge the customer when they are not online), please check the Auto Debit documentation section.

{% hint style="info" %}
Ottu supports tokenization on the following platforms:

* [Web](broken-reference)
* Mobile:
  * [Native iOS](checkout-sdk/ios.md)
  * [Native Android](checkout-sdk/android.md)
  * [Flutter](checkout-sdk/flutter.md)
{% endhint %}

## [Ottu Tokenization showcase](https://app.gitbook.com/s/dCuAiqM42FUw8xdNSjKa/\~/changes/EL1tOkSuLXf06V6iAVra/developer/rest-api/tokenization#ottu-tokenization-showcase)

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

The payment link that is generated will direct the customer to the payment checkout page. On this page, the customer will be prompted to enter their card details and check **Save Your Card** checkbox to save their card information. Finally, the customer will need to click on the **Pay Now** button to complete the payment process..

<figure><img src="../.gitbook/assets/Checkout page (1).png" alt=""><figcaption></figcaption></figure>

Payment completed **successfully**.

<figure><img src="../.gitbook/assets/Payment completed successfully (1).png" alt=""><figcaption></figcaption></figure>

If the merchant creates another payment link using the same [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional), the customer will be directed to the checkout page, which is displayed in the figure below. On this page, the customer will have two options:&#x20;

* To enter new card details.
* To use the card saved by the tokenization process, the customer only needs to enter CCV.

<figure><img src="../.gitbook/assets/Saved card (1).png" alt=""><figcaption></figcaption></figure>
