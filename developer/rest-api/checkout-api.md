# Checkout API

## [Getting started](checkout-api.md#getting-started)

Ottu provides a collection of APIs, which is a quick way to test the payment and enables you to process and manage payments.

Ottu APIs accept and return JSON in the HTTP body, and return standard HTTP response codes. You can create/update/get operations.

{% hint style="warning" %}
**REST APIs should be called only from server not from clients, like mobile apps or browser apps.**
{% endhint %}

{% hint style="success" %}
#### <mark style="color:blue;">****</mark>[**Authentication**](checkout-api.md#authentication)  **** &#x20;

[API Private key](authentication.md#private-key)
{% endhint %}

## [Create payment transaction](checkout-api.md#create-payment-transaction)

To initiate payment transaction.

#### ****[**Endpoint**](checkout-api.md#endpoint-2)****

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/checkout/v1/pymt-txn/
```

{% hint style="info" %}
* The applied permissions are only those which are related to PG codes the user is allowed to use.
* The payment transaction should be created automatically, when the merchant knows the due amount.
{% endhint %}

### [Request parameters](checkout-api.md#request-parameters)

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
Max length: 100

#### ****[**type**](checkout-api.md#type-string-required) ** **_<mark style="color:blue;">**`string`**</mark>_<mark style="color:blue;">** **</mark><mark style="color:blue;">****</mark>** **_<mark style="color:red;">**`required`**</mark>_

Defines under which plugin the transaction will be created.\
Available choices: payment\_request, e\_commerce.\
Max length: 24.

#### ****

#### [shortify\_checkout\_url](checkout-api.md#shortify\_checkout\_url-bool-optional) <mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short checkout URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as Bitly, is [configured](../../user-guide/configuration.md#url-shortener-configurations), the [checkout\_short\_url](checkout-api.md#checkout\_short\_url-url) will be shorter than checkout url response parameter. If not configured, the [checkout\_short\_url](checkout-api.md#checkout\_short\_url-url) will be in the format of "https://\<ottu-url>>/b/abc123".

#### [s**hortify\_attachment\_url** ](checkout-api.md#shortify\_attachment\_url-bool-optional)<mark style="color:blue;">**`bool`**</mark> _<mark style="color:blue;">**`optional`**</mark>_

If true, it generates short attachment retrieval URL, which could be embedded in either SMS, Email, or WhatsApp messages, as it uses fewer characters.\
If an external URL shortening service, such as Bitly, is [configured](../../user-guide/configuration.md#url-shortener-configurations), the [attachment\_short\_url](checkout-api.md#attachment\_short\_url-url) will be shorter than  attachment response parameter. if not configured, the attachment\_short\_url will be in the same format with attachment response parameter.

#### ****[**currency\_code** ](checkout-api.md#currency\_code-string-required)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">`required`</mark>_

The currency code has to be added in Currency > Currencies.\
More details [https://en.wikipedia.org/wiki/ISO\_4217](https://en.wikipedia.org/wiki/ISO\_4217)\
3 letters code.

#### ****[**mode**](checkout-api.md#mode-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The default mode is “payment”.\
Max length: 7.

#### [due\_datetime](checkout-api.md#due\_datetime-date-time-optional) **** _<mark style="color:blue;">`date time`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

It is the due date of the invoice against the requested payment amount.\
The default value is UTC.\
Should be in format (DD/MM/YYYY hh:mm)\
SMS and email reminders can be sent prior and after the due datetime.

#### ****[**pg\_codes** ](checkout-api.md#pg\_codes-list-required)_<mark style="color:blue;">`list`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">`required`</mark>_

The pg code is a list of PG setting's codes.\
Users provide only one PG code.\
**For**[ **Basic authentication**](authentication.md#basic-authentication)**:** User can use the PG code that has permission to access to.\
**For**[ **API Private key**](authentication.md#private-key)**:** User can use all the PG code.\
Max length: 255.

#### ****[**webhook\_url** ](checkout-api.md#webhook\_url-url-optional)_<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

In case of a payment event or payment operation, Ottu triggers an HTTP request to this URL, to disclose transactional data.\
It should be provided by merchant.\
Max length: 200.\
See [Webhook](../webhook/).

#### ****[**redirect\_url** ](checkout-api.md#redirect\_url-url-optional)_<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where the user is being redirected after the payment process gets completed.\
Redirect URL can be set in the administration panel.\
Max length: 200.

#### ****[**customer\_id**](checkout-api.md#customer\_id-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Customer ID is created by a merchant.\
If the customer ID is presented in the Ottu checkout SDK, regardless of the mobile app being used, the customer will be prompted to save the card.\
This will be a checkbox for the customer to choose whether to save the card.\
Max length: 64.

#### ****[**customer\_first\_name** ](checkout-api.md#customer\_first\_name-string-optional)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

For the customer's first name.\
Max length 64.

#### ****[**customer\_last\_name**](checkout-api.md#customer\_last\_name-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

For the customer's first name.\
Max length 64.

#### ****[**customer\_email** ](checkout-api.md#customer\_email-string-optional)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where to pass the customer’s email address.\
Have to be a valid email address.\
Max length 128.

#### ****[**customer\_phone**](checkout-api.md#customer\_phone-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where to pass the customer’s phone number.\
Max length 16.

{% hint style="info" %}
If the merchant wants to enable KFAST on KNET, cusotmer\_phone will be ** **_<mark style="color:red;">**`required`**</mark>_\
**KFAST** is a tokenization feature on KPay page, which works with UDF3 mapped with customer\_phone.
{% endhint %}

#### ****[**billing\_address**](checkout-api.md#billing\_address-dict-optional) **** _<mark style="color:blue;">`dict`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The billing address is the customer’s registered address.

#### ****:digit\_one:[ **line1** ](checkout-api.md#line1-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

One of the billing address parameters and should be filled by street & house data.\
Max length: 128.

#### ****:digit\_two:[ **line2** ](checkout-api.md#line2-string-optional)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">**`optional`**</mark>_

For accuracy purpose, Additional address data for the [line1](checkout-api.md#line1-string-required).\
Max length: 128.

#### ****:digit\_three:[city](checkout-api.md#city-string-required) _<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

The city where the customer is living and registered.\
Max length: 40.

#### ****:digit\_four: [**state**](checkout-api.md#state-string-optional) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">**`optional`**</mark>_

State of the customer’s city (sometimes the same as the [city](checkout-api.md#city-string-required)).\
Max length: 40.

#### ****:digit\_five:****[ **country** ](checkout-api.md#country-string-required)_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Customer’s country, ISO 3166-1 Alpha-2 code.\
Will be validated against existing countries.\
Max length: 2.

#### ****:digit\_six:****[ **postal\_code**](checkout-api.md#postal\_code-string-required) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_<mark style="color:red;">** **</mark><mark style="color:red;">****</mark>&#x20;

Postal code (it may have different length for different countries).\
Max length: 12.

#### [shipping\_address](checkout-api.md#shipping\_address-dict-optional) **** _<mark style="color:blue;">`dict`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The billing address is the customer’s registered address.

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

#### ****:digit\_four: [**state**](checkout-api.md#state-string-optional-1) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">**`optional`**</mark>_

The city where the shipment should be delivered to. (sometimes the same as the [city](checkout-api.md#city-string-required-1)).\
Max length 40.

#### ****:digit\_five: **** [**country**](checkout-api.md#country-string-required-1) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_

Destination country, ISO 3166-1 Alpha-2 code.\
Will be validated against existing countries.\
Max length: 2.

#### ****:digit\_six: **** [**postal\_code**](checkout-api.md#postal\_code-string-required-1) ** **_<mark style="color:blue;">**`string`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">**`required`**</mark>_<mark style="color:red;">** **</mark><mark style="color:red;">****</mark>&#x20;

Postal code (it may have different length for different countries).\
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

#### ****[**order\_no**](checkout-api.md#order\_no-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Merchant unique identifier for the transaction. ABC123\_1, ABC123\_2.\
Max length: 128.

#### ****[**notifications**](checkout-api.md#notifications-dict-optional) **** _<mark style="color:blue;">`dict`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Notification events are triggered by specific states, and it can be sent in various ways, such like SMS and email.\
Max lenth: longtext.

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

#### ****[**vendor\_name**](checkout-api.md#vendor\_name-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

To pass the vendor’s name.\
Max lengthlength: 64.

#### ****[**expiration\_time** ](checkout-api.md#expiration\_time-date-optional)_<mark style="color:blue;">`date`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Expiration time is for the payment cycle. \
The default value is one hour.\
Should be In format (HH:MM:SS).\
Should be consistency with [order\_no](checkout-api.md#order\_no-string-optional) expiration time.

{% hint style="info" %}
In order to automatically change the state to **expired**, **Expire Payment Transactions**? Field should be enabled.
{% endhint %}

From Ottu dashboard > administration panel > config > configuration page, then enable field **Expire Payment Transactions**? Otherwise, the transaction will be marked as expired when the customer attempts to pay past the expiration time.

#### ****[**email\_recipients** ](checkout-api.md#email\_recipients-list-optional)_<mark style="color:blue;">`list`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

When the merchant wants to email more people.\
Max length: 254.

#### ****[**extra** ](checkout-api.md#extra-dict-optional)_<mark style="color:blue;">`dict`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

The merchant can send anything in key value form.\
**For example,** the merchant can define a validation field in extra parameters, then apply the validation rules.

#### ****[**product\_type**](checkout-api.md#product\_type-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Product information.\
Max length: 128.

#### ****[**language**](checkout-api.md#language-string-optional) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

ISO 639-2 language code\
[https://www.loc.gov/standards/iso639-2/php/code\_list.php](https://www.loc.gov/standards/iso639-2/php/code\_list.php).\
Default language is en.\
Max length: 2.

### [Response parameters](checkout-api.md#response-parameters)

These parameters will be returned for all the response status.

#### ****[**session\_id** ](checkout-api.md#session\_id-string-read-only)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

Ottu unique identifier which gets generated when the transaction is created.\
It can be used to perform subsequent operations, like retrieve, acknowledge, refund, capture, and cancelation.\
Max lengthlength: 128.

#### ****[**operation** ](checkout-api.md#operation-string-read-only)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

Choice from ("purchase","authorize"). Depnding on how the PG is being selected.\
Max length: 16.

#### [initiator\_id](checkout-api.md#initiator\_idstring-read-only)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

it’s the ID of the user who did the api call.\
_`It is pressent only when Basic Authentication is used, because API Key Authentication is not associated with any user`_.\
Max length: 11.

#### ****[**checkout\_short\_url**](checkout-api.md#checkout\_short\_url-url-read-only) **** _<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

Short checkout url.\
[shortify\_checkout\_url](checkout-api.md#shortify\_checkout\_url-bool-optional) request parameter should be "true" in order to generate it.

#### ****[**attachment** ](checkout-api.md#attachment-url)_<mark style="color:blue;">`URL`</mark>_

Attachment retrieval URL, the attachment should be uploaded using [attachment](checkout-api.md#attachment-file-optional) request parameter.

#### ****[**attachment\_short\_url**](checkout-api.md#attachment\_short\_url-url) **** _<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:red;"><mark style="color:blue;"><mark style="color:blue;"></mark> <mark style="color:red;"></mark>_<mark style="color:red;">`read only`</mark>_

A short attachment retrieval URL.\
[shortify\_attachment\_url](checkout-api.md#shortify\_attachment\_url-bool-optional) request parameter should be "true" in order to generate it.\
Max length: 200.

#### ****[**amount** ](checkout-api.md#amount-string)_<mark style="color:blue;">`string`</mark>_

The merchant should always check if the amount he receives from Ottu is the same amount of the order, to avoid user changing the cart amount in between.

#### ****[**payment\_methods** ](checkout-api.md#payment\_methods-list)_<mark style="color:blue;">`list`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

List of dicts.

#### ****[**dict**](checkout-api.md#dict)****

Dict generated according to specific [pg\_code](checkout-api.md#pg\_codes-list-required) from pg\_codes list from request.

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

Using a patch function is a good method of increasing trustability whenever any change gets made to the payment transaction, such as updating the amount on the card or removing items from the cart.

#### [Endpoint](checkout-api.md#endpoint-1)

<mark style="color:purple;">**PATCH:**</mark>

```url
https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}
```

### <mark style="color:blue;"></mark>[Parameters](checkout-api.md#parameters)

All the same fields from create request can be used. The type of update is partial. But some fields can be cross-validated and require other fields to be provided.

## <mark style="color:blue;">****</mark>[**Retrieve**](checkout-api.md#retrieve)

To get information of payment transaction.

{% hint style="success" %}
**Authentication:** This endpoint is public
{% endhint %}

#### [Endpoint](checkout-api.md#endpoint-2)

<mark style="color:blue;">**GET:**</mark>

```url
https://<ottu-url>/b/checkout/v1/pymt-txn/{session_id}
```
