# Version 2

## [Getting started](version-2.md#getting-started)

Through Ottu, merchants can perform operations such as capture, refund, and void across different payment gateways.

There are conditions should be applied to perform operations, in addition, not all the payment gateways support all the operations. See [operation definitions and conditions](../../../user-guide/payment-gateway.md#operation-definitions-and-conditions).

## <mark style="color:blue;">****</mark>[**Authentication**](version-2.md#authentication)

[API Private key.](../authentication.md#private-key)

## ****[**Capture**](version-2.md#capture)****

It works with one of the two parameters, [**order\_no**](../checkout-api.md#order\_no-string-optional) **** or [**session\_id**](../checkout-api.md#session\_id-string-read-only)**.**

#### [Endpoint](version-2.md#endpoint)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v2/operation
```

#### ****[**Request parameters**](version-2.md#request-parameters-2)

“operation”\
“[order\_no](../checkout-api.md#order\_no-string-optional)” **Or** “[session\_id](../checkout-api.md#session\_id-string-read-only)” \
“[amount](../checkout-api.md#amount-string-required)”

#### [Body](version-2.md#body-4)

```json
{

    "operation": "capture",
    "order_no": "", // or "session_id",
    "amount": ""  
}
```

## ****[**Refund**](version-2.md#refund)****

It works with one of the two parameters [**order\_no**](../checkout-api.md#order\_no-string-optional) or [**session\_id**](../checkout-api.md#session\_id-string-read-only)**.**

#### [Endpoint](version-2.md#endpoint-1)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v2/operation
```

#### ****[**Request parameters**](version-2.md#request-parameters-3)****

“operation” \
“[order\_no](../checkout-api.md#order\_no-string-optional)” **Or** “[session\_id](../checkout-api.md#session\_id-string-read-only)” \
“[amount](../checkout-api.md#amount-string-required)”

#### ****[**Body**](version-2.md#body-5)****

```json
{
    "operation": "refund",
    "order_no": "", // or "session_id"    
    "amount": ""
}
```

## <mark style="color:blue;">****</mark>[**Void**](version-2.md#void)

It works with one of the two parameters,  [**order\_no**](../checkout-api.md#order\_no-string-optional) or [**session\_id**](../checkout-api.md#session\_id-string-read-only)**.**

#### [Endpoint](version-2.md#endpoint-2)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v2/operation
```

#### ****[**Request parameters**](version-2.md#request-parameters-4)

“operation” \
“[order\_no](../checkout-api.md#order\_no-string-optional)” **Or** “[session\_id](../checkout-api.md#session\_id-string-read-only)”

#### ****[**Body**](version-2.md#body-6)****

```json
{
    "operation": "void",
    "order_no": "", // or "session_id"
}
```

## ****[**Cancel**](version-2.md#cancel)****

It works with one of the two parameters,  [**order\_no**](../checkout-api.md#order\_no-string-optional) or [**session\_id**](../checkout-api.md#session\_id-string-read-only)**.**

It is only for payment transaction whose state is either **created**, **pending**, **cod**, or **attempted**. See [payment transaction states](../../../user-guide/payment-tracking.md#states-of-parent-payment-transaction).

#### [Endpoint](version-2.md#endpoint-3)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v2/operation
```

#### ****[**Request parameters**](version-2.md#request-parameters)****

“operation” \
“[order\_no](../checkout-api.md#order\_no-string-optional)” **Or** “[session\_id](../checkout-api.md#session\_id-string-read-only)”

#### ****[**Body**](version-2.md#body-7)****

```json
{
    "operation": "cancel",
    "order_no": "",// or "session_id"	
}
```

## ****[**Inquiry**](version-2.md#inquiry)****

It works with one of the two parameters, **** [**order\_no**](../checkout-api.md#order\_no-string-optional) or [**session\_id**](../checkout-api.md#session\_id-string-read-only)**.**

#### [Endpoint](version-2.md#endpoint-4)

<mark style="color:green;">**POST:**</mark>

```url
 https://<ottu-url>/b/pbl/v2/inquiry
```

#### ****[**Request parameters**](version-2.md#request-parameters-6)

“[order\_no](../checkout-api.md#order\_no-string-optional)” **Or** “[session\_id](../checkout-api.md#session\_id-string-read-only)”

#### [disclose\_to\_merchant](version-2.md#disclose\_to\_merchant-bool-optional) _<mark style="color:blue;">`bool`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

If True, the merchant will receive a disclosure request.

#### [disclosure\_url  ](version-2.md#disclosure\_url-url-optional)_<mark style="color:blue;">`URL`</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">`optional`</mark>_

Where the request would be sent to.

#### ****[**Body**](version-2.md#body-8)****

```json
{
    "order_no": "   ",// or "session_id"
}
```

{% hint style="info" %}
Inquiry enabled when payment transaction state is either pending, attempted ,failed or expired. See [payment transaction state](../../../user-guide/payment-tracking.md#states-of-parent-payment-transaction).\


**Error message:**

1. If order\_number or session\_id isn't exist: \
   **"No payment attempts found for this Lookup"**
2. If there is no transaction with provided order number but transaction have no attempts:\
   "**payment attempts for order {your order number}**"
3. If [disclose\_to\_merchant](version-2.md#disclose\_to\_merchant-bool-optional) is True and [disclosure\_url](version-2.md#disclosure\_url-url-optional) isn't defined: \
   "**No disclosure url found for order {txn order number}**"
4. If operation isn't allowed: \
   "**Operation is not allowed**"
{% endhint %}

## ****[**Expire**](version-2.md#expire)****

It works with one of the two parameters, **** [**order\_no**](../checkout-api.md#order\_no-string-optional) or [**session\_id**](../checkout-api.md#session\_id-string-read-only)**.**\
****It is only for payment transaction whose state is either **created**, **pending**, or **attempted**. Furthermore, it is not applicable for payment transaction whose state is either **authorized** or any other subsequent payment transaction. See [payment transaction states](../../../user-guide/payment-tracking.md#states-of-parent-payment-transaction).

#### [Endpoint](version-2.md#endpoint-5)

<mark style="color:green;">**POST:**</mark>

```url
https://<ottu-url>/b/pbl/v2/operation
```

#### ****[**Request parameters**](version-2.md#request-parameters-6)****

“operation” \
“[order\_no](../checkout-api.md#order\_no-string-optional)” **Or** “[session\_id](../checkout-api.md#session\_id-string-read-only)”

#### ****[**Body**](version-2.md#body-9)****

```json
{
    "operation": "expire",
    "order_no": "",// or "session_id"	
}
```

{% hint style="warning" %}
[Operations](./) are not working for foreign [currenies](../../../user-guide/currencies.md).&#x20;
{% endhint %}
