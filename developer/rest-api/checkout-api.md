# Checkout API

## [Getting Started](checkout-api.md#getting-started)

Ottu provides a comprehensive collection of APIs that offer a seamless and efficient way to test payments and enable merchants to accept and process transactions instantly. The Checkout API is the cornerstone of any payment initiation, whether it's [API-based](checkout-api.md) or [SDK-based](../checkout-sdk/).

## [Best Practices and Guidelines](checkout-api.md#best-practices-and-guidelines)

In order to ensure optimal transaction success tracking and minimize the number of required payment transactions, merchants should [create](checkout-api.md#create-payment-transaction) a Payment Transaction as soon as the amount is known. This typically occurs when a customer adds their first item to their cart. Following this, any changes to the total amount should be updated using the [Checkout API PATCH](checkout-api.md#update) method.

By updating the same payment transaction, rather than creating a new one for each payment attempt, merchants can more effectively trace customer interactions with their cart. This is particularly useful for events such as insufficient funds, where a customer may remove an item from their cart and successfully complete a transaction on their next attempt. Tracking and analyzing such events can help merchants make data-driven decisions for future improvements.

## [Permissions](checkout-api.md#permissions)

Permissions are managed using [Basic Authentication](authentication.md#basic-authentication) and [API-Keys](authentication.md#api-keys). \
Specifically, Basic Authentication is used to grant permissions for creating, updating, and reading data, as well as using allowed [PG codes](checkout-api.md#pg\_codes-list-required) when [creating ](checkout-api.md#create-payment-transaction)or [updating](checkout-api.md#update) payment transactions.

It is important to ensure that the appropriate level of permissions is assigned to each user or application using the APIs. This can help to prevent unauthorized access or modification of sensitive data. Additionally, it is recommended to rotate API-Keys on a regular basis and to use secure password storage practices when using Basic Authentication.

Ottu Checkout API supports different levels of permissions for the Payment Request and E-Commerce plugins. The permissions depend on the [authentication ](authentication.md)method being used.

### [API Key](checkout-api.md#api-key)

When using the [API-Key](authentication.md#api-keys), all permissions are granted by default, as the API-Key is considered to have admin permissions.

### [Basic Authentication](checkout-api.md#basic-authentication)

For [Basic Authentication](authentication.md#basic-authentication), permissions are granted as follows:

#### [Create](checkout-api.md#create)

* To create a transaction, the user needs specific permission depending on the [plugin](../../user-guide/plugins/) being used:
  * "Can add payment requests" for the [Payment Request](../../user-guide/plugins/#payment-request) plugin
  * "Can add e-commerce payments" for the [E-Commerce](../../user-guide/plugins/#e-commerce) plugin
* Permission to use use the payment gateway code is also required: "Can use `pg_code`"

## [Create Payment Transaction](checkout-api.md#create-payment-transaction)

{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/checkout/v1/pymt-txn" summary="Create Payment" %}
{% swagger-description %}
To initiate payment transaction
{% endswagger-description %}

{% swagger-parameter in="header" required="true" name="Authorization" type="API key" %}
Api-Key {{api_key}}. See 

[API Key](authentication.md#api-keys)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="type" required="true" type="String" %}
The type of the payment transaction. See 

[type](checkout-api.md#type-string-required)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="pg_codes" type="list" required="true" %}
List of  the payment gateway involved. See 

[pg_codes](checkout-api.md#pg_codes-list-required)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" required="true" type="String" %}
Payment transaction amount. See 

[amount](checkout-api.md#amount-string-required)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="currency_code" required="true" type="String" %}
Currency used. See 

[currency_code](checkout-api.md#currency_code-string-required)


{% endswagger-parameter %}
{% endswagger %}

### [Request Parameters](checkout-api.md#request-parameters)

#### ****[**amount** ](checkout-api.md#amount-string-required)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">`required`</mark>_

The amount of the transaction\
The number of decimals must correlate with the [currency\_code](checkout-api.md#currency\_code-string-required).\
Max length: 24\
Min value: 0.01

#### [attachment](checkout-api.md#attachment-fileoptional) <mark style="color:blue;">**`file`**</mark>_<mark style="color:blue;">**`optional`**</mark>_

The attachment file will be stored along with the payment transaction, and the payment transaction supports only one attachment\
It works only with [multipart/form-data](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using\_FormData\_Objects) encoding type\
Attachment could not be sent using [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/JSON) encoding type\
Allowed extensions:"pdf", "jpeg", "png", "doc", "docx", "jpg", "xls", "xlsx"\
The name of the attached file should be no more than 100.

#### ****[**billing\_address**](checkout-api.md#billing\_address-object-optional) **** _<mark style="color:blue;">`object`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The billing address is the customer’s registered address.

<details>

<summary><em>billing_address object details</em></summary>

#### ****:digit\_one:[ **line1** ](checkout-api.md#line1-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

One of the billing address parameters and should be filled by street & house data.\
Max length: 128.

#### ****:digit\_two:[ **line2** ](checkout-api.md#line2-string-optional)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">**`optional`**</mark>_

For accuracy purpose, Additional address data for the [line1](checkout-api.md#line1-string-required).\
Max length: 128.

#### ****:digit\_three:[city](checkout-api.md#city-string-required) _<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

The city where the customer is living and registered.\
Max length: 40.

#### ****:digit\_four: [**state** ](checkout-api.md#state-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">**`optional`**</mark>_

State of the customer’s city (sometimes the same as the [city](checkout-api.md#city-string-required)).\
Max length: 40.

#### ****:digit\_five:****[ **country** ](checkout-api.md#country-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Customer’s country, ISO 3166-1 Alpha-2 code.\
Will be validated against existing countries.\
Max length: 2.

#### ****:digit\_six:****[ **postal\_code**](checkout-api.md#postal\_code-string-required) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_<mark style="color:red;">** **</mark><mark style="color:red;">****</mark>&#x20;

Postal code (maybe has different length for different countries).\
Max length: 12.

</details>

#### ****[**currency\_code** ](checkout-api.md#currency\_code-string-required)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">`required`</mark>_

The currency code has to be added in Currency > Currencies.\
More details [https://en.wikipedia.org/wiki/ISO\_4217](https://en.wikipedia.org/wiki/ISO\_4217)\
3 letters code.

#### ****[**customer\_email** ](checkout-api.md#customer\_email-string-optional)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where to pass the customer’s email address.\
Have to be a valid email address.\
Max length 128.

#### ****[**customer\_first\_name** ](checkout-api.md#customer\_first\_name-string-optional)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

For the customer's first name.\
Max length 64.

#### ****[**customer\_id**](checkout-api.md#customer\_id-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Customer ID is created by a merchant.\
If the customer ID is presented in the Ottu checkout SDK, regardless of the mobile app being used, the customer will be prompted to save the card.\
This will be a checkbox for the customer to choose whether to save the card.\
Max length: 64.

{% hint style="info" %}
If the merchant wants to enable KFAST on KNET, [customer\_phone](checkout-api.md#customer\_phone-string-optional) will be ** **_<mark style="color:red;">**`required`**</mark>_\
**KFAST** is a tokenization feature on KPay page, which works with UDF3 mapped with [customer\_phone](checkout-api.md#customer\_phone-string-optional).
{% endhint %}

#### [customer\_last\_name](checkout-api.md#customer\_last\_name-string-optional) <mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

For the customer's last name.\
Max length 64.

#### [customer\_phone](checkout-api.md#customer\_phone-string-optional) _<mark style="color:blue;">`string optional`</mark>_

For the customer's phone number.\
Max length 16.

#### [due\_datetime](checkout-api.md#due\_datetime-string-optional) **** _<mark style="color:blue;">`date time`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

It is the due date of the invoice against the requested payment amount.\
The default value is UTC.\
Should be in format (DD/MM/YYYY hh:mm)\
SMS and email reminders can be sent prior and after the due datetime.

#### ****[**email\_recipients** ](checkout-api.md#email\_recipients-list-optional)_<mark style="color:blue;">`list`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

When the merchant wants to email more people.\
Max length: 254.

#### ****[**expiration\_time** ](checkout-api.md#expiration\_time-date-optional)_<mark style="color:blue;">`date`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Expiration time is for the payment cycle. \
The default value is one hour.\
Should be In format (HH:MM:SS).\
Should be consistency with [order\_no](checkout-api.md#order\_no-string-optional) expiration time.

{% hint style="info" %}
In order to automatically change the state to **expired**, **Expire Payment Transactions**? Field should be enabled.

From Ottu dashboard > administration panel > config > configuration page, then enable field **Expire Payment Transactions**? Otherwise, the transaction will be marked as expired when the customer attempts to pay past the expiration time.
{% endhint %}

#### ****[**extra**](checkout-api.md#extra-object-optional) **** _<mark style="color:blue;">`object`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The merchant can send anything in key value form.\
**For example,** the merchant can define a validation field in extra parameters, then apply the validation rules.

#### ****[**language**](checkout-api.md#language-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

ISO 639-2 language code\
[https://www.loc.gov/standards/iso639-2/php/code\_list.php](https://www.loc.gov/standards/iso639-2/php/code\_list.php).\
Default language is en.\
Max length: 2.

#### ****[**mode**](checkout-api.md#mode-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The default mode is “payment”.\
Max length: 7.

#### ****[**notifications**](checkout-api.md#notifications-object-optional) **** _<mark style="color:blue;">`object`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Notification events are triggered by specific states, and it can be sent in various ways, such like SMS and email.\
Max lenth: longtext.

<details>

<summary><em>notifications object details</em></summary>

#### ****:digit\_one:[ **email** ](checkout-api.md#email-list-optional)_<mark style="color:blue;">**`list`**</mark>_<mark style="color:blue;">** **</mark><mark style="color:blue;">****</mark><mark style="color:blue;">** **</mark>_<mark style="color:blue;">**`optional`**</mark>_

**Will be triggered at the following notification events:**\
\[“created”, "paid", "canceled", "failed", "expired", "authorized", "voided", "refunded", "captured"]\
**For failed**, in case payment transitions to **error** state and **failed** state was asked to send an email for, then the customer will get an email.\
Max length: 50.

#### ****:digit\_two:[ **SMS**](checkout-api.md#sms-list-optional) ** **_<mark style="color:blue;">**`list`**</mark>_<mark style="color:blue;">** **</mark><mark style="color:blue;">****</mark><mark style="color:blue;">** **</mark>_<mark style="color:blue;">**`optional`**</mark>_

**Will be triggered at the following notification events:**\
\[“created”, "paid", "canceled", "failed", "expired", "authorized", "voided", "refunded", "captured"]\
**For failed**, in case payment transitions to **error** state and **failed** state was asked to send an email for, then the customer will get an SMS.\
Max length: 50.

</details>

#### ****[**order\_no**](checkout-api.md#order\_no-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Merchant unique identifier for the transaction. ABC123\_1, ABC123\_2.\
Max length: 128.

#### ****[**pg\_codes** ](checkout-api.md#pg\_codes-list-required)_<mark style="color:blue;">`list`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">`required`</mark>_

The pg code is a list of PG setting's codes.\
Users provide only one PG code.\
**For**[ **Basic authentication**](authentication.md#basic-authentication)**:** User can use the PG code that has permission to access to.\
**For**[ **API Private key**](authentication.md#private-key)**:** User can use all the PG code.\
Max length: 255.

#### ****[**product\_type**](checkout-api.md#product\_type-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Product information.\
Max length: 128.

#### ****[**redirect\_url** ](checkout-api.md#redirect\_url-url-optional)_<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where the user is being redirected after the payment process gets completed.\
Redirect URL can be set in the administration panel.\
Max length: 200.

#### [shipping\_address](checkout-api.md#shipping\_address-object-optional) **** _<mark style="color:blue;">`object`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The billing address is the customer’s registered address.

<details>

<summary><em>shipping_address object details</em></summary>

#### ****:digit\_one:[ **line1** ](checkout-api.md#line1-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Location details where the shipment should be delivered to.\
Should be filled by street & house data.\
Max length 128.

#### ****:digit\_two:[ **line2** ](checkout-api.md#line2-string-optional)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">**`optional`**</mark>_

For accuracy purpose, Additional address data for the [line1](checkout-api.md#line1-string-required-1).\
Max length 128.

#### ****:digit\_three:[city](checkout-api.md#city-string-required) _<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

The city where the shipment should be delivered to.\
Max length 40.

#### ****:digit\_four: [**state** ](checkout-api.md#state-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">**`optional`**</mark>_

The city where the shipment should be delivered to. (sometimes the same as the [city](checkout-api.md#city-string-required-1)).\
Max length 40.

#### ****:digit\_five:****[ **country** ](checkout-api.md#country-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Destination country, ISO 3166-1 Alpha-2 code.\
Will be validated against existing countries.\
Max length: 2.

#### ****:digit\_six: **** [**postal\_code**](checkout-api.md#postal\_code-string-required-1) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_<mark style="color:red;">** **</mark><mark style="color:red;">****</mark>&#x20;

Postal code (maybe has different length for different countries).\
Max length: 12.

#### ****:digit\_seven: **** [first\_name](checkout-api.md#first\_name-string-required) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_<mark style="color:red;">** **</mark><mark style="color:red;">****</mark>&#x20;

Shipment recipient first name.\
Max length: 64.

#### :digit\_eight: [last\_name](checkout-api.md#last\_name-string-required) _<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Shipment recipient last name.\
Max length: 64.

#### :digit\_nine: [email ](checkout-api.md#email-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Shipment recipient email address.\
Max length: 128.

#### :digit\_one::digit\_zero: [phone](checkout-api.md#phone-string-required) _<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Shipment recipient phone number.\
Max length: 16.

</details>

#### [s**hortify\_attachment\_url** ](checkout-api.md#shortify\_attachment\_url-bool-optional)<mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short attachment retrieval URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as Bitly, is [configured](../../user-guide/configuration.md#url-shortener-configurations), the [attachment\_short\_url](checkout-api.md#attachment\_short\_url-url) will be shorter than  attachment response parameter. if not configured, the attachment\_short\_url will be in the same format with attachment response parameter.

#### [shortify\_checkout\_url](checkout-api.md#shortify\_checkout\_url-bool-optional) <mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short checkout URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as Bitly, is [configured](../../user-guide/configuration.md#url-shortener-configurations), the [checkout\_short\_url](checkout-api.md#checkout\_short\_url-url) will be shorter than checkout url response parameter. If not configured, the [checkout\_short\_url](checkout-api.md#checkout\_short\_url-url) will be in the format of "https://\<ottu-url>>/b/abc123".

#### ****[**type**](checkout-api.md#type-string-required) ** **_<mark style="color:blue;">**`string`**</mark>_<mark style="color:blue;">** **</mark><mark style="color:blue;">****</mark>** **_<mark style="color:red;">**`required`**</mark>_

Defines under which plugin the transaction will be created.\
Available choices: payment\_request, e\_commerce.\
Max length: 24.

#### [**vendor\_name**](checkout-api.md#vendor\_name-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

To pass the vendor’s name.\
Max lengthlength: 64.

#### ****[**webhook\_url** ](checkout-api.md#webhook\_url-url-optional)_<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

In case of a payment event or payment operation, Ottu triggers an HTTP request to this URL, to disclose transactional data.\
It should be provided by merchant.\
Max length: 200.\
See [Webhook](../webhook/).

### [Response Parameters](checkout-api.md#response-parameters)

These parameters will be returned for all the response status.

#### ****[**amount** ](checkout-api.md#amount-string)_<mark style="color:blue;">`string`</mark>_

The merchant should always check if the amount he receives from Ottu is the same amount of the order, to avoid user changing the cart amount in between.

#### ****[**attachment**](checkout-api.md#undefined) **** _<mark style="color:blue;">`URL`</mark>_

Attachment retrieval URL, the attachment should be uploaded using [attachment](checkout-api.md#attachment-file-optional) request parameter.

#### ****[**attachment\_short\_url**](checkout-api.md#attachment\_short\_url-url) **** _<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

A short attachment retrieval URL.\
[shortify\_attachment\_url](checkout-api.md#shortify\_attachment\_url-bool-optional) request parameter should be "true" in order to generate it.\
Max length: 200.

#### ****[**checkout\_short\_url**](checkout-api.md#checkout\_short\_url-url) **** _<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

Short checkout url.\
[shortify\_checkout\_url](checkout-api.md#shortify\_checkout\_url-bool-optional) request parameter should be "true" in order to generate it.

#### [initiator\_id](checkout-api.md#initiator\_idstring-read-only)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

it’s the ID of the user who did the api call.\
_`It is pressent only when`_ [_`Basic Authentication`_](authentication.md#basic-authentication) _`is used, because`_ [_`API Key Authentication`_](authentication.md#api-keys) _`is not associated with any user`_.\
Max length: 11.

#### ****[**operation** ](checkout-api.md#operation-string)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

Choice from ("purchase","authorize"). Depnding on how the PG is being selected.\
Max length: 16.

#### ****[**payment\_methods** ](checkout-api.md#payment\_methods-list)_<mark style="color:blue;">`list`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

List of dicts.

#### ****[**dict**](checkout-api.md#dict)****

Object generated according to specific [pg\_code](checkout-api.md#pg\_codes-list-required) from pg\_codes list from request.

<details>

<summary><em>dict details</em></summary>

#### :digit\_one: **** [**code** ](checkout-api.md#code-string)_<mark style="color:blue;">**`string`**</mark>_

Code of the Gateway Settings instance

#### ****:digit\_two:****[ **name** ](checkout-api.md#name-string)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Name of the Gateway Settings instance.

#### ****:digit\_three:****[ **pg** ](checkout-api.md#pg-string)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Name of the gateway, settings are applied to.

#### ****:digit\_four:****[ **is\_sandbox** ](checkout-api.md#is\_sandbox-bool)_<mark style="color:blue;">`bool`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

It is environment used for this PG settings or not.

#### ****:digit\_five:[ **icon**](checkout-api.md#icon-string-url) **** _<mark style="color:blue;">`string:URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

URL to default icon of the current gateway.

#### ****:digit\_six:****[ **flow** ](checkout-api.md#flow-string)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Choice from (“redirect”, ...).

#### ****:digit\_seven: **** [**payment\_url**](checkout-api.md#payment\_url-string-url) **** _<mark style="color:blue;">`string:URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

This URL redirects to the payment page.

</details>

#### ****[**session\_id** ](checkout-api.md#session\_id-string-read-only)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

Ottu unique identifier which gets generated when the transaction is created.\
It can be used to perform subsequent operations, like retrieve, acknowledge, refund, capture, and cancelation.\
Max lengthlength: 128.

### [Example: Checkout API - create payment transaction (request-response)](checkout-api.md#example-checkout-api-create-payment-transaction-request-response)

#### ****[**API-Request (default required data)**](checkout-api.md#api-request-default-required-data)****

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
"[type](checkout-api.md#type-string-required)", "[pg\_codes](checkout-api.md#pg\_codes-list-required)", "[amount](checkout-api.md#amount-string-required)", and  "[currency\_code](checkout-api.md#currency\_code-string-required)" are required parameters.\
When we add [notification](../../user-guide/payment-tracking.md#notifications) we should add:\
"[customer\_email](checkout-api.md#customer\_email-string-optional)" for email notification.\
"[customer\_phone](checkout-api.md#customer\_phone-string-optional)" for sms notification.
{% endhint %}

#### ****[**Response**](checkout-api.md#response) ****&#x20;

```json
{
    "amount": "14.000",
    "checkout_url": "https://<ottu-url>/b/checkout/redirect/start/?session_id=5ca114666d472a170d3c4ea6cadbf347679b2532",
    "currency_code": "KWD",
    "customer_email": "example@example.com",
    "customer_phone": "customer phone",
    "due_datetime": "15/01/2023 11:04:55",
    "expiration_time": "02:00:00",
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

## <mark style="color:blue;">****</mark>[**Update**](checkout-api.md#update)

{% swagger method="patch" path="" baseUrl="https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}" summary="Update Payment" %}
{% swagger-description %}
Using a patch function is a good method of increasing trustability whenever any change gets made to the payment transaction, such as updating the amount on the card or removing items from the cart.
{% endswagger-description %}

{% swagger-parameter in="header" required="true" name="Authorization" type="API key" %}
Api-Key {{api_key}}
{% endswagger-parameter %}
{% endswagger %}

### <mark style="color:blue;"></mark>[Parameters](checkout-api.md#parameters)

All the same fields from [create request](checkout-api.md#create-payment-transaction) can be used. The type of update is partial. But some fields can be cross-validated and require other fields to be provided.

## <mark style="color:blue;">****</mark>[**Retrieve**](checkout-api.md#retrieve)

{% hint style="success" %}
**Authentication:** This endpoint is public
{% endhint %}

{% swagger method="get" path="" baseUrl="https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}" summary="Retrieve Payment" %}
{% swagger-description %}
To get the information of the payment transaction
{% endswagger-description %}
{% endswagger %}
