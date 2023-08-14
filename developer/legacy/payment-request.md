# Payment Request

## <mark style="color:blue;"></mark>[Getting started](payment-request.md#getting-started)

Don't integrate with this API; use checkout-API instead [checkout-api.md](../checkout-api.md "mention")

### <mark style="color:blue;"></mark>[Base URLs](payment-request.md#base-urls)

> POST \<ottu-url>/pos/crt/​

{% hint style="info" %}
The variable URL is the installation domain. The URL must be secure, i.e.: https://
{% endhint %}

## <mark style="color:blue;"></mark>[Body parameters](payment-request.md#body-parameters)

### [Inbound](payment-request.md#inbound)

_<mark style="color:red;">**Required**</mark>_

**amount** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Against currency type for the decimal places.

**currency\_code**_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Letters only. ISO 4217 codes https://en.wikipedia.org/wiki/ISO\_4217. Validated against the gateway settings currencies list defined in admin. Max length: 3.

**customer\_email**&#x20;

**initiator** _<mark style="color:blue;">`integer`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

ID of the user who initiates the transaction.

_<mark style="color:blue;">**Optional**</mark>_

#### **gateway\_code**_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Gateway account code defined in admin panel.

#### **language**_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

ISO 639-2 language code. https://www.loc.gov/standards/iso639-2/php/code\_list.php. Max length: 2.

#### **order\_no**_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Merchant unique identifier. Max length: 128.

#### **email\_payment\_details** _<mark style="color:blue;">`bool`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Send email to the customer with payment result details. Validated against customer\_email.

#### **sms\_payment\_details** _<mark style="color:blue;">`bool`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Send email to the customer with payment result details.

#### **sms\_notification**_<mark style="color:blue;">`bool`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Send email to the customer with payment result details. Validated against customer\_phone.

#### **email\_notification**_<mark style="color:blue;">`bool`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Send payment link to customer to pay via email. Validated against customer\_email.

#### **customer\_phone** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

International phone number, with prefix. Max length: 16.

#### **customer\_first\_name**&#x20;

#### **customer\_last\_name**&#x20;

#### **customer\_address\_line1** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

no specific limit.

#### **customer\_address\_line2** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

no specific limit.

**customer\_address\_city** <mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`string`</mark>_

Add a default address from admin. Max length: 40.

#### **customer\_address\_state** _<mark style="color:blue;">`string`</mark>_

&#x20;Max length: 40.

#### **customer\_address\_country** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Max length: 40.

#### **customer\_address\_postal\_code** _<mark style="color:blue;">`integer`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Max length: 12.

#### **redirect\_url**_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

URL where the user to be redirected after payment process has completed. Note that includes in query string order\_no and reference\_number. Can be set in admin panel also. Max length: 200.

#### **disclosure\_url**_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

URL where to send the payment result details. Must return a http status 200, in order to proceed with redirection to redirect\_url. Can be set in admin panel also. Max length: 200.

#### **initiator** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

URL where to send the payment result details. Must return a http status 200, in order to proceed with redirection to redirect\_url. Can be set in admin panel also. Max length: 200.

## <mark style="color:blue;"></mark>[Example of Knet request](payment-request.md#example-of-knet-request)

Merchant sends a request to the API endpoint

> <mark style="background-color:purple;">​https://pay.{yourdomain}/pos/d/crt/​</mark>

where Ottu is installed.

### [Post request](payment-request.md#post-request)

```json
{
   "gateway_code":"kpay",
   "currency_code":"KWD",
   "amount":"3",
   "order_no":"Cust001",
   "full_name":"Saifuddin Tahir",
   "customer_email":"​saifuddin@kuwaitnet.com​",
   "customer_phone":"96597117060",
   "initiator":"2",
   "disclosure_url":"http://postapp.knpay.net/disclose_ok/",
}
```

## <mark style="color:blue;"></mark>[Response disclosure URL](payment-request.md#response-disclosure-url)

Ottu will respond back the Payment transaction details in POST request to this URL ​disclosure\_url​ specified by the merchant while initiating the payment request, ​disclosure\_url ​should return ​HTTP​ status ​200 ​and Merchant should save the payload in DB, If ​disclosure\_url ​doesn’t return​ HTTP ​status​ 200 ​then user will be redirected to Ottu response page and Transaction details will be displayed. Ottu GET to ​redirect\_url ​with 2 parameters, ​order\_no​ as ​TT01 ​and ​reference\_number 45FGX​,​ ​now on ​redirect\_url merchant will fetch the data from DB and based on the ​result ​field customer will be redirected to success or failure page (created by Merchant).

### <mark style="color:blue;"></mark>[Post response](payment-request.md#post-response)

```json
{
   "full_name":"Saifuddin Tahir",
   "customer_email":"​saifuddin@kuwaitnet.com​",
   "customer_phone":"96597117060",
   "Gateway_name":"kpay",
   "order_no":"TT01",
   "reference_number":"45FGX",
   "gateway_response":{
      "result":"CAPTURED",
      "postdate":"0531",
      "ref":"715014026964",
      "trackid":"45FGX",
      "tranid":"1870568381471500",
      "udf1":"TT01",
      "udf3":"Saifuddin Tahir",
      "udf2":"​saifuddin@kuwaitnet.com​",
      "eci":"7",
      "responsecode":"00",
      "paymentid":"8972815371471500",
      "auth":"442382",
      "udf4":"96597117060"
   },
   "amount":"3.000",
   "result":"success",
   "gateway_account":"kpay",
   "currency_code":"KWD"
}
```
