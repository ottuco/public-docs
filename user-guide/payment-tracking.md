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

### [**States of Child Payment Transaction**](payment-tracking.md#states-of-child-payment-transaction)

<table><thead><tr><th width="138">Child State</th><th width="134">Parent State</th><th width="222">Child State Description</th><th>                     Note</th></tr></thead><tbody><tr><td><strong>Paid</strong></td><td>Authorized</td><td>As a merchant, you'll receive a portion of the authorized payment amount (i.e., will be captured).</td><td>Rest assured, a copy of the payment transaction will be created to keep track of all the details.</td></tr><tr><td><strong>Refund</strong></td><td>Authorized</td><td>In case of a refund, a partial or the full paid amount will be returned to the customer.</td><td><p>There are two refund states to be aware of:</p><p><br></p><ul><li>Queued-refund.</li><li>Rejected-refund.</li></ul></td></tr><tr><td><strong>Refund-queued</strong></td><td>Authorized</td><td>The payment amount is currently awaiting bank approval.</td><td><br></td></tr><tr><td><strong>Refund-rejected</strong></td><td>Authorized</td><td>The payment amount has not been authorized by the bank.</td><td><br></td></tr><tr><td><strong>Voided</strong></td><td>Authorized</td><td>Sometimes, circumstances change, and you need to reverse the payment immediately. Our system allows you to void (i.e., cancel) transactions that have not yet been sent to the bank.</td><td><p>Voiding can only be performed on transactions that have not yet been sent to the bank.</p><p>This action may be executed as a single task at the end of the working hours.</p></td></tr></tbody></table>

The accompanying below figure shows the details of a payment transaction featuring both parent and child states for the same payment transaction process. Here are the main elements depicted:

1. The total payment amount.
2. Parent state `authorized`.
3. Child state `paid` and `refunded`.

Take a closer look at the figure to gain a comprehensive understanding of how these states interact in the payment transaction process.

![](<../.gitbook/assets/5 (2).gif>)

## [**Payment-Attempt**](payment-tracking.md#payment-attempt)

A payment attempt refers to the customer's attempt to complete a payment when the transaction fails. Please note that customers can make multiple attempts to complete their payment.\
Don't let a single failed payment ruin the payment experience. The Payment Retry feature is the perfect solution for customers who encounter payment failures during their transactions. With Payment Retry, customers have the power to try again and again until they complete their payment successfully. With our innovative approach, we ensure that you won't miss out on the things you love. No longer will you have to start the entire process from scratch. We've simplified things for you, making it easier than ever to finalize your customer payment and giving you the flexibility and peace of mind you deserve. It's time to say goodbye to frustration and hello to a smoother payment experience. Say goodbye to payment failures and hello to successful transactions.

### [**Payment-Attempt States**](payment-tracking.md#payment-attempt-states)

Payment attempt states represent the payment state at the customer end.

| Payment-Attempt State |             Description                                                                               |                    Note                                            |
| --------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Pending**           | The customer has viewed the transaction details.                                                      | <p><br></p>                                                        |
| **Created**           | The payment attempt is created as a result of an unsuccessful transaction (i.e., failed state).       | Customers will be redirected to the bank's page.                   |
| **Failed**            | Payment failed due to various reasons, such as insufficient funds and other related issues.           | The number of attempts is determined by the payment configuration. |
| **Canceled**          | The customer has chosen to click the cancel button.                                                   | <p><br></p>                                                        |
| **Success**           | The payment attempt was successful (i.e., successful pay).                                            | <p><br></p>                                                        |
| **Error**             | When we receive an error from the PG for that payment (e.g., PG Service is down, network error, etc.) | <p><br></p>                                                        |

{% hint style="info" %}
when **Cod** (Cash on Delivery) is used for cash payments, a payment attempt is created. This is because every change in the final state of the payment transaction must be associated with a payment attempt.
{% endhint %}

## **Payment Transaction State & Payment Attempt State**

Understanding the various possible combination of Payment Transactions that are in the <mark style="color:orange;">`attempted`</mark> state with the Payment-Attempts states is crucial. Let's simplify them:

**For the payment transactions that allow multiple attempts, we encounter two combinations:**

* When a Payment Transaction is in the <mark style="color:orange;">`attempted`</mark> state, and the Payment-Attempt state is <mark style="color:red;">`canceled`</mark> or <mark style="color:red;">`failed`</mark>, the payment transaction state remains <mark style="color:orange;">`attempted`</mark>. That means that the transaction is open for another attempt until it either expires (i.e., <mark style="color:red;">`expired`</mark> state) or successfully reaches the <mark style="color:green;">`paid`</mark> state.\
  **In simple terms:**\
  <mark style="color:orange;">`attempted`</mark> state for payment transaction ➕ <mark style="color:red;">`canceled`</mark> or <mark style="color:red;">`failed`</mark> state for payment attempt ➡ <mark style="color:orange;">`attempted`</mark> state for payment transaction, until it changed to <mark style="color:red;">`expired`</mark> or <mark style="color:green;">`paid`</mark>, and the payment transaction can be attempted again
* Alternatively, when a Payment Transaction is in the <mark style="color:orange;">`attempted`</mark> state and the Payment-Attempt succeeds (i.e., <mark style="color:blue;">`success`</mark> state), the payment transaction progresses into the <mark style="color:green;">`paid`</mark> or <mark style="color:green;">`authorized`</mark> state.\
  **In simple terms:**\
  <mark style="color:orange;">`attempted`</mark> state for payment transaction ➕ <mark style="color:blue;">`success`</mark> state for payment attempt ➡ <mark style="color:green;">`paid`</mark> or <mark style="color:green;">`authorized`</mark> state for payment transaction.

**For the payment transactions that do not allow multiple attempts:**

* Such transactions do not have an <mark style="color:orange;">`attempted`</mark> state. Instead, they can either be in a <mark style="color:red;">`failed`</mark> or <mark style="color:green;">`authorized`</mark> state.

The following image sums it all up clearly:

![](<../.gitbook/assets/6 (2).png>)

## [**Notifications**](payment-tracking.md#notifications)

Ottu has a cutting-edge Notification System, empowering merchants with unparalleled control to personalize their preferred notification channels, including Email, SMS, and WhatsApp.\
At a glance, Ottu's Notification Event feature keeps you informed about every stage of your online payment process. Seamlessly integrated into your workflow, these events provide real-time updates on crucial milestones. Let's explore the triggers and events:

| Notification Event | Triggered When                                                                                                     |
| ------------------ | ------------------------------------------------------------------------------------------------------------------ |
| **Created**        | The payment transaction is successfully initiated.                                                                 |
| **Failed**         | The Payment encounters an error, preventing its completion (i.e., the payment transitioned to the `failed` state). |
| **Authorized**     | The payment is successfully authorized.                                                                            |
| **Paid**           | The payment is successfully processed and transitioned to the `paid` state.                                        |
| **Canceled**       | The payment is canceled.                                                                                           |
| **Expired**        | The payment has reached its expiration date.                                                                       |
| **Voided**         | The payment transitioned to the `voided` state.                                                                    |
| **Refunded**       | The payment transitioned to the `refunded` state.                                                                  |
| **Captured**       | The payment is captured.                                                                                           |

With Ottu, you can effortlessly track the progress of your online payments. Stay ahead of the game and ensure a seamless experience for both you and your customers. Embrace the power of real-time updates!

## [Optimizing SMS with Shortened URLs](payment-tracking.md#optimizing-sms-with-shortened-urls)

With our innovative[ URL shortener configuration](configuration.md#url-shortener-configurations), you can streamline your messaging process and ensure optimal utilization of your SMS resources. It is a game-changing feature that allows you to effortlessly shorten URLs for SMS delivery, resulting in reduced SMS consumption. Say goodbye to wasteful long links and embrace the efficiency of Short-URL.

## [**Payment Transaction Expiration Time**](payment-tracking.md#payment-transaction-expiration-time)

Controlling your payment transaction expiration time is crucial to ensure smooth and secure transactions. This expiration time defines the duration in which a failed transaction can be retried or reprocessed. However, once this stipulated time frame has elapsed, the payment cannot be processed again. With our advanced online payment management system (OPMS), you can confidently navigate the digital landscape, knowing that your transactions are processed swiftly and securely. Embrace the future of online payments and unlock new possibilities for your business. Together, let's redefine the way you manage transactions and pave the way for success!

\
