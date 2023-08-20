# Payment Tracking

## [Overview](payment-tracking.md#overview)

At Ottu, we believe that your success is our success. That's why our dashboard is meticulously crafted to support your growth, enhance your operations, and elevate your business to new heights. Discover trends, identify opportunities, and stay ahead of the curve with data-driven guidance right at your fingertips. Our dashboard is a user-friendly and intuitive workspace designed to empower all Ottu clients with a robust payment pipeline, saving your time and effort as you manage your payments efficiently. With a comprehensive suite of tools and features, clients can effortlessly create, monitor, and track payments. All in one place. But that's not all—As soon as you log in, the Ottu dashboard welcomes you warmly with insightful analytics derived from your payment workloads, offering valuable insights into the payment pipeline and guiding you toward your next steps upon login. Embrace efficiency, drive success, and take control of your payment operations today. Your success starts here!

## [Transaction Table](payment-tracking.md#transaction-table)

Ottu ensures a seamless experience for merchants by providing an easily accessible transaction table integrated with each installed [plugin](plugins/). This user-friendly feature grants merchants full visibility into their transaction history and details. Through the transaction table, users can effortlessly perform various payment [operations](../developer/operations.md#external-operations), including voiding, refunding, and capturing, ensuring easy management of payment activities. The transaction table provides a centralized hub for efficient management.

{% hint style="info" %}
To access the transaction table, simply navigate to the “Transaction Tab” located under each installed [plugin](https://docs.ottu.com/user-guide/plugins).
{% endhint %}

### [Proxy Fields](payment-tracking.md#proxy-fields)

Ottu enhances the merchant experience by providing a dedicated transaction table for each plugin, enabling seamless monitoring of transactions. With Ottu's innovative approach, merchants gain the flexibility to customize the transaction table's headers, allowing for quick and effortless access to the desired information. This empowering feature ensures merchants can efficiently navigate and retrieve the necessary details, optimizing their transaction management process.&#x20;

#### [How to activate headers setting on the dashboard](payment-tracking.md#how-to-activate-headers-setting-on-the-dashboard)

1. From the **Ottu Dashboard**.
2. Navigate to the **Administration Panel**.
3. Access the **Plugin Config** for the desired transaction table.
4. Enable the **Individual Proxy Fields** option by ticking the checkbox.

<figure><img src="../.gitbook/assets/activate proxy fields.png" alt=""><figcaption></figcaption></figure>

#### [Header List](payment-tracking.md#header-list)

To manage the headers, follow these steps:

1. From the **Ottu Dashboard** go to the **Required Plugin Tab**.
2. Access the **Settings**.
3. In the **Table Headers** section, you can add or remove headers by dragging them from the right to the left or vice versa **(right ⇆ left)**. This dynamic empowers the merchants to customize the displayed headers according to their preferences.

<figure><img src="../.gitbook/assets/Add or remove headers (1).png" alt=""><figcaption></figcaption></figure>

#### [Child Table Transaction](payment-tracking.md#child-table-transaction)

[Child transactions](payment-tracking.md#child-payment-transaction) refer to transactions generated from capture, refund, or void operations (i.e., [external operations](../developer/operations.md#external-operations)). These transactions are linked to the main origin payment transaction, known as the [Parent Transaction](payment-tracking.md#parent-payment-transaction), and are displayed under them accordingly. The headers of each child transaction inherit from the parent transaction's headers, ensuring consistency and easy reference.

<figure><img src="../.gitbook/assets/Child proxy header (1).png" alt=""><figcaption></figcaption></figure>

### [Amount Definitions & Calculation Mechanism](payment-tracking.md#amount-definitions-and-calculation-mechanism)

Transaction tables offer the flexibility to add or remove multiple headers. Take a look at the [proxy fields](payment-tracking.md#proxy-fields) for more options. Amount headers are intelligently categorized under different titles, ensuring clarity on their specific roles within the payment process flow.

#### [**Amount**:](payment-tracking.md#amount)

It is the initial amount when the transaction is first created.

#### [**Settled amount:**](payment-tracking.md#settled-amount)&#x20;

**It refers to the amount in the same currency as the initial amount.**

* **For editable amount:** It is the customer's entered amount at the checkout page. If the customer pays the full required amount, it will be equal to the [initiating amount](payment-tracking.md#amount).
* **For non-editable amount:** The settled amount remains the same as the [initiating amount](payment-tracking.md#amount).

#### [**Paid amount:**](payment-tracking.md#paid-amount)&#x20;

It is the amount that is transferred or credited to the merchant's bank account.

* **For purchase:** It represents the settled amount, including the fee.
* **For authorize:** It is the total amount captured by the staff.

{% hint style="info" %}
The selection between Purchase and Authorization is typically determined within the [payment gateway](payment-gateway.md)'s operational configuration, specifically through the gateway settings (MID) operation configuration. This configuration governs whether the operation conducted is an authorization or a purchase.
{% endhint %}

#### [Remaining amount:](payment-tracking.md#remaining-amount)&#x20;

It refers to the calculated difference between the initially specified [Amount](payment-tracking.md#amount) (what the customer was initially required to pay) and the [Settled Amount](payment-tracking.md#settled-amount) (the actual amount settled or paid).

|[Amount](payment-tracking.md#amount) - [Settled amount](payment-tracking.md#settled-amount)|

#### [Fee:](payment-tracking.md#fee)&#x20;

The fee refers to an additional amount that is added to the [initiating amount](payment-tracking.md#amount), which the customer proceeds with.

{% hint style="info" %}
Depending on the [currency configuration](currencies.md#currency-configuration-page) of the merchant and the selected [payment gateway](payment-gateway.md) (PG), fees may or may not be applied to transactions conducted in the default currency or foreign currency.
{% endhint %}

#### [Refunded amount:](payment-tracking.md#refunded-amount)&#x20;

It refers to the amount that is sent back or returned to the customer's bank account, which is deducted from the[ paid amount](payment-tracking.md#paid-amount).

#### [Voided amount: ](payment-tracking.md#voided-amount)

It refers to the total transaction amount that is credited back to the customer's bank account upon completion of the rollback process.

It is applicable only for authorized payments that have not been fully or partially captured.

{% hint style="info" %}
* **Capture:** It can capture the entire amount, including the fee and settled amount.
* **Refund:** It can only refund the settled amount (i.e., without the fee).
* **Void:** It will nullify (or void) the entire amount, including the associated fee, if any.
{% endhint %}

## [Dashboard Charts](payment-tracking.md#dashboard-charts)

Now, merchants can effortlessly track and monitor their overall transactions and sales progress in a single, convenient location. With our user-friendly interface, merchants gain unprecedented visibility into their business performance. Stay informed with real-time updates and make the right decisions. All in one centralized hub. Our innovative charts provide a clear and comprehensive overview of your transactions and sales progress, empowering you to identify trends, analyze patterns, and seize new opportunities. Don't miss out on the power of data-driven insights. Experience the difference firsthand!\
Take advantage of our cutting-edge Dashboard Charts feature and revolutionize the way you track and monitor your business. Contemplate these snapshots captured directly from our intuitive dashboard, showcasing a variety of insightful and actionable charts.

### [Total Transactions Chart](payment-tracking.md#total-transactions-chart)

Shows the total value and overall rate of success across different periods.

![](<../.gitbook/assets/1 (2) (1).png>)

### [Average Transaction Size Chart](payment-tracking.md#average-transaction-size-chart)

<figure><img src="../.gitbook/assets/AverageTransaction (1).png" alt=""><figcaption></figcaption></figure>

### [Product Wise Sales Chart](payment-tracking.md#product-wise-sales-chart)

![](<../.gitbook/assets/3 (4).png>)

### [Recent Orders Dynamic Report](payment-tracking.md#recent-orders-dynamic-report)

![](<../.gitbook/assets/4 (4) (1).png>)

## [ Payment Transaction](payment-tracking.md#payment-transaction)

Payment transactions form the backbone of every successful merchant's operations. These vital pieces of operations empower merchants to effortlessly create payments and execute other actions such as refunds, voids, and cancellations. To ensure optimal efficiency and convenience, transactions are categorized into two types:

*   #### Parent Payment Transaction:

    These transactions store all the essential data about the payment, serving as a comprehensive record of the transaction details to track your financial process.
*   #### Child Payment transaction:

    Designed specifically to streamline subsequent operations triggered by merchants, these transactions seamlessly handle tasks that arise after initiating the payment.

### [Payment Transactions States](payment-tracking.md#payment-transactions-states)

Payment Transaction States play a pivotal role in empowering merchants to maintain an impeccable audit trail of the transaction process. These states are represented by flags, providing valuable insights into each stage of the payment journey. With a clear understanding of the transaction's current state, merchants can ensure streamlined [operations](../developer/operations.md#external-operations) and enhanced financial oversight. Stay informed, stay in control, and elevate your financial leadership with the power of transaction state tracking. Our advanced OPMS combines simplicity, accuracy, and security to elevate your business operations. Experience hassle-free payment processing, maximize efficiency, reduce errors, and boost your business's success.

### [**States of Parent Payment Transaction**](payment-tracking.md#states-of-parent-payment-transaction)

<table><thead><tr><th width="138" align="center">State</th><th width="268">          State Description</th><th width="128" align="center">Actor</th><th>                  Note</th></tr></thead><tbody><tr><td align="center"><strong>Created</strong></td><td>The payment has been initiated successfully.</td><td align="center">Merchant</td><td><br></td></tr><tr><td align="center"><strong>Pending</strong></td><td>The transaction is awaiting the customer to complete the payment process, i.e., the payment transaction has reached a stage where the customer has interacted with it. This interaction could involve the customer having seen the payment information, accessed the payment link page, or being redirected to the Ottu checkout page. In essence, it is awaiting further action, such as confirmation or completion.</td><td align="center">Customer</td><td>This state is only available with the installed Ottu plugin and utilizes the checkout page.</td></tr><tr><td align="center"><strong>Attempted</strong></td><td>This state is assigned to payments that require a retry process when there is a failure at the customer's end. The payment remains in this state until it is successfully processed, reaches its expiration date, or is canceled by the merchant. Alternatively, payments can be marked as invalid if certain crucial configurations of the payment gateway are modified, such as the removal of currency exchange support.</td><td align="center">Customer</td><td><ul><li>Attempted status has different states.</li></ul><ul><li>Please note that for payments that can only be attempted once, there is no <code>attempted</code> state; Instead, they will be either in a <code>failed</code> or <code>authorized</code> state.</li></ul></td></tr><tr><td align="center"><strong>Authorized</strong></td><td>The customer has securely entered his card details, and the bank has allocated the payment amount, but it is not deducted yet.</td><td align="center">Customer</td><td><br></td></tr><tr><td align="center"><strong>Paid</strong></td><td>The bank has deducted the payment amount successfully.</td><td align="center">Customer</td><td><br></td></tr><tr><td align="center"><strong>Failed</strong></td><td>The transaction encountered an error and couldn't be completed.</td><td align="center">Customer</td><td>This state is specific to payment transactions that can only be attempted once.</td></tr><tr><td align="center"><strong>Canceled</strong></td><td>The merchant has canceled the payment, and no further action can be taken.</td><td align="center">Merchant</td><td><br></td></tr><tr><td align="center"><strong>Expired</strong></td><td>The payment's lifespan has ended (i.e., expired).</td><td align="center">Customer</td><td><br></td></tr><tr><td align="center"><strong>Invalid</strong></td><td>The payment is no longer available due to changes in the payment configuration, currency exchange configuration, or other unforeseen events.</td><td align="center">Merchant</td><td><br></td></tr><tr><td align="center"><strong>COD</strong></td><td>Cash on Delivery</td><td align="center">Customer</td><td><br></td></tr></tbody></table>

### [**States of child payment transaction**](payment-tracking.md#states-of-child-payment-transaction)

| Child State                                          | Parent State                                    | Child State Description                                              | Note                                                                                                                                                                                                                      |
| ---------------------------------------------------- | ----------------------------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**paid**</mark>            | <mark style="color:blue;">**authorized**</mark> | <p>Part of</p><p>The payment amount will be captured by merchant</p> | A clone of the payment transaction has been created.                                                                                                                                                                      |
| <mark style="color:blue;">**refund**</mark>          | <mark style="color:blue;">**authorized**</mark> | Part of payment amount will be back to customer                      | <p>There are two refund state</p><ul><li>Queued refund</li><li>Rejected refund</li></ul>                                                                                                                                  |
| <mark style="color:blue;">**refund-queued**</mark>   | <mark style="color:blue;">**authorized**</mark> | Waiting bank’s approval on payment amount.                           |                                                                                                                                                                                                                           |
| <mark style="color:blue;">**refund-rejected**</mark> | <mark style="color:blue;">**authorized**</mark> | Payment amount not authorized from the bank.                         |                                                                                                                                                                                                                           |
| <mark style="color:blue;">**voided**</mark>          | <mark style="color:blue;">**authorized**</mark> |                                                                      | <p>Void attempts to immediately revert payment.</p><p>Voids can only be performed on transactions that have not yet been sent to</p><p>the bank could be a single job which can be fired by the end of working hours.</p> |

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

<table><thead><tr><th>Payment Attempt State</th><th>Description</th><th>Note</th><th data-hidden></th></tr></thead><tbody><tr><td><mark style="color:blue;"><strong>pending</strong></mark></td><td>The customer viewed the transaction.</td><td></td><td></td></tr><tr><td><mark style="color:blue;"><strong>created</strong></mark></td><td>The payment attempt is created due to a failed state.</td><td>Customers will be redirected to the bank page.</td><td></td></tr><tr><td><mark style="color:red;"><strong>failed</strong></mark></td><td>The payment attempt failed, number of attempts based on payment configuration.</td><td></td><td></td></tr><tr><td><mark style="color:blue;"><strong>canceled</strong></mark></td><td>When a customer clicks on the cancel button.</td><td></td><td></td></tr><tr><td><mark style="color:green;"><strong>success</strong></mark></td><td>Attempts to pay were successful.</td><td></td><td></td></tr></tbody></table>

## [**Payment transaction state and payment attempt state**](payment-tracking.md#payment-transaction-state-and-payment-attempt-state)

**Payment transaction allowed to be tried multiple times** \
<mark style="color:blue;">**attempted**</mark> state for payment transaction :heavy\_plus\_sign: <mark style="color:blue;">**canceled**</mark> or <mark style="color:red;">**failed**</mark> state for payment attempt :arrow\_right: <mark style="color:blue;">**attempted**</mark> state for payment transaction (the payment transaction is ready to be attempted another time and stay having <mark style="color:blue;">**attempted**</mark> state until it changed to <mark style="color:blue;">**expired**</mark> or <mark style="color:blue;">**paid**</mark>).

<mark style="color:blue;">**attempted**</mark> state for payment transaction :heavy\_plus\_sign: <mark style="color:green;">**success**</mark> state for payment attempt :arrow\_right: <mark style="color:blue;">**paid**</mark> or <mark style="color:blue;">**authorized**</mark> state for payment transaction.&#x20;

**Payment transaction NOT allowed to be tried multiple times**

It does not have an <mark style="color:blue;">**attempted**</mark> state, it has either <mark style="color:red;">**failed**</mark> state or <mark style="color:blue;">**authorized**</mark> state.

![](<../.gitbook/assets/6 (2).png>)

## [**Notifications**](payment-tracking.md#notifications)

Ottu has implemented an event notification system, merchant(s) has the ability to choose the types of the notification event such as (Email, SMS, and WhatsApp).

| Notification Event | Triggered When                                                                                                           |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| created            | Payment transaction has been created                                                                                     |
| failed             | Payment transits to <mark style="color:red;">**error**</mark> state and <mark style="color:red;">**failed**</mark> state |
| authorized         | Payment transits to <mark style="color:blue;">**authorized**</mark> state                                                |
| paid               | Payment transits to <mark style="color:blue;">**paid**</mark> state                                                      |
| canceled           | Payment transits to <mark style="color:blue;">**canceled**</mark> state                                                  |
| expired            | Payment transits to <mark style="color:blue;">**expired**</mark> state                                                   |
| voided             | Payment transits to <mark style="color:blue;">**voided**</mark> state                                                    |
| refunded           | Payment transits to <mark style="color:blue;">**refunded**</mark> state                                                  |
| captured           | Payment transits to <mark style="color:blue;">**captured**</mark> state                                                  |

## [**Short-URL**](payment-tracking.md#short-url)

It is the ability to shorten the URL to be sent by SMS in order to reduce SMS consumption. See [URL shortener configuration](configuration.md#url-shortener-configurations).

## [**Payment transaction expiration time**](payment-tracking.md#payment-transaction-expiration-time)

The expiration time is the length of time within which a failed transaction can be processed again, and the payment can no longer be processed after the stipulated period has expired. **By default,** the expiration period is one hour.
