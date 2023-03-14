# Web

The [Checkout SDK](./) is a JavaScript library provided by Ottu that allows you to easily integrate an Ottu-powered [checkout process](./#ottu-checkout-sdk-flow) into your web application. With the Checkout SDK, you can customize the look and feel of your checkout process, as well as which forms of payment are accepted.

To use the Checkout SDK, you'll need to include the library in your web application and initialize it with your Ottu [merchant\_id](web.md#merchant\_id-string), [session\_id](web.md#session\_id-string), and [API key](../rest-api/authentication.md#public-key). You can also specify additional options such as, which forms of payment to accept, the [css](web.md#css-object) styling for the checkout interface, and more.

{% hint style="warning" %}
Please note that the Checkout SDK requires the implementation of the [Checkout API](../rest-api/checkout-api.md) in order to function properly.
{% endhint %}

## [Checkout SDK](web.md#checkout-sdk)

## [Demo](web.md#demo)

Below is a demo of the Checkout SDK in action. This demo shows how the Checkout SDK can be used to create a streamlined checkout experience for customers, with support for multiple forms of payment and a customizable interface.

#### [Installation](web.md#installation)

To install the Checkout SDK, you'll need to include the library in your web application by adding a script tag to your HTML section. You can do this by using the following code snippet:

```html
<head>
    <script src="https://assets.ottu.net/checkout/v2/checkout.min.js" 
     data-error="errorCallback" data-cancel="cancelCallback"
     data-success="successCallback" 
     data-beforeredirect="beforeRedirect"></script>
 </head>
```

Replace [errorCallback](web.md#window.errorcallback), [cancelCallback](web.md#window.cancelcallback), [successCallback](web.md#window.successcallback), and [beforeRedirect](web.md#window.beforeredirect) with the names of your error handling, cancel handling, success handling, and before-redirect handling functions, respectively.

You're all set! You can now use the [Checkout SDK ](./)to create a checkout form on your web page and process payments through Ottu.

### [Functions](https://app.gitbook.com/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/141/developer/checkout-sdk/sdk#functions)

#### ****[**Checkout.init**](web.md#checkout.init)****

Is the function that initializes the [checkout process](./#ottu-checkout-sdk-flow) and sets up the necessary configuration options for the [Checkout SDK](./). It needs to be called once on your web page to initialize the checkout process, and it must be called with a configuration object that includes all the necessary options for the checkout process.

When you call `Checkout.init`, the SDK will take care of setting up the necessary components for the checkout process, such as creating a form for the customer to enter their payment details, and handling communication with Ottu's servers to process the payment.

#### ****[**Checkout.reload**](web.md#checkout.reload)****

The `Checkout.reload` function in the Checkout SDK is used to refresh the SDK. It's useful when you want to reload the **content** of the SDK after an **error** has occurred or when the content needs to be **refreshed**.

Here's an example of how `Checkout.reload` might be called:

```javascript
Checkout.reload();
```

In this example, the `Checkout.reload` function is called to refresh the content of the SDK. This can be useful when an error has occurred and the content needs to be reloaded or refreshed.

#### ****[**Properties** ](web.md#properties)****

#### ****[**selector**](web.md#selector-string)  **  **_<mark style="color:blue;">**`string`**</mark>_

The `selector` property in the Checkout SDK is used to specify the css selector for the HTML element that will contain the checkout form. This is typically a `<div>` element on your web page.

To specify the selector, you can add a `<div>` element to your web page with a unique `id` attribute, like this:

```html
<div id="checkout"></div>
```

In this example, the `id` attribute of the `<div>` element is set to `"checkout"`. This means that the `selector` property in `Checkout.init` should be set to `"checkout"`.

It's important to note that the `selector` property must be the ID of the HTML element that will contain the checkout form. This is because the Checkout SDK replaces the contents of the specified element with the checkout elements.

Here's an example of how `Checkout.init` might be called with a `selector` property:

```javascript
Checkout.init({
    selector: 'checkout', 
    ... // other properties
});
```

In this example, the `selector` property is set to `"checkout"`, which means that the checkout form will be contained in the `<div>` element with `id="checkout"`.

#### ****[**merchant\_id**](web.md#merchant\_id-string)  **  **_<mark style="color:blue;">**`string`**</mark>_

The `merchant_id` specifies your Ottu merchant domain. This should be the root domain of your Ottu account, without the "https://" or "http://" prefix.

For example, if your Ottu URL is `https://example.ottu.com`, then your `merchant_id` should be **example.ottu.com**. This property is used to identify which Ottu merchant account the checkout process should be linked to.

#### ****[**apiKey**](web.md#apikey)****

The `apiKey` is your Ottu [API public key](../rest-api/authentication.md#public-key). This key is used for authentication purposes when communicating with Ottu's servers during the checkout process.

According to the REST [API documentation](../rest-api/), the `apiKey` property should be set to your Ottu  [API public key](../rest-api/authentication.md#public-key).

#### ****[**session\_id**](web.md#session\_id-string) ** **_<mark style="color:blue;">**`string`**</mark>_

The `session_id` the unique identifier for the payment transaction associated with the checkout process.

This unique identifier is automatically generated when the payment transaction is created. For more information on how to use the `session_id` parameter in the Checkout API, see [session\_id](../rest-api/checkout-api.md#session\_id-string-read-only).

#### ****[**lang**](web.md#lang-string) ** **_<mark style="color:blue;">**`string`**</mark>_

The `lang` property is used to specify the language in which the checkout elements should be displayed. This property can be set to either `"en"` (for English) or `"ar"` (for Arabic).

When `lang` is set to `"en"`, the checkout form will be displayed in English, and when it's set to `"ar"`, the checkout elements will be displayed in Arabic. Additionally, when the `lang` parameter is set to `"ar"`, the layout will switch to right-to-left (RTL) to accommodate Arabic script. \
For more information on how to use lang parameter in the Checkout API, see [lang](../rest-api/checkout-api.md#language-string-optional).

#### ****[**formsOfPayment**](web.md#formsofpayment-array) ** **_<mark style="color:blue;">**`array`**</mark>_

`formsOfPayment` allows you to customize which forms of payment will be displayed in your checkout process. By default, all forms of payment are configured.

The available options for `formsOfPayment` are:

* `"applePay"`: The Apple Pay payment method that allows customers to make purchases using their Apple Pay-enabled devices.
* `"cardForm"`: A form that allows customers to enter their credit or debit card details to make a payment.
* `"tokenPay"`: A payment method that uses tokenization to securely store and process customers' payment information.
* `"redirect"`: A method where customers are redirected to a payment gateway or a third-party payment processor to complete their payment.

This property can be particularly useful when you want to customize the checkout process and display only specific forms of payment, such as only displaying the Apple Pay button and hiding the other payment options.

#### ****[**css** ](web.md#css-object) ** **_<mark style="color:blue;">**`object`**</mark>_

`css` can be used to override some of the elements rendered by the SDK to better integrate with your website.

There are several css classes that can be overridden using the [css](web.md#css-object) property, including:

* `.ottu__sdk-main`: the class of the div which wraps all the elements of the container. It's useful to override this when you want to modify the size of the rendered content by the SDK.
* `.ottu__sdk-apple-pay-button-type`: specifies the button type of the Apple Pay button. This class defaults to `pay` and if there is no card in the wallet, it will automatically change to `setUp`. More information can be found on the ApplePay official documentation for [ApplePay Button Type](https://developer.apple.com/documentation/apple\_pay\_on\_the\_web/applepaybuttontype).
* `.apple-pay-button`: the class of the Apple Pay button, which is a div. It can be customized for width, padding, margin, and more. Useful to customize when `formsOfPayment` is configured to display only the Apple Pay button.

Here's an example of how [Checkout.init](web.md#checkout.init) might be called with a  [css](web.md#css-object) property to customize the `.ottu__sdk-main` class, `.ottu__sdk-apple-pay-button-type` class, and `.apple-pay-button` class:

```javascript
Checkout.init({
    selector: 'checkout',
    merchant_id: 'domain',
    session_id: 'session_id',
    apiKey: 'api_key',
    formsOfPayment: ['applePay', 'cardForm', 'tokenPay', 'redirect'],
    css: `
    .ottu__sdk-main {
      flex-basis: 100%;
      justify-content: center;
      width: 150px!important;
      max-width: 150px!important;
    }
    .ottu__sdk-apple-pay-button-type {
      -apple-pay-button-type: check-out;
    }
    .apple-pay-button {
      width: 90%;
      padding-top: 0px;
      margin-top: 12px;
      margin-bottom: 12px;
    }
  `
});
```

In this example, the [css](web.md#css-object) property is set to override the `.ottu__sdk-main`, `.ottu__sdk-apple-pay-button-type`, and `.apple-pay-button` classes with new styles. The updated styles for these classes will be applied to the checkout container when it's rendered by the SDK.

#### [Example](web.md#example)

<mark style="color:blue;">**HTML**</mark>

```javascript
<div id="checkout"></div>
```

<mark style="color:blue;">**Javascript**</mark>

```javascript
Checkout.init({
    selector: "checkout",
    merchant_id: "",
    session_id: "",
    apiKey: "",
    lang: "en",
    formsOfPayment: ['applePay', 'cardForm', 'tokenPay', 'redirect'],
    css: `
        ottu__sdk-main {
        flex-basis: 100%;
        justify-content: center;
        width: 150px!important;
        max-width: 150px!important;
    }`
});
```

### [Callbacks](web.md#callbacks)

#### ****[**window.errorCallback**](web.md#window.errorcallback)****

The `errorCallback` is a callback function that is invoked when problems occur during a payment.

To define the `errorCallback` function, you can use the `data-error` attribute on the Checkout script tag to specify a global function that will handle errors. If an error occurs during a payment, the `errorCallback` function will be invoked with an `error` object.

The `error` object has the following properties:

* `status`: A string indicating the status of the payment transaction. The only value that will be passed to the `errorCallback` is `"error"`.
* `message`: A string containing a description of the error that occurred.

Here's an example of how `errorCallback` might be defined:

```html
<head>
    <script src="https://assets.ottu.net/checkout/v2/checkout.min.js" data-error="errorCallback"></script>
    <script>
        function errorCallback(error) {
          console.log(error);
          // handle error - redirect to payment page
          window.location.href = "https://payment.example.com";
        }
    </script>
</head>
```

In this example, the `errorCallback` function is defined and passed as the value of the `data-error` attribute on the Checkout script tag. If an error occurs during a payment, the function will be invoked with an `error` object. The function will then handle the error as needed and redirect the customer to `https://payment.example.com`.

{% hint style="info" %}
`errorCallback` function is not required to perform a redirection. It can handle errors in any way that is appropriate for your application.
{% endhint %}

#### ****[**window.cancelCallback**](web.md#window.cancelcallback)****

The cancelCallback in the Checkout SDK is a callback function that is invoked when a payment is canceled.

To define the `cancelCallback` function, you can use the `data-cancel` attribute on the Checkout script tag to specify a global function that will handle cancellations. If a customer cancels a payment, the `cancelCallback` function will be invoked with a [data object](web.md#data-object).

`cancelCallback` receives a [data object](web.md#data-object),  where the data.status is "canceled".

Here's an example of how `cancelCallback` might be defined:

```javascript
window.cancelCallback = function (data) {
    console.log(data);
    // handle cancellation
    Checkout.reload();
};
```

In this example, the `cancelCallback` function is defined and passed as the value of the `data-cancel` attribute on the Checkout script tag. If a customer cancels a payment, the function will be invoked with a [data object](web.md#data-object) containing information about the cancelled transaction. The function will then handle the cancellation as needed and refresh the Checkout SDK with [Checkout.reload()](web.md#checkout.reload).

#### ****[**window.successCallback**](web.md#window.successcallback)****

The `successCallback` in the Checkout SDK is a callback function that is invoked when the payment process has been completed successfully.

Here's an example of how `successCallback` might be defined:

```javascript
window.successCallback = function (data) {
    window.location.href = data.redirect_url;
};
```

`successCallback` receives a [data object](web.md#data-object), where the `data.status` is "success".

In this example, the `successCallback` function is defined and passed as the value of the `data-success` attribute on the Checkout script tag. If the payment process completes successfully, the function will be invoked with a [data object](web.md#data-object) containing information about the completed transaction. The function will then redirect the customer to the specified `redirect_url` using `window.location.href`.

#### ****[**Example**](https://app.gitbook.com/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/141/developer/checkout-sdk/sdk#example-1)****

```javascript
window.successCallback = function (data) {
    data = {
        "status": "success",
        "message": "Payment operation completed successfully.",
        "session_id": "",
        "order_no": "",
        "operation": "pay",
        "reference_number": "",
        "redirect_url": "https://payment.example.com/success"
    };

    window.location.href = data.redirect_url;
};
```

#### ****[**window.beforeRedirect**](web.md#window.beforeredirect)****

For `redirect` checkout processes, you may want to freeze the customer's basket before the customer is redirected to the payment page. The Checkout SDK provides a `beforeRedirect` callback that you can use to perform any necessary actions before the redirection occurs.

To define the `beforeRedirect` callback, you can use the `data-beforeredirect` attribute on the Checkout script tag to specify a global function that will handle the callback. This function should return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) that resolves when the necessary actions are complete.

Here's an example of how to define the `beforeRedirect` callback:

```html
<head>
    <script src="https://assets.ottu.net/checkout/v2/checkout.min.js" data-beforeredirect="beforeRedirect"></script>
    <script>
        window.beforeRedirect = function() {
            // Freeze the customer's basket while waiting for an API response
            return new Promise(function(resolve, reject) {
                // Send a request to your API to freeze the customer's basket
                fetch('https://api.yourdomain.com/basket/freeze', {
                        method: 'POST'
                    })
                    .then(function(response) {
                        // If the API response is successful, resolve the Promise
                        if (response.ok) {
                            resolve(true);
                        } else {
                            // If the API response fails, reject the Promise
                            reject(new Error('Failed to freeze basket'));
                        }
                    })
                    .catch(function(error) {
                        // If the API request fails, reject the Promise
                        reject(error);
                    });
            });
        }
    </script>
</head>
```

In this example, the beforeRedirect callback sends a request to an API endpoint to freeze the customer's basket while waiting for the redirection to occur. If the API response is successful, the Promise is resolved and the redirection proceeds. If the API request fails or the response is unsuccessful, the Promise is rejected and the redirection is cancelled.

#### ****[**data Object**](web.md#data-object)****

The data object received by the [cancelCallback](web.md#window.cancelcallback) and [successCallback](web.md#window.successcallback) contains information related to the payment transaction, such as the status of the payment process, the [session\_id](web.md#session\_id-string) generated for the transaction, any error message associated with the payment, and more. This information can be used to handle the payment process and take appropriate actions based on the status of the transaction.

<details>

<summary><a href="web.md#data-object-child-parameters">Data object child parameters</a></summary>

#### ****[**message**](web.md#messagestring)_<mark style="color:blue;">**`string`**</mark>_

Is a string message that can be displayed to the customer. It provides a customer-friendly message regarding the status of the payment transaction.

#### ****[**session\_id**](web.md#session\_id-string-1) ** **_<mark style="color:blue;">**`string`**</mark>_

`session_id` is a unique identifier generated when a payment transaction is created. It is used to associate a payment transaction with the checkout process. You can find the `session_id` in the response of the Checkout API's [session\_id](../rest-api/checkout-api.md#session\_id-string-read-only) endpoint. This parameter is required to initialize the Checkout SDK.

#### &#x20;**** [**status**](web.md#status-string) ** **_<mark style="color:blue;">**`string`**</mark>_

The status of the checkout process. Possible values are:

* `success`: The customer was charged successfully, and they can be redirected to a success page or display a success message.
* `canceled`: The payment was either canceled by the customer or rejected by the payment gateway for some reason. When a payment is canceled, it's typically not necessary to create a new payment transaction, and the same [session\_id](web.md#session\_id-string-1) can be reused to initiate the Checkout SDK and allow the customer to try again. By reusing the same session\_id, the customer can resume the checkout process without having to re-enter their payment information or start over from the beginning.
* `error`: An error occurred during the payment process, This can happen for a variety of reasons, such as a network failure or a problem with the payment gateway's system. The recommended action is to create a new payment transaction using the Checkout API and restart the checkout process.

#### ****[**redirect\_url** ](web.md#redirect\_url-url)_<mark style="color:blue;">**`URL`**</mark>_

`redirect_url`, is the URL which is provided in [Checkout API](../rest-api/checkout-api.md) for redirect\_url. See check out API [redirect\_url](../rest-api/checkout-api.md#redirect\_url-url-optional).

#### ****[**order\_no**](web.md#order\_no-string) ** **_<mark style="color:blue;">**`string`**</mark>_

The order number provided in the [Checkout API](../rest-api/checkout-api.md). See checkout API [order\_no](../rest-api/checkout-api.md#order\_no-string-optional).

#### ****[**operation**](web.md#operation-string) ** **_<mark style="color:blue;">**`string`**</mark>_

This property indicates whether the payment was a direct charge (`pay`) or an authorization (`authorized`) of the payment amount.

* `pay`: the payment is immediately charged to the customer's account and the funds are transferred to the merchant.
* `authorized`: the payment amount is only reserved on the customer's account and not immediately charged. The payment can then be captured at a later time using the [capture API](../rest-api/operations/version-2.md#capture).

Note that authorization typically lasts for a limited time, after which it will expire and the reserved funds will be released back to the customer's account.

#### ****[**reference\_number**](web.md#reference\_numberstring)_<mark style="color:blue;">**`string`**</mark>_

A unique identifier associated with the payment process. It is sent to the payment gateway as a unique reference and can be used for reconciliation purposes.

</details>

#### ****

#### [Extended example](https://app.gitbook.com/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/141/developer/checkout-sdk/sdk#extended-example)

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
      merchant_id: 'sandbox.ottu.net',
      session_id: '51436d465f77e59242ef25f15409c2f23fe54761',
      apiKey: 'L0Fc5f81.dLqByodGesaD9pJdzoKpo6rP1FQBkVzr',
      lang: 'en', // en or ar default en
    });
```

### [Apple Pay](web.md#apple-pay)

If you have completed the [Apple Pay integration](web.md#apple-pay) between Ottu and Apple, the Checkout SDK will automatically make the necessary checks to display the Apple Pay button.

When you initialize the Checkout SDK with your [session\_id](web.md#session\_id-string) and payment gateway [codes](../rest-api/checkout-api.md#pg\_codes-list-required), the SDK will automatically verify the following conditions:

* When initializing the Checkout SDK, a [session\_id](web.md#session\_id-string) with a [pg\_codes](../rest-api/checkout-api.md#pg\_codes-list-required) that was associated with the Apple Pay Payment Service was supplied.
* The customer has an Apple device that supports Apple Pay payments.
* The browser being used supports Apple Pay.
* The customer has a wallet configured on their Apple Pay device.

If all of these conditions are met, the Apple Pay button will be displayed and available for use in your checkout flow. If the wallet is not configured, the Apple Pay button will still appear, but with [setUp type](https://developer.apple.com/documentation/passkit/pkpaymentbuttontype/setup). Clicking on the `setUp` button Apple Pay wallet on their device will open, allowing them to configure it and add payment cards.

By default, the type of the Apple Pay button is [pay](https://developer.apple.com/documentation/passkit/pkpaymentbuttontype/instore), which is used to initiate a payment. However, you can override the default button type using the css init property of the Checkout SDK.

#### [Customize Apple Pay button](web.md#customize-apple-pay-button)

{% hint style="warning" %}
If you're using only the Apple Pay button from the Checkout SDK and want to customize its appearance, it's important to follow the [Apple Pay guidelines](https://developer.apple.com/design/human-interface-guidelines/technologies/apple-pay/buttons-and-marks) to ensure that your design is consistent with Apple's requirements. Failure to follow the guidelines could result in your app being rejected or your developer account being banned by Apple.
{% endhint %}

It's the responsibility of the merchant to ensure that their use of the Apple Pay button follows Apple's guidelines, and Ottu cannot be held responsible for any issues that arise from non-compliance. If you have any questions or concerns about using the Apple Pay button, please consult the [Apple Pay guidelines](https://developer.apple.com/design/human-interface-guidelines/technologies/apple-pay/buttons-and-marks) or contact Apple directly for assistance.

If you only want to use Apple Pay with the Ottu Checkout SDK and control the other payment methods yourself, you can customize the Apple Pay button using the Checkout SDK's [formsOfPayment](web.md#formsofpayment-array) and [css](web.md#css-object) properties.

To display only the Apple Pay button with default css, use the following code:

```javascript
Checkout.init({
    ... // define the mandatory properties
   formsOfPayment: ["applePay"]
});
```

The [formsOfPayment](web.md#formsofpayment-array) property tells the Checkout SDK to render only the Apple Pay button. If you don't include this property, the SDK will render all available payment options.

To customize the Apple Pay button's appearance, you can use the [css](web.md#css-object) property. The example below adjusts the size of the button and centers it within the Checkout SDK container:

```javascript
Checkout.init({
    ... // define the mandatory properties
    formsOfPayment: ["applePay"],
    css: `
        .ottu__sdk-main {
            flex-basis: 100%;
            justify-content: center;
            width: 150px!important;
            max-width: 150px!important;
        }
        .ottu__sdk-apple-pay-button-type {
            -apple-pay-button-type: check-out;
        }
        .apple-pay-button {
            width:100%;
            padding-top:0px;
            margin-top:18px;
            margin-bottom:18px;
        }
    `
});
```

The Apple Pay button inside the Checkout SDK container can be customized using the [css](web.md#css-object) property by defining the following css classes:

* `.ottu__sdk-apple-pay-button-type`: This class determines the type of the Apple Pay button. For example, setting `-apple-pay-button-type: plain` will render a plain Apple Pay button, while setting it to `buy` or `donate` will render buttons with the corresponding labels.
* `.apple-pay-button`: This class sets the width, margin, and padding of the button.

By default, the width of the Apple Pay button is 90% of the Checkout SDK container width, with top and bottom margins of 12px. The Checkout SDK creates a containerized `div` with the css class `ottu__sdk-main` and places the Apple Pay button inside it. This container has no margin or padding added.

To learn more about the `css` property, see the [css](web.md#css-object).

<figure><img src="../../.gitbook/assets/Button_ (2).png" alt=""><figcaption></figcaption></figure>
