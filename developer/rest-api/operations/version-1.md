# Version 1

## [Getting started](version-1.md#getting-started)

Through Ottu, merchants can perform operations such as capture, refund, and void across different payment gateways.

There are conditions should be applied to perform operations, in addition, not all the payment gateways support all the operations. See [operation definitions and conditions](../../../user-guide/payment-gateway.md#operation-definitions-and-conditions).

## <mark style="color:blue;">****</mark>[**Authentication**](version-1.md#authentication)

[Basic authentication](../authentication.md#basic-authentication)

## <mark style="color:blue;">****</mark>[**Capture**](version-1.md#capture)

#### [Endpoint](version-1.md#endpoint)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v1/operation/{{order_no}}
```

#### ****[**Request parameters**](version-1.md#request-parameters)

“operation”\
“[amount](../checkout-api.md#amount-string-required)”

#### ****[**Body**](version-1.md#body)

```json
{
   "operation":"capture",
   "amount":""
}
```

## <mark style="color:blue;">****</mark>[**Refund**](version-1.md#refund)

#### [Endpoint](version-1.md#endpoint-1)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v1/operation/{{order_no}}
```

#### ****[**Request parameters**](version-1.md#request-parameters-1)

“operation”\
****“[amount](../checkout-api.md#amount-string-required)”

#### ****[**Body**](version-1.md#body-1)****

```json
{
   "operation":"refund",
   "amount":""
}
```

## <mark style="color:blue;">****</mark>[**Cancel**](version-1.md#cancel)

#### [Endpoint](version-1.md#endpoint-2)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v1/operation/{{order_no}}
```

#### ****[**Request parameters**](version-1.md#request-parameters-3)****

"operation"

#### ****[**Body**](version-1.md#body-2)****

```json
{
   "operation":"cancel"
}
```

## ****[**Inquiry**](version-1.md#inquiry)

#### [Endpoint](version-1.md#endpoint-3)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v1/inquiry/{{order_no}}
```

#### ****[**Request parameters**](version-1.md#request-parameters-3)****

No request parameters needed.

#### [disclose\_to\_merchant](version-1.md#disclose\_to\_merchant-bool-optional) _<mark style="color:blue;">`bool`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">`optional`</mark>_

If True, the merchant will receive a disclosure request.

#### [disclosure\_url  ](version-1.md#disclosure\_url-url-optional)_<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> _<mark style="color:red;">`optional`</mark>_

Where the request would be sent to.

#### ****[**Body**](version-1.md#body-3)****

This request does not have a required body.

{% hint style="info" %}
Inquiry enabled when payment transaction state is either pending, attempted ,failed or expired. See [payment transaction state](../../../user-guide/payment-tracking.md#states-of-parent-payment-transaction).\
**Error message:**

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
