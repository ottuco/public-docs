# Checkout API

## [Getting Started](checkout-api.md#getting-started)

Ottu provides a comprehensive collection of APIs that offer a seamless and efficient way to test payments and enable merchants to accept and process transactions instantly. The Checkout API is the cornerstone of any payment initiation, whether it's [API-based](checkout-api.md) or [SDK-based](checkout-sdk/).

## [Best Practices and Guidelines](checkout-api.md#best-practices-and-guidelines)

In order to ensure optimal transaction success tracking and minimize the number of required payment transactions, merchants should [create](checkout-api.md#create-payment-transaction) a Payment Transaction as soon as the amount is known. This typically occurs when a customer adds their first item to their cart. Following this, any changes to the total amount should be updated using the [Checkout API PATCH](checkout-api.md#update) method.

By updating the same payment transaction, rather than creating a new one for each payment attempt, merchants can more effectively trace customer interactions with their cart. This is particularly useful for events such as insufficient funds, where a customer may remove an item from their cart and successfully complete a transaction on their next attempt. Tracking and analyzing such events can help merchants make data-driven decisions for future improvements.

## [Permissions](checkout-api.md#permissions)

Permissions are managed using [Basic Authentication](authentication.md#basic-authentication) and [API-Key](authentication.md#private-key-api-key). \
Specifically, Basic Authentication is used to grant permissions for creating, updating, and reading data, as well as using allowed [PG codes](checkout-api.md#pg\_codes-list-required) when [creating ](checkout-api.md#create-payment-transaction)or [updating](checkout-api.md#update) payment transactions.

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
* Permission to use the payment gateway [code](checkout-api.md#pg\_codes-array-required) is also required: "**Can use `pg_code`**"

#### [Update](checkout-api.md#update)

* To update a transaction, the user needs specific permission depending on the plugin being used:
  * "**Can change payment requests**" for the [Payment Request](../user-guide/plugins/#payment-request) plugin
  * "**Can change e-commerce payments**" for the [E-Commerce](../user-guide/plugins/#e-commerce) plugin
* Permission to use the payment gateway [code](checkout-api.md#pg\_codes-array-required) is also required: **"Can use `pg_code`**"

{% hint style="info" %}
The PUT operation cannot be used if the user does not have permission to use the previously defined payment gateway [code](checkout-api.md#pg\_codes-array-required) on the transaction. For [PATCH](checkout-api.md#update-payment-transaction), updates can be performed as long as the payment gateway codes are not updated.
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

<summary>agreement child parameters</summary>

#### [id](checkout-api.md#id-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

A unique identifier for the agreement

#### [amount\_variability](checkout-api.md#amount\_variability-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Presents if the payment amount can vary with each transaction.

#### [start\_date](checkout-api.md#start\_date-date-optional) _<mark style="color:blue;">`date`</mark>_ _<mark style="color:blue;">`optional`</mark>_

&#x20;Agreement starting date.

#### [expiry\_date](checkout-api.md#expiry\_date-date-optional) _<mark style="color:blue;">`date`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The final date until which the agreement remains valid.

#### [max\_amount\_per\_cycle](checkout-api.md#max\_amount\_per\_cycle-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The maximum debit amount for one billing cycle.

#### [cycle\_interval\_days ](checkout-api.md#cycle\_interval\_days-integer-optional)_<mark style="color:blue;">`integer`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The number of days between each recurring payment.

#### [total\_cycles](checkout-api.md#total\_cycles-integer-optional) _<mark style="color:blue;">`integer`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The total number of payment cycles within the agreement duration.

#### [frequency ](checkout-api.md#frequency-string-optional)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Represents how often the payment is to be processed.

#### [type](checkout-api.md#type-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

This is event-driven, with "`recurring`" as an example.

#### [seller](checkout-api.md#seller-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Seller information data including:&#x20;

* `"name": "string",`&#x20;
* `"short_name": "string",`
* `"category_code": "string"`

#### [extra\_params](checkout-api.md#extra\_params-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

&#x20;Provides additional information for payment processing. \
It includes the parameter "`payment_processing_day`" which provide information about the day of the month or a specific date when payment processing should occur, offering more control over the timing of payments

</details>

{% hint style="info" %}
If the merchant's sole intention is to store payment details for transactions initiated by the payer in the future, then this parameter group should not be used.
{% endhint %}

#### [**amount** ](checkout-api.md#amount-string-required)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

Represents the total amount of the payment transaction, which includes the cost of the purchased items or services but excludes any additional fees or charges. The number of decimals must correlate with the [currency\_code](checkout-api.md#currency\_code-string-required).\
Max length: 24\
Min value: 0.01

#### [attachment](checkout-api.md#attachment-file-optional) _<mark style="color:blue;">**`file`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

Attachments can be included as an optional feature in email notifications sent to the customer regarding their payment. These attachments will also be available for download on the checkout page. The primary purpose of this field is to provide the customer with additional information or documentation related to their purchase. However, it's important to note the following:

* Attachments should be sent using the [multipart/form-data](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using\_FormData\_Objects) encoding type. Ensure that you change the content type to `multipart/form-data` when sending attachments. They cannot be sent using [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/JSON) encoding.
* Allowed file extensions for attachments are: "pdf", "jpeg", "png", "doc", "docx", "jpg", "xls", "xlsx", and "txt".
* The name of the attached file should not exceed 100 characters.

#### [**billing\_address**](checkout-api.md#billing\_address-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

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

Customer’s country, [ISO 3166-1 Alpha-2 code](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2).\
Will be validated against existing countries.\
Max length: 2.

#### :digit\_six:[ **postal\_code**](checkout-api.md#postal\_code-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_&#x20;

Postal code (maybe has different length for different countries).\
Max length: 12.

</details>

#### [**currency\_code**](checkout-api.md#currency\_code-string-required) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`required`</mark>_

The currency in which the transaction is denominated. However, it does not guarantee that the payment must be made in this currency, as there can be currency conversions or [exchanges ](../user-guide/currencies.md#currency-exchanges)resulting in a different currency being charged.\
See [currencies](../user-guide/currencies.md).\
3 letters code.

#### [**customer\_email**](checkout-api.md#customer\_email-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The email address of the customer that is used to send payment notifications and receipts, and can be used for fraud prevention. This field is mandatory and is always sent to the payment gateway. It may also be included in the invoice, receipt, email, and displayed on the payment page. Have to be a valid email address.\
Max length 128.

#### [**customer\_first\_name** ](checkout-api.md#customer\_first\_name-string-optional)_<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The first name of the recipient of the payment. This field is used for various communications such as the invoice, receipt, email, SMS, or displayed on the payment page. It may also be sent to the payment gateway if necessary.\
Max length 64.

#### [**customer\_id**](checkout-api.md#customer\_id-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_&#x20;

The customer ID is a unique identifier for the customer within the merchant's system. It is also used as a merchant identifier for the customer and plays a critical role in tokenization. All the customer's cards will be associated with this ID.\
Max length: 64.

#### [customer\_last\_name](checkout-api.md#customer\_last\_name-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The last name of the recipient of the payment. This field is used for various communications such as the invoice, receipt, email, SMS, or displayed on the payment page. It may also be sent to the payment gateway if necessary.\
Max length 64.

#### [customer\_phone](checkout-api.md#customer\_phone-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Customer phone number associated with the payment. This might be sent to the payment gateway and depending on the gateway, it may trigger a click to pay process on the payment page. The phone number will also be included in the invoice, receipt, email, and displayed on the payment page.\
Max length 16.

{% hint style="info" %}
If the merchant wants to enable KFAST on KNET, [customer\_phone](checkout-api.md#customer\_phone-string-optional) will be _<mark style="color:red;">**`required`**</mark>_\
**KFAST** is a tokenization feature on KPay page, which works with UDF3 mapped with [customer\_phone](checkout-api.md#customer\_phone-string-optional).
{% endhint %}

#### [due\_datetime](checkout-api.md#due\_datetime-string-date-time-optional) _<mark style="color:blue;">`string date-time`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The date and time by which the payment is due. This field may be used to help remind the customer to complete the payment before the due date The default value is UTC.\
Should be in format (DD/MM/YYYY hh:mm)

#### [**email\_recipients**](checkout-api.md#email\_recipients-array-of-strings-optional) _<mark style="color:blue;">**`array of strings`**</mark>_ _<mark style="color:blue;">`optional`</mark>_

A comma-separated list of email addresses for internal recipients who will receive customer emails. These recipients will be included in email notifications sent to the customer regarding their payment.

#### [**expiration\_time**](checkout-api.md#expiration\_time-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

If defined, any payment transactions created more than the defined period of time ago will be invalidated or expired if the customer tries to pay them. This field may be used to help ensure that payment transactions are processed in a timely manner.\
By default, this expiration period is set to 24 hours from the time of transaction creation.\
Should be In format (DD HH:MM:SS).

{% hint style="info" %}
In order to automatically change the state to **expired**, **Expire Payment Transactions**? Field should be enabled.

From Ottu dashboard > administration panel > config > configuration page, then enable field **Expire Payment Transactions**? Otherwise, the transaction will be marked as expired when the customer attempts to pay past the expiration time.
{% endhint %}

{% hint style="info" %}
If the [expiration \_time](checkout-api.md#expiration\_time-string-optional) for a payment has passed, the payment state will be changed and cannot be paid. However, if the payment [due\_datetime](checkout-api.md#due\_datetime-string-date-time-optional) has passed, the payment can still be paid, but the customer may incur fees or penalties. The state of the payment may not change in this case, but the customer's account may be impacted by the late payment.
{% endhint %}

#### [**extra**](checkout-api.md#extra-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An object for extra data aka dynamic fields. Extra data can accept any value by default. However, if the merchant wants to enforce a specific type, they can use the plugins.Field class to do so. \
All CUSTOM fields are validated inside extra field.

#### [generate\_qr\_code](checkout-api.md#generate\_qr\_code-bool-optional) _<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`optional`</mark>_

If set to true, the [qr\_code\_url](checkout-api.md#qr\_code\_url-string-conditional) field will be present in the response. Upon scanning it, the customer will be redirected to the [checkout\_url,](checkout-api.md#checkout\_url-string-mandatory) which is the Ottu Checkout page.\
Default value is false.

#### [**language**](checkout-api.md#language-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

This field specifies the language to be used for communication with the customer, including the payment page, receipt, invoice, email, SMS, and any other communications related to the payment transaction.\
Available choices: en, ar.\
Default language is en.\
Max length: 2.

#### [**mode**](checkout-api.md#mode-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

Default: “payment”.\
Value: “payment”.

#### [**notifications**](checkout-api.md#notifications-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

An object that contains the notification settings for this payment transaction, including SMS and email notifications, as well as the events for which they will be sent (e.g., 'created', 'paid', 'refund', 'canceled', etc.). This field may be used to configure and customize the notifications sent to customers and internal recipients throughout the payment process.

<details>

<summary><em>notifications object details</em></summary>

#### :digit\_one:[ **email** ](checkout-api.md#email-list-optional)_<mark style="color:blue;">**`list`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

**Will be triggered at the following notification events:**\
\[“created”, "paid", "canceled", "failed", "expired", "authorized", "voided", "refunded", "captured"]

* If the payment transaction transitions to an **error** state and an email notification has been set up for the **failed** state, then the customer will receive an email.

#### :digit\_two:[ **SMS**](checkout-api.md#sms-list-optional) _<mark style="color:blue;">**`list`**</mark>_ _<mark style="color:blue;">**`optional`**</mark>_

**Will be triggered at the following notification events:**\
\[“created”, "paid", "canceled", "failed", "expired", "authorized", "voided", "refunded", "captured"]

* If the payment transaction transitions to an **error** state and an SMS notification has been set up for the **failed** state, then the customer will receive an SMS.

</details>

#### [**order\_no**](checkout-api.md#order\_no-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The unique identifier assigned to this payment transaction. It is used for tracking purposes and is set by the merchant or the system.\
Max length: 128.

#### [**pg\_codes**](checkout-api.md#pg\_codes-array-required) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:red;">`required`</mark>_

The list of payment gateway codes from which a customer can select to perform the payment or authorization. Customer uses only one PG code.\
**For**[ **Basic authentication**](authentication.md#basic-authentication)**:** User can use the PG code that has permission to access to.\
**For** [**API Private key**](authentication.md#private-key-api-key)**:** User can use all the PG code.

#### [**product\_type**](checkout-api.md#product\_type-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The type of product or service being purchased. This field may be used for tracking and reporting purposes\
Max length: 128.

#### [**redirect\_url**](checkout-api.md#redirect\_url-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The URL where the customer will be redirected after the payment stage only if the [webhook\_url](checkout-api.md#webhook\_url-string-optional) returns a success status. [order\_no](checkout-api.md#order\_no-string-optional), [reference\_number](webhooks/payment-webhooks.md#reference\_number-string-mandatory) and [session\_id](checkout-api.md#session\_id-string-mandatory) will be appended to the redirect\_url as query parameters. Check [how redirection works](webhooks/payment-webhooks.md#redirect-behavior-based-on-webhook\_url-response).\
redirect\_url can be set in the administration panel.\
Max length: 200.

#### [shipping\_address](checkout-api.md#shipping\_address-object-optional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`optional`</mark>_

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

Destination country, [ISO 3166-1 Alpha-2 code](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2).\
Will be validated against existing countries.\
Max length: 2.

#### :digit\_six: [**postal\_code**](checkout-api.md#postal\_code-string-required-1) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_&#x20;

Postal code (maybe has different length for different countries).\
Max length: 12.

#### :digit\_seven: [first\_name](checkout-api.md#first\_name-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_&#x20;

Shipment recipient first name.\
Max length: 64.

#### :digit\_eight: [last\_name](checkout-api.md#last\_name-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Shipment recipient last name.\
Max length: 64.

#### :digit\_nine: [email ](checkout-api.md#email-string-required)_<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Shipment recipient email address.\
Max length: 128.

#### :digit\_one::digit\_zero: [phone](checkout-api.md#phone-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

Shipment recipient phone number.\
Max length: 16.

</details>

#### [s**hortify\_attachment\_url** ](checkout-api.md#shortify\_attachment\_url-bool-optional)<mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short attachment retrieval URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as [Bitly](https://bitly.com/), is [configured](../user-guide/configuration/url-shortener-configuration.md), the [attachment\_short\_url](checkout-api.md#attachment\_short\_url-string-conditional) will be shorter than attachment response parameter. if not configured, the attachment\_short\_url will be in the same format with [attachment](checkout-api.md#attachment-string-conditional) response parameter.\
Default value is false.

#### [shortify\_checkout\_url](checkout-api.md#shortify\_checkout\_url-bool-optional) <mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short checkout retrieval URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as [Bitly](https://bitly.com/), is [configured](../user-guide/configuration/url-shortener-configuration.md), the [checkout\_short\_url](checkout-api.md#checkout\_short\_url-string-conditional) will be shorter than [checkout\_url](checkout-api.md#checkout\_url-string-mandatory) parameter. If not configured, the checkout\_short\_url will be in the format of "https://\<ottu-url>>/b/abc123".\
Default value is false.

#### [**type**](checkout-api.md#type-string-required) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:red;">**`required`**</mark>_

The type of the payment transaction. This field represents the purpose of the payment and can be one of several pre-defined choices. Available choices: [payment\_request](../user-guide/plugins/#payment-request), [e\_commerce](../user-guide/plugins/#e-commerce).\
Max length: 24.

#### [**vendor\_name**](checkout-api.md#vendor\_name-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

The name of the vendor or merchant associated with this payment. This field may be used for accounting and reporting purposes.\
Max length: 64.

#### [**webhook\_url**](checkout-api.md#webhook\_url-string-optional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`optional`</mark>_

In case of a payment event or payment operation, Ottu triggers an HTTP request to this URL, to disclose transactional data.\
It should be provided by merchant.\
See Webhook [Payment Notification](webhooks/payment-webhooks.md).

### [Response Parameters](checkout-api.md#response-parameters)

These parameters will be returned for all the response status.

#### [agreement](checkout-api.md#agreement-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

A pre-established contractual agreement with the customer making the payment, allowing the merchant to securely retain and later use their payment details for particular purposes. This might include agreements like regular payments for services such as mobile subscriptions, payments in installments for purchases, arrangements for one-time charges like account reloads, or adhering to common industry practices such as penalty fees for missed appointments

**Presence condition:**

* This parameter should be included when the payment\_type is set to "`auto_debit`" On the other hand, it must not be sent when the [payment\_type](checkout-api.md#payment\_type-string-mandatory) is designated as "`one_off`" Importantly, this isn't restricted to just the initial transaction but should be consistently present in all following transactions associated with the "`auto_debit`" payment type.

<details>

<summary>agreement child parameters</summary>

#### [id](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#id-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

A unique identifier for the agreement

#### [amount\_variability](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#amount\_variability-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

Presents if the payment amount can vary with each transaction.

#### [start\_date](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#start\_date-date-conditional) _<mark style="color:blue;">`date`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

&#x20;Agreement starting date.

#### [expiry\_date](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#expiry\_date-date-conditional) _<mark style="color:blue;">`date`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The final date until which the agreement remains valid.

#### [max\_amount\_per\_cycle](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#max\_amount\_per\_cycle-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The maximum debit amount for one billing cycle.

#### [cycle\_interval\_days](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#cycle\_interval\_days-integer-conditional) _<mark style="color:blue;">`integer`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The number of days between each recurring payment.

#### [total\_cycles](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#total\_cycles-integer-conditional) _<mark style="color:blue;">`integer`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

The total number of payment cycles within the agreement duration.

#### [frequency](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#frequency-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

Represents how often the payment is to be processed.

#### [type](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#type-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

This is event-driven, with "`recurring`" as an example.

#### [seller](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#seller-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

Seller information data including:&#x20;

* `"name": "string",`&#x20;
* `"short_name": "string",`
* `"category_code": "string"`

#### [extra\_params](https://app.gitbook.com/o/QvpaILbKwb9WBfHGe5bZ/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/527/developer/webhooks/payment-webhooks#extra\_params-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;background-color:blue;">`conditional`</mark>_

&#x20;Provides additional information for payment processing. \
It includes the parameter "`payment_processing_day`" which provide information about the day of the month or a specific date when payment processing should occur, offering more control over the timing of payments.

</details>

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

#### [**attachment\_short\_url**](checkout-api.md#attachment\_short\_url-string-conditional) <mark style="color:blue;">**`string`**</mark> _<mark style="color:blue;">`conditional`</mark>_

A short attachment retrieval URL.\
Max length: 200.

**Presence condition:**

* [shortify\_attachment\_url](checkout-api.md#shortify\_attachment\_url-bool-optional) request parameter should be "true" in order to generate it.

#### [**billing\_address**](checkout-api.md#billing\_address-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s registered address data.\
See the request parameter [billing\_address](checkout-api.md#billing\_address-object-optional) for more information.

**Presence condition:**

* Any child parameter provided with the [billing\_address](checkout-api.md#billing\_address-object-optional) object in the request payload will be populated in the response as [billing\_address](checkout-api.md#billing\_address-object-conditional) child parameter.

#### [**checkout\_short\_url**](checkout-api.md#checkout\_short\_url-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Short [checkout url.](checkout-api.md#checkout\_url-string-mandatory)

**Presence condition:**

* [shortify\_checkout\_url](checkout-api.md#shortify\_checkout\_url-bool-optional) request parameter should be set to "true" in order to generate it.

#### [checkout\_url](checkout-api.md#checkout\_url-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

URL that directs the customer to the Ottu Checkout Page where they can see the payment details and available payment methods for the transaction. If you need to share the payment link over SMS or WhatsApp, use [checkout\_short\_url](checkout-api.md#checkout\_short\_url-string-conditional) instead.

#### [currency\_code](checkout-api.md#currency\_code-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The code of the currency used in the transaction.\
See the request parameter [currency\_code](checkout-api.md#currency\_code-string-required) for more information.

#### [customer\_email](checkout-api.md#customer\_email-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s email address.\
See the request parameter [customer\_email ](checkout-api.md#customer\_email-string-optional)for more information.

**Presence condition:**

* [customer\_email](checkout-api.md#customer\_email-string-optional) request parameter should be provided.

#### [customer\_first\_name](checkout-api.md#customer\_first\_name-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s first name.\
See the request parameter [customer\_first\_name](checkout-api.md#customer\_first\_name-string-optional) for more information.

**Presence condition:**

* [customer\_first\_name](checkout-api.md#customer\_first\_name-string-optional) request parameter should be provided.

#### [customer\_id](checkout-api.md#customer\_id-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It is a unique identifier assigned to a customer. This identifier can be used to distinguish one customer from another and can be utilized for tracking purposes or to retrieve specific customer information from the API.\
See the request parameter [customer\_id](checkout-api.md#customer\_id-string-optional) for more information.

**Presence condition:**

* [customer\_id](checkout-api.md#customer\_id-string-optional) request parameter should be provided.

#### [customer\_last\_name](checkout-api.md#customer\_last\_name-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Customer’s last name.\
See the request parameter [customer\_last\_name](checkout-api.md#customer\_last\_name-string-optional) for more information.

**Presence condition:**

* [customer\_last\_name](checkout-api.md#customer\_last\_name-string-optional) request parameter should be provided.

#### [custome\_phone](checkout-api.md#custome\_phone-string-conditional) _<mark style="color:blue;">**`string`**</mark>_ _<mark style="color:blue;">**`conditional`**</mark>_

Customer's phone number.\
See the request parameter [customer\_phone](checkout-api.md#customer\_phone-string-optional) for more information.

**Presence condition:**

* [customer\_phone](checkout-api.md#customer\_phone-string-optional) request parameter should be provided.

#### [due\_datetime](checkout-api.md#due\_datetime-string-date-time-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`date-time`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It specifies the deadline for payment. It has no effect on changing the transaction state, and the transaction can be paid even after due\_datetime.\
See the request parameter [due\_datetime](checkout-api.md#due\_datetime-string-date-time-optional) for more information.

#### [email\_recipients](checkout-api.md#email\_recipients-array-of-strings-conditional) _<mark style="color:blue;">`array of strings`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

This is a list of internal email recipients, who will receive notifications sent to the customer about their payment.\
See the request parameter [email\_recipients](checkout-api.md#email\_recipients-array-of-strings-optional) for more information.

**Presence condition:**

* [email\_recipients](checkout-api.md#email\_recipients-array-of-strings-optional) should be provided in the request payload.

#### [**expiration\_time**](checkout-api.md#expiration\_time-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It refers to the specific point in time after which the transaction cannot be paid anymore, and its state changes accordingly.\
See the request parameter [expiration\_time](checkout-api.md#expiration\_time-string-optional) for more information.

#### [extra](checkout-api.md#extra-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents additional data fields that can be dynamically added to the response using the extra request parameter.\
See the request parameter [extra](checkout-api.md#extra-object-optional) for more information.

**Presence condition:**

* Any child parameter provided with the [extra](checkout-api.md#extra-object-optional) object in the request payload will be populated in the response as [extra](checkout-api.md#extra-object-conditional) object child parameter.

#### [initiator\_id](checkout-api.md#initiator\_id-integer-initiator-conditional) _<mark style="color:blue;">`integer(initiator)`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

The user who initiated the creation of this payment transaction, if available. This field is optional and may be used to track who created the payment transaction\
Max length: 11.

**Presence condition:**

* It is present only when [Basic Authentication](authentication.md#basic-authentication) is used, because [API-Key](authentication.md#private-key-api-key) Authentication is not associated with any user.

#### [**language**](checkout-api.md#language-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

It represents the language code that is utilized for all communication related to payment transactions with the customer, including payment page, receipt, invoice, email, and SMS\
For more details check the request parameter [language](checkout-api.md#language-string-optional)

#### [**mode**](checkout-api.md#mode-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Default: “payment”.\
Value: “payment”.&#x20;

#### [notifications](checkout-api.md#notifications-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents the notification settings for this payment transaction which have been received and processed.\
See the request [notifications](checkout-api.md#notifications-object-optional) for more information.

**Presence condition:**

* Any child parameter provided with the [notifications](checkout-api.md#notifications-object-optional) object in the request payload will be populated in the response as [notifications](checkout-api.md#notifications-object-conditional) child parameter.

#### [**operation**](checkout-api.md#operation-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

Specifies the type of operation to be performed by the payment gateway. If set to 'purchase', the payment source will be directly charged. If set to 'authorize', the payment source will only be authorized and the actual charge will be made at a later time\
Max length: 16.

#### [order\_no](checkout-api.md#order\_no-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It is a unique identifier assigned to the payment transaction, which is primarily used for tracking purposes. The identifier can be set by the merchant or the system.\
See the request parameter [order\_no](checkout-api.md#order\_no-string-optional) for information.

**Presence condition:**

* [order\_no](checkout-api.md#order\_no-string-optional) request parameter should be included in the request payload.

#### [**payment\_methods**](checkout-api.md#payment\_methods-array-object-mandatory) _<mark style="color:blue;">`array [object]`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

An array containing all the payment methods derived from the [pg\_codes](checkout-api.md#pg\_codes-array-required) input. Each object in the array contains information about a single payment gateway, including its icon and the [redirect\_url](checkout-api.md#redirect\_url-string-optional) that will redirect the customer to the payment gateway's payment page

<details>

<summary>payment_methods <em>object details</em></summary>

#### :digit\_one: [**code** ](checkout-api.md#code-string)_<mark style="color:blue;">**`string`**</mark>_

Code of the Gateway Settings instance

#### :digit\_two:[ **name** ](checkout-api.md#name-string)_<mark style="color:blue;">`string`</mark>_&#x20;

Name of the Gateway Settings instance.

#### :digit\_three:[ **pg** ](checkout-api.md#pg-string)_<mark style="color:blue;">`string`</mark>_&#x20;

Name of the gateway, settings are applied to.

#### :digit\_four:[ **is\_sandbox** ](checkout-api.md#is\_sandbox-bool)_<mark style="color:blue;">`bool`</mark>_&#x20;

It is environment used for this PG settings or not.

#### :digit\_five: [**icon**](checkout-api.md#icon-string) _<mark style="color:blue;">`string`</mark>_&#x20;

URL to default icon of the current gateway.

#### :digit\_six:[ **flow** ](checkout-api.md#flow-string)_<mark style="color:blue;">`string`</mark>_&#x20;

Choice from (“redirect”, ...).

#### :digit\_seven: [**redirect\_url**](checkout-api.md#redirect\_url-string) _<mark style="color:blue;">`string`</mark>_&#x20;

This URL redirects to the payment page.See [redirect\_url](checkout-api.md#redirect\_url-string-optional)

</details>

#### [pg\_codes](checkout-api.md#pg\_codes-array-mandatory) _<mark style="color:blue;">`array`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The options of the payment gateway codes included in the request payload to enable customers to make payments.\
See the request parameter [pg\_codes](checkout-api.md#pg\_codes-array-required) for more information.

#### [product\_type](checkout-api.md#product\_type-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

The nature of the purchased product or service, which can be employed for the purpose of keeping track and generating reports.\
See the request parameter [product\_type](checkout-api.md#product\_type-string-optional) for information.

**Presence condition:**

* &#x20;[product\_type](checkout-api.md#product\_type-string-optional) request parameter should be included in the request payload.

#### [qr\_code\_url](checkout-api.md#qr\_code\_url-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

A QR code that, when scanned, redirects to the checkout page for this payment. This QR code may be displayed on invoices, receipts, or other documents to allow customers to easily access the checkout page and make a payment.&#x20;

**Presence condition:**

* The request parameter [generate\_qr\_code](checkout-api.md#generate\_qr\_code-bool-optional) should be set to "true".

#### [redirect\_url](checkout-api.md#redirect\_url-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents the URL where the customer will be redirected after the payment stage is complete.\
See the request parameter [redirect\_url](checkout-api.md#redirect\_url-string-optional) more information.

**Presence condition:**

* The request parameter [redirect\_url](checkout-api.md#redirect\_url-string-optional) should be provided.

#### [**session\_id**](checkout-api.md#session\_id-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

A unique identifier for each payment transaction, used to maintain the session state during the payment process. It can be used to perform subsequent operations, like retrieve, acknowledge, refund, capture, and cancellation.\
Max length: 128.

#### [shipping\_address](checkout-api.md#shipping\_address-object-conditional) _<mark style="color:blue;">`object`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

Shipping location data of the customer.\
See the request parameter [shipping\_address](checkout-api.md#shipping\_address-object-optional) for more information.

**Presence condition:**

* The child objects of the [shipping\_address](checkout-api.md#shipping\_address-object-optional) parameter provided in the request object will be populated as child objects of the [shipping\_address](checkout-api.md#shipping\_address-object-conditional) parameter in the response object.

#### [**state**](checkout-api.md#state-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The current state of the payment transaction, it helps to understand the progress of the payment.\
Enum: "created" "pending" "attempted" "authorized" "paid" "failed" "canceled" "expired" "invalided" "refunded" "cod".\
See [payment transaction state](../user-guide/payment-tracking/#payment-transaction-state-and-payment-attempt-state) for more information.

#### [type](checkout-api.md#type-string-mandatory) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">`mandatory`</mark>_

The type of the payment transaction.\
See the request parameter [type ](checkout-api.md#type-string-required)for more information.

#### [vendor\_name](checkout-api.md#vendor\_name-string-conditional) _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It represents the name of the merchant or vendor associated with a payment transaction.\
For more information see [vendor\_name](checkout-api.md#vendor\_name-string-optional).

**Presence condition:**

* The request parameter [vendor\_name](checkout-api.md#vendor\_name-string-optional) should be included in the request payload.

#### [**webhook\_url**](checkout-api.md#webhook\_url-url-conditional)  _<mark style="color:blue;">**`URL`**</mark>_ _<mark style="color:blue;">`conditional`</mark>_

It contains the URL where the payment result will be sent via a POST request after the customer has completed the payment session. The payment result will be included in the request body.\
See Webhook [Payment Notification](webhooks/payment-webhooks.md).

**Presence condition:**

* The request parameter [webhook\_url](checkout-api.md#webhook\_url-string-optional) should be provided.

### [API Schema Reference ](checkout-api.md#api-schema-reference)

For a more detailed technical understanding and the implementation specifics of these Checkout API please refer to the below Open API schema.

{% swagger src="../.gitbook/assets/Ottu API (24).yaml" path="/b/checkout/v1/pymt-txn/" method="post" %}
[Ottu API (24).yaml](<../.gitbook/assets/Ottu API (24).yaml>)
{% endswagger %}

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
"[type](checkout-api.md#type-string-required)", "[pg\_codes](checkout-api.md#pg\_codes-array-required)", "[amount](checkout-api.md#amount-string-required)", and  "[currency\_code](checkout-api.md#currency\_code-string-required)" are required parameters.\
When we add [notification](checkout-api.md#notifications-object-optional) we should add:\
"[customer\_email](checkout-api.md#customer\_email-string-optional)" for email notification.\
"[customer\_phone](checkout-api.md#customer\_phone-string-optional)" for SMS notification.
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
    "mode": "payment",
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
            "redirect_url": "https://<ottu-url>/checkout/5ca114666d472a170d3c4ea6cadbf347679b2532?chd-only=True"
        }
    ],
    "pg_codes": [
        "pg_codes"
    ],
    "session_id": "5ca114666d472a170d3c4ea6cadbf347679b2532",
    "state": "created",
    "type": "payment_request"
}

```

{% swagger method="patch" path="" baseUrl="https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}" summary="Update Payment" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" required="true" name="Authorization" type="API key" %}
Api-Key \{{api\_key\}}
{% endswagger-parameter %}
{% endswagger %}

#### [Update Payment Transaction ](checkout-api.md#update-payment-transaction)

Using a patch function is a good method of increasing trustability whenever any change gets made to the payment transaction, such as updating the amount on the card or removing items from the cart.

### [Parameters](checkout-api.md#parameters)

All the same fields from [create request](checkout-api.md#create-payment-transaction) can be used. The type of update is partial. But some fields can be cross-validated and require other fields to be provided.

{% swagger method="get" path="" baseUrl="https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}" summary="Retrieve Payment" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}

#### [Retrieve Payment Transaction](checkout-api.md#retrieve-payment-transaction)

To get the information of the payment transaction.

{% hint style="success" %}
**Authentication:** This endpoint is public
{% endhint %}
