# Payment Transactions Insights

Delve into the heart of your financial universe with Ottu's Payment Transaction Insights. In this enlightening section, you'll have the knowledge and tools to dissect and understand your transactions like never before. Let's embark on a journey to unveil the secrets hidden within your payments.

## [Transaction Table](payment-transactions-insights.md#transaction-table)

Ottu ensures a seamless experience for merchants by providing an easily accessible transaction table integrated with each installed [plugin](../plugins/). This user-friendly feature grants merchants full visibility into their transaction history and details. Through the transaction table, users can effortlessly perform various payment [operations](../../developer/operations.md#external-operations), including voiding, refunding, and capturing, ensuring easy management of payment activities. The transaction table provides a centralized hub for efficient management.

{% hint style="info" %}
To access the transaction table, simply navigate to the “Transaction Tab” located under each installed [plugin](../plugins/).
{% endhint %}

### [Proxy Fields](payment-transactions-insights.md#proxy-fields)

Ottu enhances the merchant experience by providing a dedicated transaction table for each plugin, enabling seamless monitoring of transactions. With Ottu's innovative approach, merchants gain the flexibility to customize the transaction table's headers, allowing for quick and effortless access to the desired information. This empowering feature ensures merchants can efficiently navigate and retrieve the necessary details, optimizing their transaction management process.&#x20;

#### [How to activate headers setting on the dashboard](payment-transactions-insights.md#how-to-activate-headers-setting-on-the-dashboard)

1. From the **Ottu Dashboard**.
2. Navigate to the **Administration Panel**.
3. Access the **Plugin Config** for the desired transaction table.
4. Enable the **Individual Proxy Fields** option by ticking the checkbox.

<figure><img src="../../.gitbook/assets/activate proxy fields.png" alt=""><figcaption></figcaption></figure>

#### [Header List](payment-transactions-insights.md#header-list)

To manage the headers, follow these steps:

1. From the **Ottu Dashboard** go to the **Required Plugin Tab**.
2. Access the **Settings**.
3. In the **Table Headers** section, you can add or remove headers by dragging them from the right to the left or vice versa **(right ⇆ left)**. This dynamic empowers the merchants to customize the displayed headers according to their preferences.

<figure><img src="../../.gitbook/assets/Add or remove headers (1).png" alt=""><figcaption></figcaption></figure>

#### [Child Table Transaction](payment-transactions-insights.md#child-table-transaction)

[Child transactions](payment-transactions-insights.md#child-payment-transaction) refer to transactions generated from capture, refund, or void operations (i.e., [external operations](../../developer/operations.md#external-operations)). These transactions are linked to the main origin payment transaction, known as the [Parent Transaction](payment-transactions-insights.md#parent-payment-transaction), and are displayed under them accordingly. The headers of each child transaction inherit from the parent transaction's headers, ensuring consistency and easy reference.

<figure><img src="../../.gitbook/assets/Child proxy header (1).png" alt=""><figcaption></figcaption></figure>

### [Amounts Breakdown](payment-transactions-insights.md#amounts-breakdown)

Transaction tables offer the flexibility to add or remove multiple headers. Take a look at the [proxy fields](payment-transactions-insights.md#proxy-fields) for more options. Amount headers are intelligently categorized under different titles, ensuring clarity on their specific roles within the payment process flow.

#### [**Amount**:](payment-transactions-insights.md#amount)

It is the initial amount when the transaction is first created.

#### [**Settled amount:**](payment-transactions-insights.md#settled-amount)&#x20;

**It refers to the amount in the same currency as the initial amount.**

* **For editable amount:** It is the customer's entered amount at the checkout page. If the customer pays the full required amount, it will be equal to the [initiating amount](payment-transactions-insights.md#amount).
* **For non-editable amount:** The settled amount remains the same as the [initiating amount](payment-transactions-insights.md#amount).

#### [**Paid amount:**](payment-transactions-insights.md#paid-amount)&#x20;

It is the amount that is transferred or credited to the merchant's bank account.

* **For purchase:** It represents the settled amount, including the fee.
* **For authorize:** It is the total amount captured by the staff.

{% hint style="info" %}
The selection between Purchase and Authorization is typically determined within the [payment gateway](../payment-gateway.md)'s operational configuration, specifically through the gateway settings (MID) operation configuration. This configuration governs whether the operation conducted is an authorization or a purchase.
{% endhint %}

#### [Remaining amount:](payment-transactions-insights.md#remaining-amount)&#x20;

It refers to the calculated difference between the initially specified [Amount](payment-transactions-insights.md#amount) (what the customer was initially required to pay) and the [Settled Amount](payment-transactions-insights.md#settled-amount) (the actual amount settled or paid).

|[Amount](payment-transactions-insights.md#amount) - [Settled amount](payment-transactions-insights.md#settled-amount)|

#### [Fee:](payment-transactions-insights.md#fee)&#x20;

The fee refers to an additional amount that is added to the [initiating amount](payment-transactions-insights.md#amount), which the customer proceeds with.

{% hint style="info" %}
Depending on the [currency configuration](../currencies.md#currency-configuration-page) of the merchant and the selected [payment gateway](../payment-gateway.md) (PG), fees may or may not be applied to transactions conducted in the default currency or foreign currency.
{% endhint %}

#### [Refunded amount:](payment-transactions-insights.md#refunded-amount)&#x20;

It refers to the amount that is sent back or returned to the customer's bank account, which is deducted from the[ paid amount](payment-transactions-insights.md#paid-amount).

#### [Voided amount: ](payment-transactions-insights.md#voided-amount)

It refers to the total transaction amount that is credited back to the customer's bank account upon completion of the rollback process.

It is applicable only for authorized payments that have not been fully or partially captured.

{% hint style="info" %}
* **Capture:** It can capture the entire amount, including the fee and settled amount.
* **Refund:** It can only refund the settled amount (i.e., without the fee).
* **Void:** It will nullify (or void) the entire amount, including the associated fee, if any.
{% endhint %}

## [Dashboard Charts](payment-transactions-insights.md#dashboard-charts)

Now, merchants can effortlessly track and monitor their overall transactions and sales progress in a single, convenient location. With our user-friendly interface, merchants gain unprecedented visibility into their business performance. Stay informed with real-time updates and make the right decisions. All in one centralized hub. Our innovative charts provide a clear and comprehensive overview of your transactions and sales progress, empowering you to identify trends, analyze patterns, and seize new opportunities. Don't miss out on the power of data-driven insights. Experience the difference firsthand!\
Take advantage of our cutting-edge Dashboard Charts feature and revolutionize the way you track and monitor your business. Contemplate these snapshots captured directly from our intuitive dashboard, showcasing a variety of insightful and actionable charts.

### [Total Transactions](payment-transactions-insights.md#total-transactions)

Shows the total value and overall rate of success across different periods.

![](<../../.gitbook/assets/1 (2) (1).png>)

### [Average Transaction](payment-transactions-insights.md#average-transaction)

<figure><img src="../../.gitbook/assets/AverageTransaction (1).png" alt=""><figcaption></figcaption></figure>

### [Product Wise Sales](payment-transactions-insights.md#product-wise-sales)

![](<../../.gitbook/assets/3 (4).png>)

### [Recent Orders Report](payment-transactions-insights.md#recent-orders-report)

![](<../../.gitbook/assets/4 (4) (1).png>)
