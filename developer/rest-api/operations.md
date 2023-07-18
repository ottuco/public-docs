---
description: Version 2
---

# Operations

## [Getting started](operations.md#getting-started)

Ottu offers a range of operations via Rest API for merchants to carry out across multiple payment gateways, including capturing, refunding, voiding, canceling, inquiring, and expiring transactions. Ottu is dedicated to continuous development and has recently launched version 2 of its operation API. Nonetheless, documentation for version 1 of the operation API remains accessible [here](http://127.0.0.1:5000/s/HliFFcthyaYAsSykrr31/)[Ottu Operation API Version 1](http://127.0.0.1:5000/s/HliFFcthyaYAsSykrr31/ "mention").\
There are conditions should be applied to perform operations, in addition, not all the payment gateways support all the operations. See [Operation Definitions & Conditions.](../../user-guide/payment-gateway.md#\_h9m0s92vp7g8)\
\


{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v2/operation" summary="Capture" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](authentication.md#basic-authentication)

 or 

[API Private key.](authentication.md#private-key)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
capture
{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" required="true" %}
The required 

[amount ](checkout-api.md#amount-string-required)

for capture
{% endswagger-parameter %}

{% swagger-parameter in="body" name=""order_no  " or "session_id"" required="true" %}
It works with one of the two parameters, 

[order_no](checkout-api.md#order_no-string-optional)

 or 

[session_id](checkout-api.md#session_id-string-read-only)


{% endswagger-parameter %}
{% endswagger %}

#### [Capture](operations.md#capture-1)

The capture operation is a payment processing function that allows merchants to capture authorized funds for payment transactions that have not yet been settled.\
\
Capture operation can only be performed on authorized payment transactions.\
Merchants can capture the authorized amount in full or partially. However, it is not possible to capture more than the authorized amount.

The capture operation creates a child payment transaction of the original payment transaction. This child payment transaction will contain the details of the capture operation. The child payment transaction is only created if the capture operation has succeeded. Child transaction can be tracked over [child transaction table](../../user-guide/payment-tracking.md#child-table-transaction).\
\
Payment gateways support capture operation: \[MPGS,Tabby].\
\


{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v2/operation" summary="Refund" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](authentication.md#basic-authentication)

 or 

[API Private key.](authentication.md#private-key)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
refund
{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" required="true" %}
The required 

[amount ](checkout-api.md#amount-string-required)

for refund
{% endswagger-parameter %}

{% swagger-parameter in="body" name=""order_no  " or "session_id"" required="true" %}
IIt works with one of the two parameters, 

[order_no](checkout-api.md#order_no-string-optional)

 or 

[sesssion_id](checkout-api.md#session_id-string-read-only)


{% endswagger-parameter %}
{% endswagger %}

#### [Refund](operations.md#refund-1)

The Payment Refund operation offered by Ottu enables merchants to refund previously captured or paid payment transactions. This allows for the transfer of funds back to the customer's account when required.

For authorized payment transactions, a capture should be performed beforehand, and the captured amount must be sufficient for the requested refund amount. \
For purchase payment transactions, the paid amount should be enough to cover the refund amount.

Refund can be performed in full or partial amounts. \
When a refund operation is performed, a child payment transaction of the original payment transaction is generated. This child transaction contains all the details related to the refund operation and is created only if the refund operation is successful. Child transaction can be tracked over [child transaction table](../../user-guide/payment-tracking.md#child-table-transaction).

Additionally, Ottu offers an operation approval feature that enables merchants to control the refund operation. A staff member is designated as a checker who can approve or reject any refund requests sent by other staff members. This ensures that refunds are only issued when authorized by a designated person, which helps prevent fraudulent or unauthorized refunds. See [operation request flow](../../user-guide/plugins/features/operation-request-flow.md).

&#x20;Payment gateways support refund operation: \[Benefit, FSS, Kpay, MPGS, MyFatoorah, NGenius , payuindia, QPay,Tabby].\
\


{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v2/operation" summary="Void" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](authentication.md#basic-authentication)

 or 

[API Private key.](authentication.md#private-key)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
void
{% endswagger-parameter %}

{% swagger-parameter in="body" name=""order_no  " or "session_id"" required="true" %}
It works with one of the two parameters, 

[order_no](checkout-api.md#order_no-string-optional)

 or 

[sesssion_id](checkout-api.md#session_id-string-read-only)


{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
// Some code{
    "amount": "19.00",
    "initiator": {},
    "is_sandbox": true,
    "operation": "void",
    "pg_code": "ksa-mpgs",
    "pg_response": {
        "authorizationResponse": {
            "cardLevelIndicator": "88",
            "cardSecurityCodeError": "M",
            "commercialCard": "888",
            "commercialCardIndicator": "3",
            "marketSpecificData": "8",
            "posData": "1025104006600",
            "posEntryMode": "812",
            "processingCode": "003000",
            "responseCode": "00",
            "returnAci": "8",
            "stan": "219376",
            "transactionIdentifier": "123456789012345",
            "validationCode": "6789"
        },
        "customer": {
            "firstName": "Example"
        },
        "device": {
            "browser": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36",
            "ipAddress": "161.129.70.6"
        },
        "gatewayEntryPoint": "WEB_SERVICES_API",
        "merchant": "TEST121234345656",
        "order": {
            "amount": 19.0,
            "certainty": "FINAL",
            "chargeback": {
                "amount": 0,
                "currency": "SAR"
            },
            "creationTime": "2023-07-18T10:54:50.397Z",
            "currency": "SAR",
            "id": "hotfix445FEF",
            "lastUpdatedTime": "2023-07-18T10:55:37.876Z",
            "merchantAmount": 19.0,
            "merchantCategoryCode": "1111",
            "merchantCurrency": "SAR",
            "reference": "hotfix445FEF",
            "status": "CANCELLED",
            "totalAuthorizedAmount": 0.0,
            "totalCapturedAmount": 0.0,
            "totalRefundedAmount": 0.0
        },
        "response": {
            "acquirerCode": "00",
            "acquirerMessage": "Approved",
            "cardSecurityCode": {
                "acquirerCode": "M",
                "gatewayCode": "MATCH"
            },
            "gatewayCode": "APPROVED"
        },
        "result": "SUCCESS",
        "sourceOfFunds": {
            "provided": {
                "card": {
                    "brand": "VISA",
                    "expiry": {
                        "month": "1",
                        "year": "39"
                    },
                    "fundingMethod": "DEBIT",
                    "nameOnCard": "Example",
                    "number": "450875xxxxxx1019",
                    "scheme": "VISA",
                    "storedOnFile": "NOT_STORED"
                }
            },
            "type": "CARD"
        },
        "timeOfLastUpdate": "2023-07-18T10:55:37.876Z",
        "timeOfRecord": "2023-07-18T10:55:37.838Z",
        "transaction": {
            "acquirer": {
                "batch": 20230718,
                "date": "0718",
                "id": "BSF_S2I",
                "merchantId": "121234345656",
                "transactionId": "123456789012345"
            },
            "amount": 19.0,
            "authorizationCode": "220466",
            "currency": "SAR",
            "id": "0_void",
            "receipt": "319910219376",
            "reference": "hotfix445FEF_0",
            "source": "INTERNET",
            "stan": "220466",
            "targetTransactionId": "1",
            "terminal": "bsf00001",
            "type": "VOID_AUTHORIZATION"
        },
        "version": "57"
    },
    "reference_number": "hotfix46ACQY",
    "result": "success",
    "source": "input",
    "success": true,
    "timestamp_utc": "2023-07-18 10:55:37",
    "txn": {
        "amount": "19.00",
        "currency_code": "SAR",
        "customer_email": "",
        "extra": {},
        "order_no": "",
        "state": "voided",
        "session_id": "a4c59ceba9b57b61670281a4900d2a70f2090d59"

```
{% endswagger-response %}
{% endswagger %}

#### [Void](operations.md#void-1)

Over void operation,  merchants can cancel an authorized payment transaction before any capture operation is performed. This means that the payment transaction will not be captured, and the customer will not be charged.

To perform the void operation, the payment transaction must be authorized, and no capture operation should have been previously performed.\
Partial voids are not applicable, and void can only be applied to the whole payment transaction.

Each void operation creates a child payment transaction of the original payment transaction, which will contain the details of the operation. Child transaction can be tracked over [child transaction table](../../user-guide/payment-tracking.md#child-table-transaction).

Ottu also provides an operation approval feature that allows merchants to manage void operations. The feature designates a staff member as a checker who can approve or reject any void requests sent by other staff members. This ensures that voids are only issued with authorization from a designated person, preventing fraudulent or unauthorized voids. See [operation request flow](../../user-guide/plugins/features/operation-request-flow.md).

&#x20;Payment gateway supports void operation: \[MPGS].\
\


{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v2/operation" summary="Cancel" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](authentication.md#basic-authentication)

 or 

[API Private key.](authentication.md#private-key)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
cancel
{% endswagger-parameter %}

{% swagger-parameter in="body" name=""order_no  " or "session_id"" required="true" %}
It works with one of the two parameters, 

[order_no](checkout-api.md#order_no-string-optional)

 or 

[session_id](checkout-api.md#session_id-string-read-only)


{% endswagger-parameter %}
{% endswagger %}

#### [Cancel](operations.md#cancel-1)

The Payment Cancel operation is used to cancel a payment transaction that is in the “created”, ”pending”, ”cod”, or “attempted” state. See [payment transaction states](../../user-guide/payment-tracking.md#states-of-parent-payment-transaction).\
If a payment needs to be canceled, the merchant can initiate a cancel operation to stop the payment from proceeding.

\
Once a payment has been canceled, it cannot be resumed or processed further, and it cannot be reversed once executed. Therefore, merchants should use this operation with caution and only cancel payments when necessary.

All payment gateways support cancel operation.\
\


{% swagger method="post" path="" baseUrl=" https://<ottu-url>/b/pbl/v2/inquiry" summary="Inquiry" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](authentication.md#basic-authentication)

 or 

[API Private key.](authentication.md#private-key)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="disclose_to_merchant" required="false" type="bool" %}
If True, the merchant will receive a disclosure request
{% endswagger-parameter %}

{% swagger-parameter in="body" name="disclosure_url" type="URL" %}
Where the request would be sent to
{% endswagger-parameter %}

{% swagger-parameter in="body" name=""order_no  " or "session_id"" required="true" %}
It works with one of the two parameters, 

[order_no](checkout-api.md#order_no-string-optional)

 or 

[session_id](checkout-api.md#session_id-string-read-only)


{% endswagger-parameter %}
{% endswagger %}

#### [Inquiry](operations.md#inquiry-1)

Payment inquiry operation is a functionality that enables merchants to retrieve information about a payment transaction. This information includes the transaction status, details, and other related information.

Inquiry operation is enabled when the payment transaction state is either pending, attempted, failed, or expired.

Merchants can also disclose the transaction data using the "disclose\_to\_merchant" parameter within the inquiry API request.\
Additionally, inquiry can be executed manually,  and it can be automated for its scheduling, when the transaction is in attempted state, to enable merchants to stay up to date with their payment transactions without manual intervention.

{% hint style="info" %}
Inquiry enabled when payment transaction state is either pending, attempted ,failed or expired. See [payment transaction state](../../user-guide/payment-tracking.md#states-of-parent-payment-transaction).\


**Error message:**

1. If order\_number or session\_id isn't exist: \
   **"No payment attempts found for this Lookup"**
2. If there is no transaction with provided order number but transaction have no attempts:\
   "**payment attempts for order {your order number}**"
3. If `disclose_to_merchant` is True and `disclosure_url` isn't defined: \
   "**No disclosure url found for order {txn order number}**"
4. If operation isn't allowed: \
   "**Operation is not allowed**"
{% endhint %}

\
\


{% swagger method="post" path="" baseUrl="https://<ottu-url>/b/pbl/v2/operation" summary="Expire" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}


[Basic authentication](authentication.md#basic-authentication)

 or 

[API Private key.](authentication.md#private-key)


{% endswagger-parameter %}

{% swagger-parameter in="body" name="operation" required="true" %}
expire
{% endswagger-parameter %}

{% swagger-parameter in="body" name=""order_no  " or "session_id"" required="true" %}
It works with one of the two parameters, 

[order_no](checkout-api.md#order_no-string-optional)

 or 

[session_id](checkout-api.md#session_id-string-read-only)


{% endswagger-parameter %}
{% endswagger %}

#### [Expire](operations.md#expire-1)

The payment expire operation via REST API is used to cancel a payment transaction that has been initiated but not completed within a certain period of time. This operation is only applicable to payment transactions whose state is either created, pending, or attempted.

The expire operation is triggered when a payment transaction has been initiated but has not been completed within the specified time limit. Once the payment transaction enters the expired state, it cannot be resumed or completed.

All payment gateways support cancel operation.

{% hint style="warning" %}
[Operations](operations.md) are not working for foreign [currenies](../../user-guide/currencies.md).&#x20;
{% endhint %}
