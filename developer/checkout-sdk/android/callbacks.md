# Callbacks

In the Checkout SDK, callback functions are essential for delivering real-time updates on the status of payment transactions. These callbacks improve the user experience by facilitating smooth and effective management of different payment scenarios, including errors, successful transactions, and cancellations.&#x20;

{% hint style="info" %}
The callbacks outlined below are applicable to any type of payment.
{% endhint %}

## [**errorCallback**](callbacks.md#errorcallback)

The `errorCallback` is a callback function triggered when issues occur during a payment process. Properly handling these errors is essential for maintaining a smooth user experience.&#x20;

{% hint style="info" %}
The best practice recommended in the event of an error is to restart the checkout process by generating a new `session_id` through the [Checkout API](https://docs.ottu.com/developer/checkout-api).
{% endhint %}

To set up the `errorCallback` function, use the `data-error` attribute on the Checkout script tag to designate a global function that will manage errors. If an error arises during a payment, the `errorCallback` function will be called, receiving a `JSONObject` with a `data.status` value indicating an error.

**Params Available in** `data` **`JSONObject` for** `errorCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `status` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `order_no` optional
* `reference_number` optional

## [**cancelCallback**](callbacks.md#cancelcallback)

The `cancelCallback` is a callback function in the Checkout SDK that is activated when a payment is canceled.&#x20;

To configure the `cancelCallback` function, you can use the `data-cancel` attribute on the Checkout script tag to specify a global function that will handle cancellations. If a payment is canceled by a customer, the `cancelCallback` function will be called, and it will receive a `JSONObject` containing a `data.status` value of "canceled".

**Params Available in** `data` **`JSONObject` for** `cancelCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `status` mandatory
* `order_no` optional
* `reference_number` optional
* `payment_gateway_info` optional

{% hint style="warning" %}
In both `cancelCallback` and `errorCallback`, the SDK must be reinitialized, either on the same session or on a new session.
{% endhint %}

## [**successCallback**](callbacks.md#successcallback)

The `successCallback` is a function that is triggered when the payment process is successfully completed. This callback receives a `JSONObject` containing a `data.status` value of "success."

**Params Available in** `data` `JSONObject` for `successCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `status` mandatory
* `order_no` optional
* `reference_number` optional
* `redirect_url` optional
* `payment_gateway_info` optional

The `successCallback` function is defined and assigned by setting the `data-success` attribute on the Checkout script tag. This attribute specifies a global function that will be invoked when the payment process successfully completes.
