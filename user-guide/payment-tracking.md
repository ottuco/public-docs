# Payment Tracking

## [Overview](payment-tracking.md#overview)

Ottu dashboard is an intuitive workspace composed of a set of tools, to empower all Ottu clients\
to have a proper payment pipeline, create, monitor, and track payments, the dashboard greets clients with analytical information captured from payment workload(s), to give the client an insight about the payment pipeline and how could proceed with next-login.

## [Transaction table ](payment-tracking.md#transaction-table)

All transaction history and details are easily accessible for merchants using Ottu's transaction table, which is provided with each [plugin](plugins/) installed. \
Through the transaction table, users can proceed with payment [operations](../developer/rest-api/operations.md) such as void, refund, and capture.

{% hint style="info" %}
The transaction table is located in the transaction tab under each installed [plugin](plugins/).
{% endhint %}

### [Proxy fields ](payment-tracking.md#proxy-fields)

Ottu creates a transaction table for each [plugin](plugins/) in order to allow merchant(s) to monitor their transactions. \
Ottu offers an amazing method of adding or removing transaction table’s headers, by using this method, the merchant(s) are empowered to find the required information quickly and easily.&#x20;

#### [How to activate headers setting on the dashboard](payment-tracking.md#how-to-activate-headers-setting-on-the-dashboard)

&#x20;Ottu dashboard > administration panel> plugin config (the plugin of the targeted transaction table) >Tick on (**Individual proxy fields enabled**)

<figure><img src="../.gitbook/assets/activate proxy fields.png" alt=""><figcaption></figcaption></figure>

#### [Header list ](payment-tracking.md#header-list)

Ottu dashboard > Required Plugin Tab> Setting > Table headers By dragging the required header (right ⇆ left), the merchant(s) can add or remove the header.

<figure><img src="../.gitbook/assets/Add or remove headers (1).png" alt=""><figcaption></figcaption></figure>

#### [Child table transaction ](payment-tracking.md#child-table-transaction)

[Child transactions](payment-tracking.md#states-of-child-payment-transaction) are those created by subsequent operations (capture, refund, or void). \
They should be listed in the transaction table under the main origin payment transaction, which is called parent transaction.\
The headers of each child payment transaction are inherited from the headers of the parent payment transaction.

<figure><img src="../.gitbook/assets/Child proxy header (1).png" alt=""><figcaption></figcaption></figure>

### [Amount definitions and calculation mechanism](payment-tracking.md#amount-definitions-and-calculation-mechanism)&#x20;

Many headers can be added / removed in transaction tables. See [proxy fields](payment-tracking.md#proxy-fields-1) \
Amount headers are displayed in different titles based on where the header refers to the payment amount in the process flow.

#### [**Amount**:](payment-tracking.md#amount)

This is the initial amount at the creation of the transaction.

#### [**Settled amount:**](payment-tracking.md#settled-amount)&#x20;

**Is the amount with the same currency of the initiating amount,**

* **For editable amount:** It is the amount that the customer enters at the checkout page, If the customer pays the full required amount,  it will have the same value as the [initiating amount](payment-tracking.md#amount).
* **For  non-editable amount:** The settled amount is  the same value as the [initiating amount](payment-tracking.md#amount).

#### [**Paid amount:**](payment-tracking.md#paid-amount)&#x20;

It is the amount that is credited to the merchant's bank account.&#x20;

* **For purchase:** It is a settled amount after adding the fee.
* **For authorize:** Is the total amount captured by the staff.

{% hint style="info" %}
Purchase or authorized is determined in payment gateway setting or configuration.\
&#x20;See [PG configuration](payment-gateway.md#configure-payment-gateway).
{% endhint %}

#### [Remaining amount:](payment-tracking.md#remaining-amount)&#x20;

|[Amount](payment-tracking.md#amount) - [Settled amount](payment-tracking.md#settled-amount)|

#### [Fee:](payment-tracking.md#fee)&#x20;

It represents a markup amount on the [initiating amount](payment-tracking.md#amount), which the customer proceeds with.

{% hint style="info" %}
Depending on the selected merchant(s) [currency configuration](currencies.md#currency-configuration), which is used by the selected PG, fees may be applied or not to the transactions in the default currency or foreign currency.
{% endhint %}

#### [Refunded amount:](payment-tracking.md#refunded-amount)&#x20;

The amount, which is sent back to customer's bank account, which is deducted from the[ paid amount](payment-tracking.md#paid-amount).

#### [Voided amount: ](payment-tracking.md#voided-amount)

It is the full transaction amount, which is credited to the customer's bank account, when the transaction rolling back process gets done.\
It works only for authorized payment, which is not fully or partially captured.

{% hint style="info" %}
* **Capture,** can capture the entire amount, fee + settled amount.&#x20;
* **Refund,** can only refund the settled amount, that’s without fee.&#x20;
* **Void,** will void the full amount, including the fee.
{% endhint %}

## [Dashboard charts](payment-tracking.md#dashboard-charts)

Merchant(s) easily track and monitor overall transactions and sales progress all in one place, the following snapshots have been taken from the dashboard showing a number of usable charts.

### [Total transactions chart](payment-tracking.md#total-transactions-chart)

Represents gross volume and overall success rate across various periods.

![](<../.gitbook/assets/1 (2) (1).png>)

### [Average transaction size chart](payment-tracking.md#average-transaction-size-chart)

<figure><img src="../.gitbook/assets/AverageTransaction (1).png" alt=""><figcaption></figcaption></figure>

### [Product wise sales chart](payment-tracking.md#product-wise-sales-chart)

![](<../.gitbook/assets/3 (4).png>)

### [Recent orders dynamic report](payment-tracking.md#recent-orders-dynamic-report)

![](<../.gitbook/assets/4 (4) (1).png>)

## [ Payment transaction ](payment-tracking.md#payment-transaction)

Payment transaction is a core transactional data which is used by the merchant(s) to create a payment, and to perform certain operations like refund void, cancel .etc. \
**Payment transaction is divided into two types.**

{% hint style="info" %}
**Parent payment transaction:** Holds the data about the payment.
{% endhint %}

{% hint style="info" %}
**Child payment transaction:** Is created for subsequent operations triggered by the merchant(s).
{% endhint %}

### [Payment transactions states](payment-tracking.md#payment-transactions-states)

Payment transactions state are flags, which help merchant(s) to audit-trail the transaction process.

### [**States of parent payment transaction**](payment-tracking.md#states-of-parent-payment-transaction)

| <mark style="color:blue;">**state**</mark>      | <mark style="color:blue;">**state Description**</mark>                                                                                                                                                                                                                              | <mark style="color:blue;">**Actor**</mark> | <mark style="color:blue;">**Note(s)**</mark>                                                   |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**created**</mark>    | The payment has been created.                                                                                                                                                                                                                                                       | Merchant                                   |                                                                                                |
| <mark style="color:blue;">**pending**</mark>    | The customer viewed the transaction.                                                                                                                                                                                                                                                | Customer                                   | Only available with the installed Ottu-plugin , uses the checkout-page.                        |
| <mark style="color:blue;">**attempted**</mark>  | <p>It allows a try-again procedure on payment which has a failure state at the customer-end till it gets succeeded/expired.</p><p>For the payment which is allowed to be tried for once, it does not have an attempted state, it has either failed state or authorized state. \</p> | Customer                                   | <p><mark style="color:blue;"><strong>attempted</strong></mark></p><p>has different states.</p> |
| <mark style="color:blue;">**authorized**</mark> | The customer entered the card details, the amount has been allocated by the bank but not yet deducted.                                                                                                                                                                              | Customer                                   |                                                                                                |
| <mark style="color:blue;">**paid**</mark>       | The amount has been deducted by the bank.                                                                                                                                                                                                                                           | Customer                                   |                                                                                                |
| <mark style="color:blue;">**failed**</mark>     | The transaction is failed                                                                                                                                                                                                                                                           | Customer                                   | It is only applicable for the payment transaction that is attempted once.                      |
| <mark style="color:blue;">**canceled**</mark>   | The merchant has canceled the payment, and it can’t proceed afterward.                                                                                                                                                                                                              | Merchant                                   |                                                                                                |
| <mark style="color:blue;">**expired**</mark>    | Payment life-time is expired.                                                                                                                                                                                                                                                       | Customer                                   |                                                                                                |
| <mark style="color:blue;">**invalided**</mark>  | <p>The payment will not be available any longer, due to:</p><ul><li>Changing in payment configuration.</li><li>Changing in currency exchange configuration.</li><li>Other unforeseen events</li></ul>                                                                               | Merchant                                   |                                                                                                |
| <mark style="color:blue;">**cod**</mark>        | Cash-on -delivery.                                                                                                                                                                                                                                                                  | Customer                                   | It is mostly used in food ordering services.                                                   |

### [**States of child payment transaction**](payment-tracking.md#states-of-child-payment-transaction)

| <mark style="color:blue;">**Child state**</mark>     | <mark style="color:blue;">**Parent state**</mark> | <mark style="color:blue;">**Child state description**</mark>         | <mark style="color:blue;">**Note**</mark>                                                                                                                                                                                 |
| ---------------------------------------------------- | ------------------------------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**paid**</mark>            | <mark style="color:blue;">**authorized**</mark>   | <p>Part of</p><p>The payment amount will be captured by merchant</p> | A clone of the payment transaction has been created.                                                                                                                                                                      |
| <mark style="color:blue;">**refund**</mark>          | <mark style="color:blue;">**authorized**</mark>   | Part of payment amount will be back to customer                      | <p>There are two refund state</p><ul><li>Queued refund</li><li>Rejected refund</li></ul>                                                                                                                                  |
| <mark style="color:blue;">**refund-queued**</mark>   | <mark style="color:blue;">**authorized**</mark>   | Waiting bank’s approval on payment amount.                           |                                                                                                                                                                                                                           |
| <mark style="color:blue;">**refund-rejected**</mark> | <mark style="color:blue;">**authorized**</mark>   | Payment amount not authorized from the bank.                         |                                                                                                                                                                                                                           |
| <mark style="color:blue;">**voided**</mark>          | <mark style="color:blue;">**authorized**</mark>   |                                                                      | <p>Void attempts to immediately revert payment.</p><p>Voids can only be performed on transactions that have not yet been sent to</p><p>the bank could be a single job which can be fired by the end of working hours.</p> |

{% hint style="info" %}
Below figure shows the payment transaction details which is having parent and child state for same payment transaction process. \
A. The full payment amount.\
B. Parent state (**authorized**).\
C. Child state (**paid** and **refunded**).
{% endhint %}

![](<../.gitbook/assets/5 (2).gif>)

## [**Payment-attempt**](payment-tracking.md#payment-attempt)

Payment attempt is the trial that the customer(s) make to proceed the payment when the payment transaction fails, and it is allowed to proceed multiple times.

### [**Payment-attempt states**](payment-tracking.md#payment-attempt-states)

Payment attempt state represents the payment state at the customer-end.

| <mark style="color:blue;">**Payment-attempt state**</mark> | <mark style="color:blue;">**Description**</mark>                               | <mark style="color:blue;">**Mark**</mark>      |   |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------------- | - |
| <mark style="color:blue;">**pending**</mark>               | The customer viewed the transaction.                                           |                                                |   |
| <mark style="color:blue;">**created**</mark>               | The payment attempt is created due to a failed state.                          | Customers will be redirected to the bank page. |   |
| <mark style="color:red;">**failed**</mark>                 | The payment attempt failed, number of attempts based on payment configuration. |                                                |   |
| <mark style="color:blue;">**canceled**</mark>              | When a customer clicks on the cancel button.                                   |                                                |   |
| <mark style="color:green;">**success**</mark>              | Attempts to pay were successful.                                               |                                                |   |

## [**Payment transaction state and payment attempt state**](payment-tracking.md#payment-transaction-state-and-payment-attempt-state)

**Payment transaction allowed to be tried multiple times** \
<mark style="color:blue;">**attempted**</mark> state for payment transaction :heavy\_plus\_sign:  <mark style="color:blue;">**canceled**</mark> or <mark style="color:red;">**failed**</mark> state for payment attempt :arrow\_right: <mark style="color:blue;">**attempted**</mark> state for payment transaction (the payment transaction is ready to be attempted another time and stay having <mark style="color:blue;">**attempted**</mark> state until it changed to <mark style="color:blue;">**expired**</mark> or <mark style="color:blue;">**paid**</mark>).

<mark style="color:blue;">**attempted**</mark> state for payment transaction :heavy\_plus\_sign: <mark style="color:green;">**success**</mark> state for payment attempt :arrow\_right: <mark style="color:blue;">**paid**</mark> or <mark style="color:blue;">**authorized**</mark> state for payment transaction.&#x20;

**Payment transaction NOT allowed to be tried multiple times**

It does not have an <mark style="color:blue;">**attempted**</mark> state, it has either <mark style="color:red;">**failed**</mark> state or <mark style="color:blue;">**authorized**</mark> state.

![](<../.gitbook/assets/6 (2).png>)

## [**Notifications**](payment-tracking.md#notifications)

Ottu has implemented an event notification system, merchant(s) has the ability to choose the types of the notification event such as (Email, SMS, and WhatsApp).

| <mark style="color:blue;">**Notification event**</mark> | <mark style="color:blue;">**Triggered When**</mark>                                                                      |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| created                                                 | Payment transaction has been created                                                                                     |
| failed                                                  | Payment transits to <mark style="color:red;">**error**</mark> state and <mark style="color:red;">**failed**</mark> state |
| authorized                                              | Payment transits to <mark style="color:blue;">**authorized**</mark> state                                                |
| paid                                                    | Payment transits to <mark style="color:blue;">**paid**</mark> state                                                      |
| canceled                                                | Payment transits to <mark style="color:blue;">**canceled**</mark> state                                                  |
| expired                                                 | Payment transits to <mark style="color:blue;">**expired**</mark> state                                                   |
| voided                                                  | Payment transits to <mark style="color:blue;">**voided**</mark> state                                                    |
| refunded                                                | Payment transits to <mark style="color:blue;">**refunded**</mark> state                                                  |
| captured                                                | Payment transits to <mark style="color:blue;">**captured**</mark> state                                                  |

## [**Short-URL**](payment-tracking.md#short-url)

It is the ability to shorten the URL to be sent by SMS in order to reduce SMS consumption. See [URL shortener configuration](configuration.md#url-shortener-configurations).

## [**Payment transaction expiration time**](payment-tracking.md#payment-transaction-expiration-time)

The expiration time is the length of time within which a failed transaction can be processed again, and the payment can no longer be processed after the stipulated period has expired. **By default,** the expiration period is one hour.
