# Ecommerce

## [Getting started](ecommerce.md#getting-started)

Don't integrate with this API; use checkout-API instead [checkout-api.md](../checkout-api.md "mention"). Ottu offers ecommerce plugin, which empowers merchants business and facilitates the payments against the products.

### [Base URLs](ecommerce.md#base-urls)

> <mark style="background-color:purple;">POST \<ottu-url>/pos/crt/</mark>

The variable URL is the installation domain. The URL must be secure, i.e.: https://...

## [Body parameters](ecommerce.md#body-parameters)

### [Inbound](ecommerce.md#inbound)

_<mark style="color:red;">**Required**</mark>_

#### **amount** _<mark style="color:blue;">`string`</mark>_&#x20;

Against currency type for the decimal places.

#### **gateway\_code** _<mark style="color:blue;">`string`</mark>_

Gateway account code defined in admin panel.

#### **currency\_code** _<mark style="color:blue;">`string`</mark>_

Letters only. ISO 4217 codes https://en.wikipedia.org/wiki/ISO\_4217. Validated against the gateway settings currencies list defined in admin. Max length: 3.

#### **capture\_address**_<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`(`</mark><mark style="color:red;">**Required**</mark><mark style="color:blue;">`, if the multi step is enabled)`</mark>_

&#x20;It will display the form fields if received True in a request payload and the child plugin of multi step is enabled.

#### **capture\_delivery**_<mark style="color:blue;">`bool`</mark>_ _<mark style="color:blue;">`(`</mark><mark style="color:red;">**Required**</mark><mark style="color:blue;">`,if multi step child plugin is enabled)`</mark>_

&#x20;It will display the Map to mark the location, upon accessing the payment request link by customer, when the multistep child plugin is enabled.

_<mark style="color:blue;">**Optional**</mark>_

#### **language** _<mark style="color:blue;">`string`</mark>_

ISO 639-2 language code. https://www.loc.gov/standards/iso639-2/php/code\_list.php. Max length: 2.

#### **order\_no** _<mark style="color:blue;">`string`</mark>_

Merchant unique identifier. Max length: 128.

#### **email\_payment\_details** _<mark style="color:blue;">`bool`</mark>_

Send email to the customer with payment result details. Validated against customer\_email.

#### **sms\_payment\_details**_<mark style="color:blue;">`bool`</mark>_

Send email to the customer with payment result details. Validated against customer\_phone.

#### **sms\_notification** _<mark style="color:blue;">`bool`</mark>_

Send payment link to customer to pay via SMS. Validated against customer\_phone.

#### **email\_notification** _<mark style="color:blue;">`bool*`</mark>_

Send payment link to customer to pay via email. Validated against customer\_email.

#### **customer\_email**&#x20;

Customer email. Validated against customer\_email

#### **customer\_phone**_<mark style="color:blue;">`string`</mark>_

&#x20;International phone number, with prefix. Max length: 16.

#### **customer\_first\_name** _<mark style="color:blue;">`string`</mark>_

#### **customer\_last\_name**_<mark style="color:blue;">`string`</mark>_

#### **customer\_address\_line1** _<mark style="color:blue;">`string`</mark>_&#x20;

no specific limit.

#### **customer\_address\_line2**_<mark style="color:blue;">`string`</mark>_

no specific limit.

#### **customer\_address\_city** _<mark style="color:blue;">`string`</mark>_

&#x20;Add a default address from admin. Max length: 40.

#### **customer\_address\_state** _<mark style="color:blue;">`string`</mark>_&#x20;

Max length: 40.

#### **customer\_address\_country** _<mark style="color:blue;">`string`</mark>_&#x20;

ISO 3166-1 alpha-2 – two-letter country codes https://en.wikipedia.org/wiki/ISO\_3166-1. Max length: 2.

#### **customer\_address\_postal\_code** _<mark style="color:blue;">`integer`</mark>_&#x20;

&#x20;Max length: 12.

#### **vendor\_name** _<mark style="color:blue;">`string`</mark>_&#x20;

Vendor name is mapped to udf6 for kpay or knet. Max length: 64.

#### **email\_recipients** _<mark style="color:blue;">`string`</mark>_&#x20;

Comma separated emails. Recipients to receive emails, beside customer\_email.

#### **product\_type** _<mark style="color:blue;">`string`</mark>_&#x20;

Product information. Max length: 128.

#### **redirect\_url** _<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`(optional, if is set in admin control)`</mark>_&#x20;

URL where the user to be redirected after payment process has completed. Note that in includes in query string order\_no and reference\_number. Can be set in admin panel also. Max length: 200.

#### **disclosure\_url**_<mark style="color:blue;">`string`</mark>_ _<mark style="color:blue;">`(optional, if is set in admin control)`</mark>_&#x20;

URL where to send the payment result details. Must return a http status 200, in order to proceed with redirection to redirect\_url.\
Can also be set in admin panel. Max length: 200.

#### **extra**_<mark style="color:blue;">`key value pair {}`</mark>_

Anything sent here will be sent back as a query string when redirecting the customer back to the merchant website. Must be valid JSON.

### [Outbound](ecommerce.md#outbound)

_<mark style="color:red;">**Required**</mark>_

#### **amount** _<mark style="color:blue;">`string`</mark>_

#### **result** _<mark style="color:blue;">`string`</mark>_&#x20;

&#x20;Payment status, which can be "success", "canceled", "failed" or "error".

#### **reference\_number** _<mark style="color:blue;">`string`</mark>_

&#x20;Ottu unique identifier.

#### **order\_no** _<mark style="color:blue;">`string`</mark>_

#### **customer\_first\_name**  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_last\_name**  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_phone**  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_email**  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_address\_line1**  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_address\_line2**  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_address\_city**  _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_address\_state** _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_address\_postal\_code** _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **customer\_address\_line2** _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

#### **vendor\_name** _<mark style="color:blue;">`string`</mark>_ _<mark style="color:red;">**Required**</mark>`, if present in inbound payload`_

_<mark style="color:blue;">**Optional**</mark>_

#### **currency\_code**_<mark style="color:blue;">`string`</mark>_

#### **message** _<mark style="color:blue;">`string`</mark>_

&#x20;Error message in case of a failed payment.

#### **bulk\_id** _<mark style="color:blue;">`string`</mark>_

## [Example of Knet request](ecommerce.md#example-of-knet-request)

Merchant sends a request to the API endpoint

> <mark style="background-color:purple;">​https://pay.{yourdomain}/pos/d/crt/​</mark>

where Ottu is installed.

### [Post request](ecommerce.md#post-request)

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
   "disclosure_url":"​http://postapp.knpay.net/disclose_ok/"
}
```

## [Response disclosure URL](ecommerce.md#response-disclosure-url) <a href="#response-disclosure-url" id="response-disclosure-url"></a>

Ottu will respond back the Payment transaction details in POST request to this URL ​disclosure\_url​ specified by the merchant while initiating the payment request, ​disclosure\_url ​should return ​HTTP​ status ​200 ​and Merchant should save the payload in DB, If ​disclosure\_url ​doesn’t return​ HTTP ​status​ 200 ​then user will be redirected to Ottu response page and Transaction details will be displayed. Ottu GET to ​redirect\_url ​with 2 parameters, ​order\_no​ as ​TT01 ​and ​reference\_number 45FGX​,​ ​now on ​redirect\_url merchant will fetch the data from DB and based on the ​result ​field customer will be redirected to success or failure page (created by Merchant).

### [Post response](ecommerce.md#getting-started) <a href="#post-response" id="post-response"></a>

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
