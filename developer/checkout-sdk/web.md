# Web

{% hint style="warning" %}
the Checkout SDK requires the implementation of the [Checkout API](../rest-api/checkout-api.md) in order to function properly.
{% endhint %}

## ****[Checkout JS](web.md#checkout-js)

### [Installation](web.md#installation)

```html
<head>
    <script src="https://assets.ottu.net/checkout/v2/checkout.min.js" 
     data-error="errorCallback" data-cancel="cancelCallback"
     data-success="successCallback" 
     data-beforeredirect="beforeRedirect"></script>HTML
 </head>
```

### [Functions](web.md#functions)

#### [Checkout.init](web.md#checkout.init)

To initiate checkout SDK, which will generate the SDK HTML

#### ****[**Parameters description**](web.md#parameters-description)****

#### ****[**selector **<mark style="color:blue;">****</mark> ](web.md#selector-string)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Should be same ID of checkout.&#x20;

#### ****[**merchant\_id**](web.md#merchant\_id-string)****[ **** ](web.md#amount-string-required)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Merchant domain. \
If the URL is https://example.ottu.com, the merchant\_id will be **example.ottu.com**.

#### ****[**apiKey**](web.md#apikey)****

API [Public key](../rest-api/authentication.md#public-key) should be used.

#### ****[**session\_id**](web.md#session\_id)****

It is generated when payment was created. See checkout API [Session ID](../rest-api/checkout-api.md#session\_id-string-read-only).

#### **l**[**ang**](web.md#lang)****

Language used. See checkout API [language](../rest-api/checkout-api.md#language-string-optional).

#### [Example](web.md#example)

<mark style="color:blue;">**HTML**</mark>

```javascript
<div id="checkout"></div>
```

<mark style="color:blue;">**js**</mark>

```javascript
Checkout.init({
   "selector":"checkout",
   "merchant_id":"",
   "session_id":"",
   "apiKey":"",
   "lang":"en"
});
```

#### [Checkout.reload](web.md#checkout.reload)

To refresh the SDK.\
It is useful when we get an error and want to refresh the content.

### [Callbacks](web.md#callbacks)

#### [window.errorCallback](web.md#window.errorcallback)

The error callback is invoked when problems occur during a payment.\
It must be defined using the data-error attribute on the Checkout script tag. \
The attribute value may be the name of a global function or a URL string.\
When a URL is provided, the browser will be redirected to the new page with a query parameter appended for each argument.

#### ****[**Parameters description**](web.md#parameters-description-1)****

#### ****[**status**](web.md#status-string) **** _<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Could be (“created”,  “in\_process”, “canceled”, “success” or “error”).&#x20;

#### ****[**message**](web.md#messagestring)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

It is the returned string**.**

#### [Example](web.md#example-1)

```javascript
window.errorCallback = function ("data)"{
   "data"{
      "status":"error",
      "message":"create_session error."
   }
}
  // example
  // redirect to checkout page

```

#### [window.cancel.Callback](web.md#window.cancel.callback)

Called when a customer cancels the payment.

#### ****[**Parameters description**](web.md#parameters-description-2)****

#### ****[**session\_id** ](web.md#session\_id-1)

It is generated when payment was created. See checkout API [Session ID](../rest-api/checkout-api.md#session\_id-string-read-only)&#x20;

#### ****[**status**](web.md#status-string-1)****[ **** ](web.md#amount-string-required)_<mark style="color:blue;">`string`</mark>_

&#x20;<mark style="color:blue;"></mark> Could (“created”,  “in\_process”, “canceled”, “success” or “error”).&#x20;

#### ****[**redirect\_url** ](web.md#redirect\_url)

Where the customer gets navigated to, after the payment process ends. See check out API [redirect\_url](../rest-api/checkout-api.md#redirect\_url-url-optional).

#### ****[**order\_no**](web.md#order\_no)****

A number for transaction provided by merchant. See checkout API [order\_no](../rest-api/checkout-api.md#order\_no-string-optional).

#### [operation **** ](web.md#operation-string)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Either pay or authorized.

#### ****[**reference\_number**](web.md#reference\_numberstring)_<mark style="color:blue;">`string`</mark>_

Transaction reference number.

#### ****[**message**](web.md#messagestring-1)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

It is the returned string**.**

#### [Example](web.md#example-2)

```javascript
window.cancelCallback = function (data) {
      data
       {
       "status": "canceled",
       "message": "Payment operation completed successfully.",
       "session_id": "",
       "order_no": "",
       “operation”: "pay",
       "reference_number": "",
       "redirect_url": "",
       }
      // example
      Checkout.reload();
}
```

#### [window.success.Callback](web.md#window.success.callback)

Called when the payment completed successfully.

#### [Parameters description](web.md#parameters-description-3)

#### ****[**message**](web.md#messagestring-2)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

It is the returned string**.**

#### ****[**session\_id**](web.md#session\_id-2)&#x20;

It is generated when payment was created. See checkout API [Session ID](../rest-api/checkout-api.md#session\_id-string-read-only).

#### <mark style="color:blue;">****</mark>[ **** ](web.md#amount-string-required)****[**status** ](web.md#status-string-2)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Could (“created”,  “in\_process”, “canceled”, “success” or “error”).&#x20;

#### ****[**redirect\_url** ](web.md#redirect\_url-1)

Where the customer gets navigated to, after the payment process ends. See check out API [redirect\_url](../rest-api/checkout-api.md#redirect\_url-url-optional).

#### ****[**order\_no**](web.md#order\_no-1)****

A number for transaction provided by merchant. See checkout API [order\_no](../rest-api/checkout-api.md#order\_no-string-optional).

#### [operation](web.md#operation-string-1)[ **** ](web.md#amount-string-required)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Either pay or authorized.

#### ****[**reference\_number**](web.md#reference\_numberstring-1)_<mark style="color:blue;">`string`</mark>_

Transaction reference number.

#### ****[**Example**](web.md#example-3)****

```javascript
window.successCallback = function (data) {
         data
      {
        "status": "success",
        "message": "Payment operation completed successfully.",
        "session_id": "",
        "order_no": "",
        "operation": "pay",
        "reference_number": "",
        "redirect_url": "",
       }
      // example
      window.location.href = data.redirect_url
}
```

#### [window.beforeRedirect](web.md#window.beforeredirect)

return new Promise(function(resolve, reject), It is a helper function that has to return a **promise** object, to create the[ **redirect\_url**](../rest-api/checkout-api.md#redirect\_url-url-optional)**.**\
****This allows the merchant to redirect the user to the cart page and wait for a while before creating the [**redirect\_url.** ](../rest-api/checkout-api.md#redirect\_url-url-optional)\
In case the customer changes items in the cart, the due amount will be updated accordingly, then the merchant will wait for a while until the customer does not return, then the function returns a **promise** object, the cart will be frozen and marked as submitted, and the[ **redirect\_url**](../rest-api/checkout-api.md#redirect\_url-url-optional) **** will be generated.

#### [Parameters description](web.md#parameters-description-4)

#### ****[**status**](web.md#status-string-3)****[ **** ](web.md#amount-string-required)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

Could (“created”,  “in\_process”, “canceled”, “success” or “error”).&#x20;

#### ****[**message**](web.md#messagestring-3)_<mark style="color:blue;">`string`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark>&#x20;

It is the returned string**.**

#### ****[**redirect\_url**](web.md#redirect\_url-2) ****&#x20;

Where the customer gets navigated to, after the payment process ends. See check out API [redirect\_url](../rest-api/checkout-api.md#redirect\_url-url-optional).

#### [Example](web.md#example-4)

<pre class="language-javascript"><code class="lang-javascript"><strong>window.beforeRedirect = function (data) {
</strong>      data
       {
        “status”: “success”,
        “message”: "Redirecting to the payment page...",
        “redirect_url”: “"
       }
      // example
      return new Promise(function (resolve, reject) {
        setTimeout(function () {
          resolve(true);
        }, 2000);
      });
}
</code></pre>

### [Full example](web.md#full-example)

```javascript
// HTML
    
    <div id="checkout"></div>
    
    <script src='./checkout.min.js' 
        data-error="errorCallback", 
        data-success="successCallback", 
        data-cancel="cancelCallback">
        
    
    </script>
// JS
    
    // Error callback function
    // Possible values:
    error:    
    {
        "status": "error",
        "message": "create_session error."
    }

    window.errorCallback = function(error) {
        console.log('applw pay error callback',error)
        if (error.redirect_url)
            window.location.href = error.redirect_url
    }
// JS
    
    // Success callback function   
    // Possible values:
    success:
    {
        "status": "success",
        "message": "Payment operation completed successfully.",
        "session_id": "",
        "order_no": "",
        "operation": "pay",
        "reference_number": ""
        "redirect_url": ""
    }
    
    
    window.successCallback = function(success) {
        window.location.href = success.redirect_url
        if(success.data.redirect_url)
            window.location.href = success.data.redirect_url
    }
// JS
    
    // Cancel callback function 
    // Possible values:
    cancel:
    {
        "status": "canceled",
        "message": "payment operation is cancelled.",
        "session_id": "",
        "order_no": "",
        "operation": "pay",
        "reference_number": ""
        "redirect_url": ""
    }
    
    
    window.cancelCallback = function(cancel) {
        Checkout.reload()
        console.log('cancel callback', cancel)
    }
// JS
    
    // Checkout init function

    Checkout.init({
      selector: 'checkout', 
      merchant_id: 'ksa.ottu.dev',
      session_id: '51436d465f77e59242ef25f15409c2f23fe54761',
      apiKey: 'L0Fc5f81.dLqByodGesaD9pJdzoKpo6rP1FQBkVzr',
      lang: 'en', // en or ar default en
    });
```

## [Apple Pay](web.md#apple-pay)

Apple Pay will show automatically if the following conditions are being met:

* Customer has an Apple device which supports Apple Pay payments.
* The browser is safari, only for web payments in the mobile SDK it doesn’t matter.
* The customer has a wallet configured on his Apple Pay device.
* The customer has more than one active cards in the wallet.

### ****[**Integrate** Apple Pay](web.md#integrate-apple-pay)

{% hint style="info" %}
For [Apple Pay integration](web.md#apple-pay), you have to enable Apple Pay in capabilities in your project.\
If apple pay available, will show by default.
{% endhint %}
