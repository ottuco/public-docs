# Callbacks

In the Checkout SDK, callback functions are essential for providing real-time updates on the status of payment transactions.

These `callbacks` improve the user experience by enabling seamless and efficient handling of different payment scenarios, including:

* Successful payments
* Transaction cancellations
* Errors encountered during the payment process

All the callbacks described below can be triggered for any type of payment.

## [**errorCallback**](callbacks.md#errorcallback)

The `errorCallback` function is triggered when an issue occurs during the payment process. Proper error handling is essential to ensure a smooth user experience.

**Best Practice for Handling Errors**

In the event of an error, the recommended approach is to restart the checkout process by generating a new `session_id` through the [Checkout API](../../checkout-api.md).

**Defining the** `errorCallback` **Function**

The `errorCallback` function can be defined using the `data-error` attribute on the Checkout script tag. This attribute allows the specification of a global function to handle errors.

When an **error occurs**, the **`errorCallback`** function is invoked with a **`data` JSON object**, where **`data.status`** is set to **`error`**.

**Params Available in** `data` **`JSONObject` for** `errorCallback`

* `message` mandatory
* `form_of_payment` mandatory
* `status` mandatory
* `challenge_occurred` optional
* `session_id` optional
* `order_no` optional
* `reference_number` optional

## [**cancelCallback**](callbacks.md#cancelcallback)

The `cancelCallback` function in the [Checkout SDK](../) is triggered when a payment is canceled.

**Defining the** `cancelCallback` **Function**

The `cancelCallback` function can be defined using the `data-cancel` attribute on the Checkout script tag. This attribute allows the specification of a global function to handle cancellations.

**Invocation of** `cancelCallback`

If a customer cancels a payment, the `cancelCallback` function is invoked with a `data` JSON object, where `data.status` is set to `canceled`.

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

In the [Checkout SDK](../), the `successCallback` function is triggered upon the successful completion of the [payment process](../#ottu-checkout-sdk-flow).

**Defining the `successCallback` Function**

The `successCallback` function is defined and assigned as the value of the `data-success` attribute within the Checkout script tag.

**Invocation of** `successCallback`

When a payment is successfully processed, the `successCallback` function is invoked with a `data` JSON object, where `data.status` is set to `success`.

**Params Available in** `data` **`JSONObject` for** `successCallback`

* `message` mandatory
* `form_of_payment`mandatory
* `challenge_occurred` optional
* `session_id` optional
* `status` mandatory
* `order_no` optional
* `reference_number` optional
* `redirect_url` optional
* `payment_gateway_info` optional
