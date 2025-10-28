# Checkout API

Ottu provides a comprehensive collection of APIs that offer a seamless and efficient way to test payments and enable merchants to accept and process transactions instantly. The Checkout API is the cornerstone of any payment initiation, whether it's [API-based](checkout-api.md) or [SDK-based](checkout-sdk/).

## [Best Practices and Guidelines](checkout-api.md#best-practices-and-guidelines)

In order to ensure optimal transaction success tracking and minimize the number of required payment transactions, merchants should [create](checkout-api.md#create-payment-transaction) a Payment Transaction as soon as the amount is known. This typically occurs when a customer adds their first item to their cart. Following this, any changes to the total amount should be updated using the [Checkout API PATCH](checkout-api.md#update) method.

By updating the same payment transaction, rather than creating a new one for each payment attempt, merchants can more effectively trace customer interactions with their cart. This is particularly useful for events such as insufficient funds, where a customer may remove an item from their cart and successfully complete a transaction on their next attempt. Tracking and analyzing such events can help merchants make data-driven decisions for future improvements.

## [Permissions](checkout-api.md#permissions)

Permissions are managed using [Basic Authentication](authentication.md#basic-authentication) and [API-Key](authentication.md#private-key-api-key). \
Specifically, Basic Authentication is used to grant permissions for creating, updating, and reading data, as well as using allowed [PG codes](checkout-api.md#pg_codes-list-required) when [creating ](checkout-api.md#create-payment-transaction)or [updating](checkout-api.md#update) payment transactions.

It is important to ensure that the appropriate level of permissions is assigned to each user or application using the APIs. This can help to prevent unauthorized access or modification of sensitive data. Additionally, it is recommended to rotate API-Keys on a regular basis and to use secure password storage practices when using Basic Authentication.

Ottu Checkout API supports different levels of permissions for the Payment Request and E-Commerce plugins. The permissions depend on the [authentication ](authentication.md)method being used.

### [API Key](checkout-api.md#api-key)

When using the [API-Key](authentication.md#private-key-api-key), all permissions are granted by default, as the API-Key is considered to have admin permissions. See [How to Generate API Keys](../user-guide/configuration/how-to-get-api-keys.md)

### [Basic Authentication](checkout-api.md#basic-authentication)

For [Basic Authentication](authentication.md#basic-authentication), permissions are granted as follows:

#### [Create](checkout-api.md#create)

* To create a transaction, the user needs specific permission depending on the [plugin](../user-guide/plugins/) being used:
  * "**Can add payment requests**" for the [Payment Request](../user-guide/plugins/#payment-request) plugin
  * "**Can add e-commerce payments**" for the [E-Commerce](../user-guide/plugins/#e-commerce) plugin
* Permission to use the payment gateway [code](checkout-api.md#pg_codes-array-required) is also required: "**Can use `pg_code`**"

#### [Update](checkout-api.md#update)

* To update a transaction, the user needs specific permission depending on the plugin being used:
  * "**Can change payment requests**" for the [Payment Request](../user-guide/plugins/#payment-request) plugin
  * "**Can change e-commerce payments**" for the [E-Commerce](../user-guide/plugins/#e-commerce) plugin
* Permission to use the payment gateway [code](checkout-api.md#pg_codes-array-required) is also required: **"Can use `pg_code`**"

{% hint style="info" %}
The PUT operation cannot be used if the user does not have permission to use the previously defined payment gateway [code](checkout-api.md#pg_codes-array-required) on the transaction. For [PATCH](checkout-api.md#update-payment-transaction), updates can be performed as long as the payment gateway codes are not updated.
{% endhint %}

#### [View](checkout-api.md#view)

* By default, if a user has either the "**Can add**" or "**Can change**" permission, they can [fetch](checkout-api.md#retrieve) transactions from the API.
* For more granular control, the following view permissions can be used:
  * "**Can view e-commerce payments**" for the [E-Commerce](../user-guide/plugins/#e-commerce) plugin
  * "**Can view payment requests**" for the [Payment Request](../user-guide/plugins/#payment-request) plugin

## [Create Payment Transaction](checkout-api.md#create-payment-transaction)

The objective of the  POST request is to facilitate the creation of payment transactions and the subsequent generation of payment links, each of which is associated with a unique session ID. These links can be effortlessly shared with customers through a range of communication channels, including email, WhatsApp, and SMS. Additionally, it is possible to incorporate the customer's billing and shipping information into the transaction. Moreover, users of this API have the ability to include diverse forms of data and information within the body request.

### [Request Parameters](checkout-api.md#request-parameters)

#### [agreement](checkout-api.md#agreement-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An established contractual arrangement with the payer, which authorizes the merchant to securely store and subsequently utilize their payment information for specific purposes. This could encompass arrangements like recurring payments for services such as mobile subscriptions, installment-based payments for purchases, arrangements for ad-hoc charges like account top-ups, or for standard industry practices like penalty charges for missed appointments. For more information please refer [here](auto-debit.md#what-is-an-agreement).

<details>

<summary>agreement object details</summary>

#### [id](checkout-api.md#id-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It serves as a unique identifier for the agreement established with the payer. It is integral to link to the specific terms and conditions authorized by the payer for processing their stored card details. The Agreement ID plays a crucial role as an identifier for correlated payments associated with a customer order or invoice, highlighting the need for it to vary based on the correlated payments.

#### [amount\_variability](checkout-api.md#amount_variability-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It defines if the payment amount can vary with each transaction within the agreement.\
Enum: `fixed`, `variable`

#### [start\_date](checkout-api.md#start_date-date-optional) _<mark style="color:blue;">`date`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The date on which the agreement with the payer to process payments begins.

#### [expiry\_date](checkout-api.md#expiry_date-date-optional) _<mark style="color:blue;">`date`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The final date until which the agreement remains valid.

#### [max\_amount\_per\_cycle](checkout-api.md#max_amount_per_cycle-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The agreed-upon maximum amount for an individual payment within the series.

#### [cycle\_interval\_days ](checkout-api.md#cycle_interval_days-integer-optional)_<mark style="color:blue;">`integer`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The number of days between each recurring payment

#### [total\_cycles](checkout-api.md#total_cycles-integer-optional) _<mark style="color:blue;">`integer`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The total number of payment cycles within the agreement duration.

#### [frequency ](checkout-api.md#frequency-string-optional)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

This pertains to the frequency of payments within the series as agreed upon with the payer. Such as: `irregular`, `daily`, ⁣`weekly`, `monthly`, `quarterly`, `semi_annually`, `yearly`, and `other`.\
When `type` set as `unscheduled`, `frequency` should set as  `irregular`.

#### [type](checkout-api.md#type-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The type of commercial agreement that the payer has with the merchant, which can fall into categories such as:

* `recurring`  (Default value)
* `unscheduled`&#x20;
* `other`&#x20;
* `event_based`&#x20;
* `installment`

#### [seller](checkout-api.md#seller-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It details about the retailer, if the agreement is for installment payments.\
&#xNAN;**`seller` child object:**

* <mark style="color:blue;">**category\_code**</mark> _<mark style="color:blue;">**`string`**</mark>_\
  A 4-digit code classifying the retailer's business by the type of goods or services it offers.

- <mark style="color:blue;">**short\_name**</mark> _<mark style="color:blue;">**`string`**</mark>_\
  Abbreviation of the retailer's trading name, suitable for payer's statement display.

* <mark style="color:blue;">**names**</mark><mark style="color:blue;">**&#x20;**</mark>_<mark style="color:blue;">**`string`**</mark>_\
  The retailer's trading name.

#### [extra\_params](checkout-api.md#extra_params-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Additional parameters related to the agreement.

</details>

{% hint style="info" %}
It is imperative for the merchant to strictly avoid including these parameters when the payment type is labeled as "`one-off`" The agreement parameter should be sent exclusively when the payment type is designated as "`auto-debit`".
{% endhint %}

#### [**amount** ](checkout-api.md#amount-string-required)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

Represents the total amount of the payment transaction, which includes the cost of the purchased items or services but excludes any additional fees or charges. The number of decimals must correlate with the [currency\_code](checkout-api.md#currency_code-string-required).\
Max length: 24\
Min value: 0.01

#### [attachment](checkout-api.md#attachment-file-optional) _<mark style="color:blue;">**`file`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

Attachments can be included as an optional feature in email notifications sent to the customer regarding their payment. These attachments will also be available for download on the checkout page. The primary purpose of this field is to provide the customer with additional information or documentation related to their purchase. However, it's important to note the following:

* Attachments should be sent using the [multipart/form-data](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects) encoding type. Ensure that you change the content type to `multipart/form-data` when sending attachments. They cannot be sent using [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) encoding.
* Allowed file extensions for attachments include: PDF, JPEG, PNG, DOC, DOCX, JPG, XLS, XLSX, and TXT.
* The name of the attached file should not exceed 100 characters.

#### [**billing\_address**](checkout-api.md#billing_address-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An object to save customer registered address data into payment transaction.

<details>

<summary><em>billing_address object details</em></summary>

#### :digit\_one:[ **line1** ](checkout-api.md#line1-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

One of the billing address parameters and should be filled by street & house data.\
Max length: 128.

#### :digit\_two:[ **line2** ](checkout-api.md#line2-string-optional)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

For accuracy purpose, Additional address data for the [line1](checkout-api.md#line1-string-required).\
Max length: 128.

#### :digit\_three:[city](checkout-api.md#city-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

The city where the customer is living and registered.\
Max length: 40.

#### :digit\_four: [**state** ](checkout-api.md#state-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

State of the customer’s city (sometimes the same as the [city](checkout-api.md#city-string-required)).\
Max length: 40.

#### :digit\_five:[ **country** ](checkout-api.md#country-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Customer’s country, [ISO 3166-1 Alpha-2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).\
Will be validated against existing countries.\
Max length: 2.

#### :digit\_six:[ **postal\_code**](checkout-api.md#postal_code-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_&#x20;

Postal code (maybe has different length for different countries).\
Max length: 12.

</details>

#### [card\_acceptance\_criteria](checkout-api.md#card_acceptance_criteria-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

This field allows the merchant to define specific rules and conditions that a card must meet to be eligible for payment. These stipulations apply regardless of whether a customer chooses to pay using a saved card or opts to add a new card for the transaction. By leveraging the `card_acceptance_criteria`, merchant gains the power to fine-tune his payment processing strategy, tailoring acceptance rules to align with his business needs, security standards, and risk management policies.

**Example**: If merchant runs an exclusive service that caters predominantly to premium customers, he might set criteria that only allow transactions from high-tier credit cards like VISA Platinum. This ensures that payments align with the exclusivity and branding of his services. Merchant should configure these criteria thoughtfully. Striking the right balance between security, risk mitigation, and user experience is paramount.

{% hint style="info" %}
The card\_acceptance\_criteria field is applicable only for direct payments and not for hosted session payments.
{% endhint %}

<details>

<summary>card_acceptance_criteria <em>object details</em></summary>

#### [min\_expiry\_time](checkout-api.md#min_expiry_time-string-optional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`optional`</mark>_

Specifies the minimum required validity period, in days, for a card to be eligible for payment. If set to 30 days, for example, cards set to expire within the next month would be declined. This ensures short-lived cards nearing their expiration date are filtered out, reducing chances of payment failures. When implementing, balance merchant's operational needs with customer convenience. Setting it too stringent might increase payment declines, while a lenient approach could risk future payment failures.

Additionally, it defaults to 30 days when the `payment_type` is `auto_debit`. If any other payment type is selected, no default value is set.

</details>

#### [**currency\_code**](checkout-api.md#currency_code-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The currency in which the transaction is denominated. However, it does not guarantee that the payment must be made in this currency, as there can be currency conversions or [exchanges ](../user-guide/currencies.md#currency-exchanges)resulting in a different currency being charged.\
See [currencies](../user-guide/currencies.md).\
3 letters code.

#### [customer\_birthdate ](checkout-api.md#customer_birthdate)

_<mark style="color:blue;">`string datetime`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Specifies the customer's date of birth. This field can be used for various purposes such as identity verification, eligibility checks, or customer profiling. The value must be provided in the `YYYY-MM-DD` format.

#### [**customer\_email**](checkout-api.md#customer_email-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:orange;">`conditional`</mark>_

This field specifies the customer's email address and is used to send payment notifications and receipts. Additionally, it is used for fraud prevention and is transmitted to the payment gateway. The email address may also be included on invoices, receipts, and displayed on the payment page. It must be a valid email address.\
Max length 128

{% hint style="info" %}
* If Email [notifications](checkout-api.md#notifications-object-optional) are applied, the [`notification.email`](checkout-api.md#notifications-object-details) parameter must be included, making `customer_email parameter`<mark style="color:red;">**`required`**</mark>. **else will be&#x20;**_<mark style="color:blue;">**`optional`**</mark>_.
{% endhint %}

#### [**customer\_first\_name** ](checkout-api.md#customer_first_name-string-optional)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The first name of the recipient of the payment. This field is used for various communications such as the invoice, receipt, email, SMS, or displayed on the payment page. It may also be sent to the payment gateway if necessary.\
Max length 64.

#### [**customer\_id**](checkout-api.md#customer_id-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_&#x20;

The customer ID is a unique identifier for the customer within the merchant's system. It is also used as a merchant identifier for the customer and plays a critical role in tokenization. All the customer's cards will be associated with this ID.\
Max length: 64.

#### [customer\_last\_name](checkout-api.md#customer_last_name-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The last name of the recipient of the payment. This field is used for various communications such as the invoice, receipt, email, SMS, or displayed on the payment page. It may also be sent to the payment gateway if necessary.\
Max length 64.

#### [customer\_phone](checkout-api.md#customer_phone-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:orange;">`conditional`</mark>_

Customer phone number associated with the payment. This might be sent to the payment gateway and depending on the gateway, it may trigger a click to pay process on the payment page. The phone number will also be included in the invoice, receipt, email, and displayed on the payment page.\
Max length 16.

{% hint style="info" %}
**It becomes a&#x20;**<mark style="color:red;">**required**</mark>**&#x20;parameter:**

* If the merchant wants to enable KFAST on KNET. **KFAST** is a tokenization feature on KPay page, which works with UDF3 mapped with `customer_phone`.
* If SMS [notifications](checkout-api.md#notifications-object-optional) are enabled, the [notification.sms](checkout-api.md#notifications-object-details) parameter must be included, making the `customer_phone` parameter <mark style="color:red;">**required**</mark>.&#x20;

**Otherwise, it remains&#x20;**<mark style="color:blue;">**optional**</mark>**&#x20;parameter.**
{% endhint %}

#### [due\_datetime](checkout-api.md#due_datetime-string-date-time-optional) _<mark style="color:blue;">`string datetime`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The date and time by which the payment is due. This field may be used to help remind the customer to complete the payment before the due date The default value is UTC.\
Should be in format (DD/MM/YYYY hh:mm)

#### [**email\_recipients**](checkout-api.md#email_recipients-array-of-strings-optional) _<mark style="color:blue;">**`array of strings`**</mark>_ _<mark style="color:blue;">`optional`</mark>_

A comma-separated list of email addresses for internal recipients who will receive customer emails. These recipients will be included in email notifications sent to the customer regarding their payment.

#### [**expiration\_time**](checkout-api.md#expiration_time-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

If defined, any payment transactions created more than the defined period of time ago will be invalidated or expired if the customer tries to pay them. This field may be used to help ensure that payment transactions are processed in a timely manner.\
By default, this expiration period is set to 24 hours from the time of transaction creation.\
Should be In format (DD HH:MM:SS).

{% hint style="info" %}
In order to automatically change the state to **expired**, **Expire Payment Transactions**? Field should be enabled.

From Ottu dashboard > administration panel > config > configuration page, then enable field **Expire Payment Transactions**? Otherwise, the transaction will be marked as expired when the customer attempts to pay past the expiration time.
{% endhint %}

{% hint style="info" %}
If the [expiration \_time](checkout-api.md#expiration_time-string-optional) for a payment has passed, the payment state will be changed and cannot be paid. However, if the payment [due\_datetime](checkout-api.md#due_datetime-string-date-time-optional) has passed, the payment can still be paid, but the customer may incur fees or penalties. The state of the payment may not change in this case, but the customer's account may be impacted by the late payment.
{% endhint %}

#### [**extra**](checkout-api.md#extra-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An object for extra data aka dynamic fields. Extra data can accept any value by default. However, if the merchant wants to enforce a specific type, they can use the plugins.Field class to do so. \
All CUSTOM fields are validated inside extra field.

#### [generate\_qr\_code](checkout-api.md#generate_qr_code-bool-optional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`optional`</mark>_

If set to true, the [qr\_code\_url](checkout-api.md#qr_code_url-string-conditional) field will be present in the response. Upon scanning it, the customer will be redirected to the [checkout\_url,](checkout-api.md#checkout_url-string-mandatory) which is the Ottu Checkout page.\
Default value is false.

#### [include\_sdk\_setup\_preload](checkout-api.md#include_sdk_setup_preload-bool-optional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`optional`</mark>_

When set to true, the Checkout API will include the [sdk\_setup\_preload\_payload](checkout-api.md#sdk_setup_preload_payload-object-conditional) in its response. This payload facilitates immediate UI setup without the need for further API calls.&#x20;

By default, the parameter is set to `false`, and the `sdk_setup_preload_payload`  is not included in the API response.

#### [**language**](checkout-api.md#language-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

This field specifies the language to be used for communication with the customer, including the payment page, receipt, invoice, email, SMS, and any other communications related to the payment transaction.\
Available choices: en, ar.\
Default language is en.\
Max length: 2.

#### [**notifications**](checkout-api.md#notifications-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An object that contains the notification settings for this payment transaction, including SMS and email notifications, as well as the events for which they will be sent (e.g., `created`, `paid`, `refund`, `canceled`, etc.). This field may be used to configure and customize the notifications sent to customers and internal recipients throughout the payment process.

<details>

<summary><em>notifications object details</em></summary>

#### :digit\_one: [**email**](checkout-api.md#email-list-optional) _<mark style="color:blue;">**`list`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

**Will be triggered at the following notification events:**\
\[ `created`, `paid`, `canceled`, `failed`, `expired`, `authorized`, `voided`, `refunded`, `captured`]

* If the payment transaction transitions to an **error** state and an email notification has been set up for the **failed** state, then the customer will receive an email.

#### :digit\_two: [sms](checkout-api.md#sms-list-optional) _<mark style="color:blue;">**`list`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

**Will be triggered at the following notification events:**\
\[ `created`, `paid`, `canceled`, `failed`, `expired`, `authorized`, `voided`, `refunded`, `captured`]

* If the payment transaction transitions to an **error** state and an SMS notification has been set up for the **failed** state, then the customer will receive an SMS.

</details>

#### [**order\_no**](checkout-api.md#order_no-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The unique identifier assigned to this payment transaction. It is used for tracking purposes and is set by the merchant or the system.\
Max length: 128.

{% hint style="info" %}
For MPGS transactions, the `order_no` value must not exceed 37 characters.
{% endhint %}

#### [**pg\_codes**](checkout-api.md#pg_codes-array-required) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:red;">`required`</mark>_

The list of payment gateway codes from which a customer can select to perform the payment or authorization. Customer uses only one PG code.\
**For**[ **Basic authentication**](authentication.md#basic-authentication)**:** User can use the PG code that has permission to access to.\
**For** [**API Private key**](authentication.md#private-key-api-key)**:** User can use all the PG code.

#### [payment\_type](checkout-api.md#payment_type-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

**Enum:** `one_off`, `auto_debit`, or `save_card`

* &#x20;`one_off` : For payments that occur only once without future commitments.&#x20;
* `save_card` : To indicates that the transaction is for the purpose of saving the card information. For additional information, please refer to the [Tokenization without Payment](tokenization.md#tokenization-without-payment) document. &#x20;
* &#x20;`auto_debit` : For payments that are automatically deducted, such as recurring subscriptions, installments, or unscheduled auto-debits. for more information about auto-debit API, please refer to [Auto-Debit API documentation](auto-debit.md).

If `auto_debit` is selected:

1. [agreement](checkout-api.md#agreement-object-conditional) and [customer\_id](checkout-api.md#customer_id-string-conditional) parameters will also be mandatory.
2. Only `PG codes` supporting [tokenization](tokenization.md) can be selected. As a side effect, the card used for the payment will be associated with the supplied `agreement.id`. This card will be locked, preventing the customer from deleting it from the system until an alternate card is chosen for `auto-debit` payments.

### [**payment\_instrument**](checkout-api.md#payment_instrument)&#x20;

_<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It provides the essential values required for direct payment processing, including details about the selected payment instrument, the credentials used to authorize the transaction, and the rules that define the payment processing flow.

{% hint style="danger" %}
Do **not** call this API from the browser. When using the `payment_instrument` parameter in the Checkout API, this endpoint must be called **server-to-server** to prevent exposure of sensitive payment data and credentials.
{% endhint %}

{% hint style="warning" %}
In `payment_instrument`, only one `pg_code` is allowed. The selected gateway must support the chosen payment method (Apple Pay, Google Pay, or Auto Debit).
{% endhint %}

<details>

<summary><em>payment_instrument object details</em></summary>

#### :digit\_one:**instrument\_type &#x20;**_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

It specifies the payment method being used. This determines how the `payload` is interpreted and how the payment flow proceeds.

> **Enum:**
>
> * `apple_pay` → Apple Pay (digital wallet)
> * `google_pay` → Google Pay (digital wallet)
> * `token` → Token-based recurring or stored payment

:digit\_two:**payload&#x20;**_<mark style="color:blue;">**`object`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

It contains the payment credentials or token received from the provider. Must be passed **exactly as received**, with no modifications.

**Example:**

*   **`"instrument_type": "apple_pay"`**\
    The `payload` must be a dictionary object containing the complete Apple Pay token from Apple’s Payment Sheet. Always pass it exactly as received, without any modification.

    {% code overflow="wrap" %}
    ```json
    {
      "payment_instrument": {
        "instrument_type": "apple_pay",
        "payload": {
          "version": "EC_v1",
          "data": "encrypted_payment_data_base64",
          "signature": "signature_base64",
          "header": {
            "ephemeralPublicKey": "key_base64",
            "publicKeyHash": "hash_base64",
            "transactionId": "transaction_id"
          }
        }
      }
    }
    ```
    {% endcode %}
*   **`"instrument_type": "google_pay"`** \
    The `payload` must be a dictionary object containing the complete Google Pay payment data, including encrypted payment information and signatures.

    ```json
    {
      "payment_instrument": {
        "instrument_type": "google_pay",
        "payload": {
          "signature": "MEUCIQCbtFh7UIe1Bpi7...",
          "intermediateSigningKey": {
            "signedKey": "{\"keyValue\":\"MFkwEw...\"}",
            "signatures": ["MEQCIH0G6CYKU..."]
          },
          "protocolVersion": "ECv2",
          "signedMessage": "{\"encryptedMessage\":\"...\"}"
        }
      }
    }
    ```
*   **`"instrument_type": "token"`** \
    The `payload` must be a dictionary. It contains the saved card toke.

    ```json
    {
        "payment_instrument": {
            "instrument_type": "token"
            "payload": {
                "token": "*********"
            },

        }
    }
    ```

</details>

#### [**product\_type**](checkout-api.md#product_type-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The type of product or service being purchased. This field may be used for tracking and reporting purposes\
Max length: 128.

#### [**redirect\_url**](checkout-api.md#redirect_url-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

It is the destination where customers are redirected after the payment stage, only if the `webhook_url` returns a success status. Query parameters, including [order\_no](checkout-api.md#order_no-string-optional), [reference\_number](webhooks/payment-notification.md#reference_number-string-mandatory), [session\_id](checkout-api.md#session_id-string-mandatory), and any [extra](checkout-api.md#extra-object-optional) parameters, will be added to the `redirect_url`. \
For more information on how redirection works, please check [here](webhooks/payment-notification.md#redirectional-diagram).

#### [shipping\_address](checkout-api.md#shipping_address-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An object to save address data into payment transaction

<details>

<summary><em>shipping_address object details</em></summary>

#### :digit\_one:[ **line1** ](checkout-api.md#line1-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Location details where the shipment should be delivered to.\
Should be filled by street & house data.\
Max length 128.

#### :digit\_two:[ **line2** ](checkout-api.md#line2-string-optional)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

For accuracy purpose, Additional address data for the [line1](checkout-api.md#line1-string-required-1).\
Max length 128.

#### :digit\_three:[city](checkout-api.md#city-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

The city where the shipment should be delivered to.\
Max length 40.

#### :digit\_four: [**state** ](checkout-api.md#state-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

The city where the shipment should be delivered to. (sometimes the same as the [city](checkout-api.md#city-string-required-1)).\
Max length 40.

#### :digit\_five:[ **country** ](checkout-api.md#country-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Destination country, [ISO 3166-1 Alpha-2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).\
Will be validated against existing countries.\
Max length: 2.

#### :digit\_six: [**postal\_code**](checkout-api.md#postal_code-string-required-1) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_&#x20;

Postal code (maybe has different length for different countries).\
Max length: 12.

#### :digit\_seven: [first\_name](checkout-api.md#first_name-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_&#x20;

Shipment recipient first name.\
Max length: 64.

#### :digit\_eight: [last\_name](checkout-api.md#last_name-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Shipment recipient last name.\
Max length: 64.

#### :digit\_nine: [email ](checkout-api.md#email-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Shipment recipient email address.\
Max length: 128.

#### :digit\_one::digit\_zero: [phone](checkout-api.md#phone-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Shipment recipient phone number.\
Max length: 16.

</details>

#### [s**hortify\_attachment\_url** ](checkout-api.md#shortify_attachment_url-bool-optional)<mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short attachment retrieval URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as [Bitly](https://bitly.com/), is [configured](../user-guide/configuration/url-shortener-configuration.md), the [attachment\_short\_url](checkout-api.md#attachment_short_url-string-conditional) will be shorter than attachment response parameter. if not configured, the attachment\_short\_url will be in the same format with [attachment](checkout-api.md#attachment-string-conditional) response parameter.\
Default value is false.

#### [shortify\_checkout\_url](checkout-api.md#shortify_checkout_url-bool-optional) <mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short checkout retrieval URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as [Bitly](https://bitly.com/), is [configured](../user-guide/configuration/url-shortener-configuration.md), the [checkout\_short\_url](checkout-api.md#checkout_short_url-string-conditional) will be shorter than [checkout\_url](checkout-api.md#checkout_url-string-mandatory) parameter. If not configured, the checkout\_short\_url will be in the format of "https://\<ottu-url>>/b/abc123".\
Default value is false.

#### [**type**](checkout-api.md#type-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

The type of the payment transaction. This field represents the purpose of the payment and can be one of several pre-defined choices. Available choices: [payment\_request](../user-guide/plugins/#payment-request), [e\_commerce](../user-guide/plugins/#e-commerce).\
Max length: 24.

#### [attachment\_upload\_url](checkout-api.md#attachment_upload_url-string-optional)  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

A writable field used to reference an attachment that has already been uploaded to the server.

#### [**vendor\_name**](checkout-api.md#vendor_name-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The name of the vendor or merchant associated with this payment. This field may be used for accounting and reporting purposes.\
Max length: 64.

#### [**webhook\_url**](checkout-api.md#webhook_url-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

In case of a payment event or payment operation, Ottu triggers an HTTP request to this URL, to disclose transactional data.\
It should be provided by merchant.\
See Webhook [Payment Notification](webhooks/payment-notification.md).

### [Response Parameters](checkout-api.md#response-parameters)

These parameters will be returned for all the response status.

#### [agreement](checkout-api.md#agreement-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It denotes a pre-arranged contractual agreement with the paying customer, enabling the secure retention and future use of their payment details for specific purposes. These agreements encompass various payment arrangements, including recurring service payments like mobile subscriptions, installment payments for purchases, one-time charges such as account reloads, or compliance with industry practices like penalty fees for missed appointments.\
See the request parameter [agreement](checkout-api.md#agreement-object-optional) for more information.

**Presence condition:**

* This parameter should be included when the [payment\_type](checkout-api.md#payment_type-string-mandatory) is set to `auto_debit` On the other hand, it must not be sent when the [payment\_type](checkout-api.md#payment_type-string-optional) is designated as `one_off` Importantly, this isn't restricted to just the initial transaction but should be consistently present in all following transactions associated with the "`auto_debit`" payment type.

{% hint style="info" %}
In certain agreement types, the condition state becomes a required element. For further details on which parameters are mandatory for recurring agreements, please refer [here](auto-debit.md#importance-for-merchants).
{% endhint %}

#### [**amount**](checkout-api.md#amount-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Payment transaction total amount. The merchant should always check if the amount he receives from Ottu is the same amount of the order, to avoid user changing the cart amount in between.\
See the request parameter [amount](checkout-api.md#amount-string-required) for more information.

#### [**attachment**](checkout-api.md#attachment-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Attachment retrieval URL. \
See the request parameter [attachment](checkout-api.md#attachment-stringoptional) for more information.

**Presence condition:**

* The attachment should be uploaded using [attachment](checkout-api.md#attachment-stringoptional) request parameter.

#### [**attachment\_short\_url**](checkout-api.md#attachment_short_url-string-conditional) <mark style="color:blue;">**`string`**</mark> _<mark style="color:blue;">`conditional`</mark>_

A short attachment retrieval URL.\
Max length: 200.

**Presence condition:**

* [shortify\_attachment\_url](checkout-api.md#shortify_attachment_url-bool-optional) request parameter should be "true" in order to generate it.

#### [**billing\_address**](checkout-api.md#billing_address-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s registered address data.\
See the request parameter [billing\_address](checkout-api.md#billing_address-object-optional) for more information.

**Presence condition:**

* Any child parameter provided with the [billing\_address](checkout-api.md#billing_address-object-optional) object in the request payload will be populated in the response as [billing\_address](checkout-api.md#billing_address-object-conditional) child parameter.

#### [card\_acceptance\_criteria](checkout-api.md#card_acceptance_criteria-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It outlines the rules for a card's payment eligibility\
See the request parameter [card\_acceptance\_criteria](checkout-api.md#card_acceptance_criteria-object-optional) for more information.

**Presence condition:**

* Any child parameter provided with the [card\_acceptance\_criteria](checkout-api.md#card_acceptance_criteria-object-optional) object in the request payload will be populated in the response as [card\_acceptance\_criteria](checkout-api.md#card_acceptance_criteria-object-optional) child parameter.

#### [**checkout\_short\_url**](checkout-api.md#checkout_short_url-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Short [checkout url.](checkout-api.md#checkout_url-string-mandatory)

**Presence condition:**

* [shortify\_checkout\_url](checkout-api.md#shortify_checkout_url-bool-optional) request parameter should be set to "true" in order to generate it.

#### [checkout\_url](checkout-api.md#checkout_url-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

URL that directs the customer to the Ottu Checkout Page where they can see the payment details and available payment methods for the transaction. If you need to share the payment link over SMS or WhatsApp, use [checkout\_short\_url](checkout-api.md#checkout_short_url-string-conditional) instead.

#### [currency\_code](checkout-api.md#currency_code-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The code of the currency used in the transaction.\
See the request parameter [currency\_code](checkout-api.md#currency_code-string-required) for more information.

#### [customer\_birthdate ](checkout-api.md#customer_birthdate-1)

_<mark style="color:blue;">`string datetime`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Customer's date of birth. For more information, please refer to [customer\_birthdate](checkout-api.md#customer_birthdate).

**Presence condition:**

* [customer\_birthdate](checkout-api.md#customer_birthdate) request parameter should be provided.

#### [customer\_email](checkout-api.md#customer_email-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s email address.\
See the request parameter [customer\_email ](checkout-api.md#customer_email-string-optional)for more information.

**Presence condition:**

* [customer\_email](checkout-api.md#customer_email-string-optional) request parameter should be provided.

#### [customer\_first\_name](checkout-api.md#customer_first_name-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s first name.\
See the request parameter [customer\_first\_name](checkout-api.md#customer_first_name-string-optional) for more information.

**Presence condition:**

* [customer\_first\_name](checkout-api.md#customer_first_name-string-optional) request parameter should be provided.

#### [customer\_id](checkout-api.md#customer_id-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It is a unique identifier assigned to a customer. This identifier can be used to distinguish one customer from another and can be utilized for tracking purposes or to retrieve specific customer information from the API.\
See the request parameter [customer\_id](checkout-api.md#customer_id-string-optional) for more information.

**Presence condition:**

* [customer\_id](checkout-api.md#customer_id-string-optional) request parameter should be provided.

#### [customer\_last\_name](checkout-api.md#customer_last_name-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s last name.\
See the request parameter [customer\_last\_name](checkout-api.md#customer_last_name-string-optional) for more information.

**Presence condition:**

* [customer\_last\_name](checkout-api.md#customer_last_name-string-optional) request parameter should be provided.

#### [custome\_phone](checkout-api.md#custome_phone-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`conditional`**</mark>_

Customer's phone number.\
See the request parameter [customer\_phone](checkout-api.md#customer_phone-string-optional) for more information.

**Presence condition:**

* [customer\_phone](checkout-api.md#customer_phone-string-optional) request parameter should be provided.

#### [due\_datetime](checkout-api.md#due_datetime-string-date-time-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`date-time`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It specifies the deadline for payment. It has no effect on changing the transaction state, and the transaction can be paid even after due\_datetime.\
See the request parameter [due\_datetime](checkout-api.md#due_datetime-string-date-time-optional) for more information.

#### [email\_recipients](checkout-api.md#email_recipients-array-of-strings-conditional) _<mark style="color:blue;">`array of strings`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

This is a list of internal email recipients, who will receive notifications sent to the customer about their payment.\
See the request parameter [email\_recipients](checkout-api.md#email_recipients-array-of-strings-optional) for more information.

**Presence condition:**

* [email\_recipients](checkout-api.md#email_recipients-array-of-strings-optional) should be provided in the request payload.

#### [**expiration\_time**](checkout-api.md#expiration_time-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It refers to the specific point in time after which the transaction cannot be paid anymore, and its state changes accordingly.\
See the request parameter [expiration\_time](checkout-api.md#expiration_time-string-optional) for more information.

#### [extra](checkout-api.md#extra-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents additional data fields that can be dynamically added to the response using the extra request parameter.\
See the request parameter [extra](checkout-api.md#extra-object-optional) for more information.

**Presence condition:**

* Any child parameter provided with the [extra](checkout-api.md#extra-object-optional) object in the request payload will be populated in the response as [extra](checkout-api.md#extra-object-conditional) object child parameter.

#### [initiator\_id](checkout-api.md#initiator_id-integer-initiator-conditional) _<mark style="color:blue;">`integer(initiator)`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

The user who initiated the creation of this payment transaction, if available. This field is optional and may be used to track who created the payment transaction\
Max length: 11.

**Presence condition:**

* It is present only when [Basic Authentication](authentication.md#basic-authentication) is used, because [API-Key](authentication.md#private-key-api-key) Authentication is not associated with any user.

#### [**language**](checkout-api.md#language-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It represents the language code that is utilized for all communication related to payment transactions with the customer, including payment page, receipt, invoice, email, and SMS\
For more details check the request parameter [language](checkout-api.md#language-string-optional)

#### [notifications](checkout-api.md#notifications-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents the notification settings for this payment transaction which have been received and processed.\
See the request [notifications](checkout-api.md#notifications-object-optional) for more information.

**Presence condition:**

* Any child parameter provided with the [notifications](checkout-api.md#notifications-object-optional) object in the request payload will be populated in the response as [notifications](checkout-api.md#notifications-object-conditional) child parameter.

#### [**operation**](checkout-api.md#operation-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Specifies the type of operation to be performed by the payment gateway. If set to 'purchase', the payment source will be directly charged. If set to 'authorize', the payment source will only be authorized and the actual charge will be made at a later time\
Max length: 16.

#### [order\_no](checkout-api.md#order_no-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It is a unique identifier assigned to the payment transaction, which is primarily used for tracking purposes. The identifier can be set by the merchant or the system.\
See the request parameter [order\_no](checkout-api.md#order_no-string-optional) for information.

**Presence condition:**

* [order\_no](checkout-api.md#order_no-string-optional) request parameter should be included in the request payload.

#### [**payment\_methods**](checkout-api.md#payment_methods-array-object-mandatory) _<mark style="color:blue;">`array [object]`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

An array containing all the payment methods derived from the [pg\_codes](checkout-api.md#pg_codes-array-required) input. Each object in the array contains information about a single payment gateway, including its icon and the [redirect\_url](checkout-api.md#redirect_url-string-optional) that will redirect the customer to the payment gateway's payment page

<details>

<summary>payment_methods <em>object details</em></summary>

#### :digit\_one: [**code** ](checkout-api.md#code-string)_<mark style="color:blue;">**`string`**</mark>_

Code of the Gateway Settings instance

#### :digit\_two:[ **name** ](checkout-api.md#name-string)_<mark style="color:blue;">`string`</mark>_&#x20;

Name of the Gateway Settings instance.

#### :digit\_three:[ **pg** ](checkout-api.md#pg-string)_<mark style="color:blue;">`string`</mark>_&#x20;

Name of the gateway, settings are applied to.

#### :digit\_four:[ **is\_sandbox** ](checkout-api.md#is_sandbox-bool)_<mark style="color:blue;">`bool`</mark>_&#x20;

It is environment used for this PG settings or not.

#### :digit\_five: [**icon**](checkout-api.md#icon-string) _<mark style="color:blue;">`string`</mark>_&#x20;

URL to default icon of the current gateway.

#### :digit\_six:[ **flow** ](checkout-api.md#flow-string)_<mark style="color:blue;">`string`</mark>_&#x20;

Choice from (“redirect”, ...).

#### :digit\_seven: [**redirect\_url**](checkout-api.md#redirect_url-string) _<mark style="color:blue;">`string`</mark>_&#x20;

This URL redirects to the payment page.See [redirect\_url](checkout-api.md#redirect_url-string-optional)

</details>

#### [pg\_codes](checkout-api.md#pg_codes-array-mandatory) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The options of the payment gateway codes included in the request payload to enable customers to make payments.\
See the request parameter [pg\_codes](checkout-api.md#pg_codes-array-required) for more information.

#### [payment\_type ](checkout-api.md#payment_type-string-mandatory)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It presents options such as `one_off` for one-time payments without future obligations and `auto_debit` for automated deductions, encompassing recurring subscriptions, installment payments, or unscheduled debits. For further details on the `Auto Debit API` and payment\_type please refer to [Auto-Debit API](auto-debit.md).\
Default value: "`one_off`".\
See the request parameter [payment\_type](checkout-api.md#payment_type-string-optional) for more information.

#### [product\_type](checkout-api.md#product_type-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

The nature of the purchased product or service, which can be employed for the purpose of keeping track and generating reports.\
See the request parameter [product\_type](checkout-api.md#product_type-string-optional) for more information.

**Presence condition:**

* &#x20;[product\_type](checkout-api.md#product_type-string-optional) request parameter should be included in the request payload.

#### [qr\_code\_url](checkout-api.md#qr_code_url-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

A QR code that, when scanned, redirects to the checkout page for this payment. This QR code may be displayed on invoices, receipts, or other documents to allow customers to easily access the checkout page and make a payment.&#x20;

**Presence condition:**

* The request parameter [generate\_qr\_code](checkout-api.md#generate_qr_code-bool-optional) should be set to `true`.

#### [redirect\_url](checkout-api.md#redirect_url-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents the URL where the customer will be redirected after the payment stage is complete.\
See the request parameter [redirect\_url](checkout-api.md#redirect_url-string-optional) more information.

**Presence condition:**

* The request parameter [redirect\_url](checkout-api.md#redirect_url-string-optional) should be provided.

#### [sdk\_setup\_preload\_payload](checkout-api.md#sdk_setup_preload_payload-object-conditional)  _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It is a JSON object containing preloaded data necessary for initializing the [Checkout SDK](checkout-sdk/). This data encompasses vital details such as customer information, available payment methods, and specific transaction details, all formatted according to the `Checkout SDK`'s initialization specifications. By offering essential information upfront, this feature streamlines the [checkout process](checkout-sdk/#ottu-checkout-sdk-flow), contributing to an improved user experience and increased efficiency.

{% hint style="info" %}
It must be passed to the `Checkout SDK` constructor without any modifications.
{% endhint %}

**Presence Conditions:**

* It will be included in the response solely when the [include\_sdk\_setup\_preload](checkout-api.md#include_sdk_setup_preload-bool-optional) parameter is set to `true`.

#### [**session\_id**](checkout-api.md#session_id-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

A unique identifier for each payment transaction, used to maintain the session state during the payment process. It can be used to perform subsequent operations, like retrieve, acknowledge, refund, capture, and cancellation.\
Max length: 128.

#### [shipping\_address](checkout-api.md#shipping_address-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Shipping location data of the customer.\
See the request parameter [shipping\_address](checkout-api.md#shipping_address-object-optional) for more information.

**Presence condition:**

* The child objects of the [shipping\_address](checkout-api.md#shipping_address-object-optional) parameter provided in the request object will be populated as child objects of the [shipping\_address](checkout-api.md#shipping_address-object-conditional) parameter in the response object.

#### [**state**](checkout-api.md#state-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The current state of the payment transaction, it helps to understand the progress of the payment.\
Enum: `created` , `pending` , `attempted` , `authorized` , `paid` , `failed` , `canceled` , `expired` , `invalided` ,`refunded` , `cod`.\
See [payment transaction state](../user-guide/payment-tracking/#payment-transaction-state-and-payment-attempt-state) for more information.

#### [type](checkout-api.md#type-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The type of the payment transaction.\
See the request parameter [type ](checkout-api.md#type-string-required)for more information.

#### [attachment\_upload\_url](checkout-api.md#attachment_upload_url-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

A field in the response that contains the URL of an attachment that has already been uploaded to the server.

**Presence condition:**

* The request parameter [attachment\_upload\_url](checkout-api.md#attachment_upload_url-string-optional) should be included in the request payload.

#### [vendor\_name](checkout-api.md#vendor_name-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents the name of the merchant or vendor associated with a payment transaction.\
For more information see [vendor\_name](checkout-api.md#vendor_name-string-optional).

**Presence condition:**

* The request parameter [vendor\_name](checkout-api.md#vendor_name-string-optional) should be included in the request payload.

#### [**webhook\_url**](checkout-api.md#webhook_url-url-conditional)  _<mark style="color:blue;">**`URL`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It contains the URL where the payment result will be sent via a POST request after the customer has completed the payment session. The payment result will be included in the request body.\
See Webhook [Payment Notification](webhooks/payment-notification.md).

**Presence condition:**

* The request parameter [webhook\_url](checkout-api.md#webhook_url-string-optional) should be provided.

### [API Schema Reference ](checkout-api.md#api-schema-reference)

{% openapi-operation spec="october" path="/b/checkout/v1/pymt-txn/" method="post" %}
[OpenAPI october](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/14a4390b9a9c41b51d6db420a030f14c46ab50c37cf102cd4741fe12d0384f86.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20251028%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20251028T135249Z&X-Amz-Expires=172800&X-Amz-Signature=9385155d87f7900e6beabc3342fbc2511ea6396c7e03189ce59ddc1200212d4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

### [Example: Checkout API - create payment transaction (request-response)](checkout-api.md#example-checkout-api-create-payment-transaction-request-response)

#### [**API-Request (default required data)**](checkout-api.md#api-request-default-required-data)

```json
{
    "type": "payment_request",
    "pg_codes": ["pg_codes"],
    "amount": "14",
    "currency_code": "KWD",
    "customer_email":"example@example.com",
    "customer_phone":"customer phone",
    "notifications": {
    "email": ["created", "paid", "canceled", "failed", "expired", "authorized", "voided", "refunded", "captured"],
    "sms": ["created", "paid", "canceled", "failed", "expired", "authorized", "voided", "refunded", "captured"]
        }
}

```

{% hint style="info" %}
"[type](checkout-api.md#type-string-required)", "[pg\_codes](checkout-api.md#pg_codes-array-required)", "[amount](checkout-api.md#amount-string-required)", and  "[currency\_code](checkout-api.md#currency_code-string-required)" are required parameters.\
When we add [notification](checkout-api.md#notifications-object-optional) we should add:\
"[customer\_email](checkout-api.md#customer_email-string-optional)" for email notification.\
"[customer\_phone](checkout-api.md#customer_phone-string-optional)" for SMS notification.
{% endhint %}

#### [**Response**](checkout-api.md#response)&#x20;

```json
{
    "amount": "14.000",
    "checkout_url": "https://<ottu-url>/b/checkout/redirect/start/?session_id=5ca114666d472a170d3c4ea6cadbf347679b2532",
    "currency_code": "KWD",
    "customer_email": "example@example.com",
    "customer_phone": "customer phone",
    "due_datetime": "15/01/2023 11:04:55",
    "expiration_time": "1 00:00:00",
    "language": "en",
    "notifications": {
        "email": [
            "authorized",
            "created",
            "canceled",
            "expired",
            "failed",
            "captured",
            "paid",
            "voided",
            "refunded"
        ],
        "sms": [
            "authorized",
            "created",
            "canceled",
            "expired",
            "failed",
            "captured",
            "paid",
            "voided",
            "refunded"
        ]
    },
    "operation": "authorize",
    "payment_methods": [
        {
            "code": "pg_codes",
            "name": "ottu PG",
            "pg": "Ottu PG",
            "type": "sandbox",
            "amount": "14.000",
            "currency_code": "KWD",
            "fee": "0.000",
            "fee_description": "fix fee",
            "icon": "https://<ottu-url>/static/images/pg_icons/master_visa_mada.png",
            "flow": "redirect",
            "redirect_url": "https://<ottu-url>/checkout/5ca114666d472a170d3c4ea6cadbf347?chd-only=True"
        }
    ],
    "pg_codes": [
        "pg_codes"
    ],
    "session_id": "5ca114666d472a170d3c4ea6cadbf347",
    "state": "created",
    "type": "payment_request"
}

```

## Update Payment

<mark style="color:purple;">`PATCH`</mark> `https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}`

#### Headers

| Name                                            | Type    | Description            |
| ----------------------------------------------- | ------- | ---------------------- |
| Authorization<mark style="color:red;">\*</mark> | API key | Api-Key \{{api\_key\}} |

#### [Update Payment Transaction ](checkout-api.md#update-payment-transaction)

Using a patch function is a good method of increasing trustability whenever any change gets made to the payment transaction, such as updating the amount on the card or removing items from the cart.

### [Parameters](checkout-api.md#parameters)

All the same fields from [create request](checkout-api.md#create-payment-transaction) can be used. The type of update is partial. But some fields can be cross-validated and require other fields to be provided.

## Retrieve Payment

<mark style="color:blue;">`GET`</mark> `https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}`

#### [Retrieve Payment Transaction](checkout-api.md#retrieve-payment-transaction)

To get the information of the payment transaction.

{% hint style="success" %}
**Authentication:** This endpoint is public
{% endhint %}

## [**FAQ**](checkout-api.md#faq)

#### :digit\_one: [**What is the Ottu Checkout API used for?**](checkout-api.md#what-is-the-ottu-checkout-api-used-for)

The **Checkout API** is used to create and manage payment transactions. It enables merchants to generate a **payment session (**[session\_id](checkout-api.md#session_id-string-mandatory)**)**, redirect users to a payment gateway, and receive payment updates via [webhooks](webhooks/).

#### :digit\_two:[**How do I create a payment transaction?**](checkout-api.md#how-do-i-create-a-payment-transaction)

To create a payment transaction, send a [POST request](checkout-api.md#create-payment-transaction) to the Checkout API with the required parameters:

* `amount` – Payment amount
* `currency_code` – Transaction currency
* `pg_codes` – Payment gateways allowed
* `type` - `e_commerce` or `payment_request`

After submission, the API returns a [session\_id](checkout-api.md#session_id-string-mandatory), which is required for processing the transaction.

#### :digit\_three: [**How can I ensure that the payer cannot make a payment after a certain amount of time since redirection?**](checkout-api.md#how-can-i-ensure-that-the-payer-cannot-make-a-payment-after-a-certain-amount-of-time-since-redirecti)

To prevent a payer from completing a payment after a specific period, you need to configure the expiration\_time parameter based on the payment gateway (PG) auto inquiry time. This ensures that even if the user remains on the payment page, they won’t be able to proceed once the expiration time has passed.

The expiration time should be set as:

[expiration\_time](checkout-api.md#expiration_time-string-optional) = [auto inquiry time](checkout-api.md#auto-inquiry-time) + max{[PG auto inquiry time](checkout-api.md#pg-auto-inquiry-time)}

*   #### **Auto inquiry time:**&#x20;

    This is the time Ottu waits before automatically checking the transaction status with the payment gateway. for more information, please refer [here](payment-status-inquiry.md#automatic-inquiry).
*   #### **PG auto inquiry time:**&#x20;

    This is the maximum duration the payment gateway allows before timing out a payment session. More information about PG auto inquiry time is accessible [here](../user-guide/payment-gateway.md#payment-gateway-features-summary).

#### **Example Calculation:**

* For **MPGS (Mastercard Payment Gateway Services)**, the [PG auto inquiry time](checkout-api.md#pg-auto-inquiry-time) is **11 minutes**.
* For **KNET**, the PG auto inquiry time is **8 minutes**.

If your system sets an [auto inquiry time](checkout-api.md#auto-inquiry-time) **of 5 minutes**, then:

* For MPGS: [expiration\_time](checkout-api.md#expiration_time-string-optional) = 5 + 11 = 16 minutes
* For KNET: [expiration\_time](checkout-api.md#expiration_time-string-optional) = 5 + 8 = 13 minutes

This means that after 16 minutes (for MPGS) or 13 minutes (for KNET), the payer will not be able to complete the transaction, even if they are still on the payment page.

#### :digit\_four: [**Can I update an existing transaction?**](checkout-api.md#can-i-update-an-existing-transaction)

Yes! You can update a transaction **before** it is completed by sending a [PUT request](checkout-api.md#update-payment) to the Checkout API, referencing the `session_id`. Updates might include modifying the `amount`, `currency_code`, or allowed `pg_codes`.

#### :digit\_five: [**What happens if a customer abandons the payment?**](checkout-api.md#what-happens-if-a-customer-abandons-the-payment)

If a customer does not complete the payment, the transaction remains in a `pending` state until it expires. Ottu's [automatic inquiry](checkout-api.md#auto-inquiry-time) will check the status based on the configured timing and update the state accordingly.

#### :digit\_six: [**How do I get notified when a payment is completed?**](checkout-api.md#how-do-i-get-notified-when-a-payment-is-completed)

Ottu **sends a** [payment notification ](webhooks/payment-notification.md)webhook to the [webhook\_url](checkout-api.md#webhook_url-string-optional) you provided in the API request. The webhook contains details like:

* `status`: `success`, `failed`, `pending`
* `amount` and `currency_code`
* `pg_response`: Response from the payment gateway

Ensure your server verifies and processes these webhook notifications.

#### :digit\_seven: [**What is the difference between `webhook_url` and `redirect_url`?**](checkout-api.md#what-is-the-difference-between-webhook_url-and-redirect_url)

* **`webhook_url`** – The API sends real-time payment updates it (for backend processing).
* **`redirect_url`** – After payment, the customer is sent to it  (for UI redirection).

For more details on redirection behavior related to `webhook_url`, please refer to this [section.](webhooks/payment-notification.md#redirectional-diagram)

Both are optional but recommended for a seamless experience.

#### :digit\_eight: [**Can I retry a failed payment?**](checkout-api.md#can-i-retry-a-failed-payment)

Yes, if a transaction fails, you can **reuse the same** [session\_id](checkout-api.md#session_id-string-mandatory). the payment state will be `attempt`. for more information about , please refer [here](../user-guide/payment-tracking/payment-transactions-states.md#payment-attempt).

#### :digit\_nine: [**Can I set up recurring payments using the Checkout API?**](checkout-api.md#can-i-set-up-recurring-payments-using-the-checkout-api)

Yes! Use [Auto-Debit](auto-debit.md) with a [payment\_type](checkout-api.md#payment_type-string-optional) set to `auto_debit` to create [agreements](checkout-api.md#agreement-object-optional) for recurring transactions. Learn more in the [Auto-Debit](auto-debit.md).

#### :digit\_one::digit\_zero: [**How do I integrate the Checkout API with the SDK?**](checkout-api.md#how-do-i-integrate-the-checkout-api-with-the-sdk)

The Checkout SDK ([Web](checkout-sdk/web.md), [Android](checkout-sdk/android/), [iOS](checkout-sdk/ios/)) requires a [session\_id](checkout-api.md#session_id-string-mandatory) from the Checkout API. Your backend should:

1. Call the [Checkout API](checkout-api.md) to create a transaction.
2. Pass the `session_id` to the SDK for UI rendering.
3. Handle [webhooks](webhooks/) for transaction updates.
