# Version 1

## [Getting started](version-1.md#getting-started)

Through Ottu, merchants can perform operations such as capture, refund, and void across different payment gateways.

There are conditions should be applied to perform operations, in addition, not all the payment gateways support all the operations. See [operation definitions and conditions](../../../user-guide/payment-gateway.md#operation-definitions-and-conditions).

## <mark style="color:blue;">****</mark>[**Capture**](version-1.md#capture)

{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v1/operation/{{order_no}}" summary="To convert an authorized payment transaction into an actual payment" %}
{% swagger-description %}
The process of obtaining a complete or partial authorized payment and depositing the captured amount into the merchant's bank account.

\


The payment transaction should be authorized.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](../authentication.md#basic-authentication)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
capture
{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" required="true" %}
Capture amount
{% endswagger-parameter %}
{% endswagger %}

## <mark style="color:blue;">****</mark>[**Refund**](version-1.md#refund)

{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v1/operation/{{order_no}}" summary="To returne money to a customer" %}
{% swagger-description %}
The process of refunding involves deducting either the full or partial amount that was paid or captured and then transferring it back to the customer's bank account.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](../authentication.md#basic-authentication)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
refund
{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" required="true" %}
Refund amount
{% endswagger-parameter %}
{% endswagger %}

## <mark style="color:blue;">****</mark>[**Cancel**](version-1.md#cancel)

{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v1/operation/{{order_no}}" summary="To cancel the payment transaction " %}
{% swagger-description %}
To halt a transaction before it is fully processed.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](../authentication.md#basic-authentication)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
cancel
{% endswagger-parameter %}
{% endswagger %}

## ****[**Inquiry**](version-1.md#inquiry)

{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v1/inquiry/{{order_no}}" summary="To retrieve payment transaction details " %}
{% swagger-description %}
The process of checking the status of a payment transaction
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](../authentication.md#basic-authentication)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="disclose_to_merchant" required="false" type="bool" %}
If True, the merchant will receive a disclosure request
{% endswagger-parameter %}

{% swagger-parameter in="body" name="disclosure_url" type="URL" %}
Where the request would be sent to
{% endswagger-parameter %}
{% endswagger %}

Inquiry enabled when payment transaction state is either pending, attempted ,failed or expired. See [payment transaction state](../../../user-guide/payment-tracking.md#states-of-parent-payment-transaction).\
**Error message:**

{% hint style="info" %}
1. If there is no transaction with provided order number:\
   "**Order number does not exist**"
2. If there is no transaction with provided order number but txn have no attempts: "**payment attempts for order {your order number}**"
3. If disclose\_to\_merchant is True and disclosure\_url isn't defined: \
   "**No disclosure url found for order {txn order number}**"
4. If operation isn't allowed: \
   "**Operation is not allowed**"
{% endhint %}

{% hint style="warning" %}
[Operations](./) are not working for foreign [currenies](../../../user-guide/currencies.md).&#x20;
{% endhint %}
