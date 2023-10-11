# Payment Transactions States

Embark on a voyage through the intricate world of payment states with Ottu's comprehensive guide. In this section, you'll explore the various stages your payments go through. From initiation to completion, every step is illuminated. To ensure optimal efficiency and convenience, transactions are categorized into two types:

*   #### Parent Payment Transaction:

    These transactions store all the essential data about the payment, serving as a comprehensive record of the transaction details to track your financial process.
*   #### Child Payment transaction:

    Designed specifically to streamline subsequent operations triggered by merchants, these transactions seamlessly handle tasks that arise after initiating the payment.

## [Payment States](payment-transactions-states.md#payment-states)

Payment Transaction States play a pivotal role in empowering merchants to maintain an impeccable audit trail of the transaction process. These states are represented by flags, providing valuable insights into each stage of the payment journey. With a clear understanding of the transaction's current state, merchants can ensure streamlined [operations](../../developer/operations.md#external-operations) and enhanced financial oversight. Stay informed, stay in control, and elevate your financial leadership with the power of transaction state tracking. Our advanced OPMS combines simplicity, accuracy, and security to elevate your business operations. Experience hassle-free payment processing, maximize efficiency, reduce errors, and boost your business's success.

### [**Parent States**](payment-transactions-states.md#parent-states)

In the realm of payment transactions, understanding the different transaction states, their corresponding descriptions, and the involved actors is essential for orchestrating seamless and secure financial exchanges. The below table offers a concise overview of these key elements, shedding light on the intricate dance of processes and participants that define the world of financial transactions. Through this table, we aim to provide a clear and informative snapshot of this vital aspect of modern commerce.

<table><thead><tr><th width="138" align="center">State</th><th width="268">          State Description</th><th width="128" align="center">Actor</th><th>                  Note</th></tr></thead><tbody><tr><td align="center"><strong>Created</strong></td><td>The payment has been initiated successfully.</td><td align="center">Merchant</td><td><br></td></tr><tr><td align="center"><strong>Pending</strong></td><td>The transaction is awaiting the customer to complete the payment process, i.e., the payment transaction has reached a stage where the customer has interacted with it. This interaction could involve the customer having seen the payment information, accessed the payment link page, or being redirected to the Ottu checkout page. In essence, it is awaiting further action, such as confirmation or completion.</td><td align="center">Customer</td><td>This state is only available with the installed Ottu plugin and utilizes the checkout page.</td></tr><tr><td align="center"><strong>Attempted</strong></td><td>This state is assigned to payments that require a retry process when there is a failure at the customer's end. The payment remains in this state until it is successfully processed, reaches its expiration date, or is canceled by the merchant. Alternatively, payments can be marked as invalid if certain crucial configurations of the payment gateway are modified, such as the removal of currency exchange support.</td><td align="center">Customer</td><td><ul><li>Attempted status has different states.</li></ul><ul><li>Please note that for payments that can only be attempted once, there is no <code>attempted</code> state; Instead, they will be either in a <code>failed</code> or <code>authorized</code> state.</li></ul></td></tr><tr><td align="center"><strong>Authorized</strong></td><td>The customer has securely entered his card details, and the bank has allocated the payment amount, but it is not deducted yet.</td><td align="center">Customer</td><td><br></td></tr><tr><td align="center"><strong>Paid</strong></td><td>The bank has deducted the payment amount successfully.</td><td align="center">Customer</td><td><br></td></tr><tr><td align="center"><strong>Failed</strong></td><td>The transaction encountered an error and couldn't be completed.</td><td align="center">Customer</td><td>This state is specific to payment transactions that can only be attempted once.</td></tr><tr><td align="center"><strong>Canceled</strong></td><td>The merchant has canceled the payment, and no further action can be taken.</td><td align="center">Merchant</td><td><br></td></tr><tr><td align="center"><strong>Expired</strong></td><td>The payment's lifespan has ended (i.e., expired).</td><td align="center">Customer</td><td><br></td></tr><tr><td align="center"><strong>Invalid</strong></td><td>The payment is no longer available due to changes in the payment configuration, currency exchange configuration, or other unforeseen events.</td><td align="center">Merchant</td><td><br></td></tr><tr><td align="center"><strong>COD</strong></td><td>Cash on Delivery</td><td align="center">Customer</td><td><br></td></tr></tbody></table>

### [**Child Payment States**](payment-transactions-states.md#child-payment-states)

Within the dynamic landscape of online payment transactions, The below table provides a comprehensive breakdown of [child states](payment-transactions-states.md#child-payment-transaction), their corresponding [parent states](payment-transactions-states.md#parent-payment-transaction), and succinct descriptions. Here, The parent state represents the initial status of a payment transaction before it progresses to any of its child states.

<table><thead><tr><th width="138">Child State</th><th width="134">Parent State</th><th width="222">Child State Description</th><th>                     Note</th></tr></thead><tbody><tr><td><strong>Paid</strong></td><td>Authorized</td><td>As a merchant, you'll receive a portion of the authorized payment amount (i.e., will be captured).</td><td>Rest assured, a copy of the payment transaction will be created to keep track of all the details.</td></tr><tr><td><strong>Refunded</strong></td><td>Authorized / Paid</td><td>In case of a refund, a partial or the full paid amount will be returned to the customer.</td><td><p>There are two refund states to be aware of:</p><p><br></p><ul><li>Queued-refund.</li><li>Rejected-refund.</li></ul></td></tr><tr><td><strong>Refund-queued</strong></td><td>Authorized / Paid</td><td>The payment amount is currently awaiting bank approval.</td><td><br></td></tr><tr><td><strong>Refund-rejected</strong></td><td>Authorized / Paid</td><td>The payment amount has not been authorized by the bank.</td><td><br></td></tr><tr><td><strong>Voided</strong></td><td>Authorized</td><td>Sometimes, circumstances change, and you need to reverse the payment immediately. Our system allows you to void (i.e., cancel) transactions that have not yet been sent to the bank.</td><td><p>Voiding can only be performed on transactions that have not yet been sent to the bank.</p><p>This action may be executed as a single task at the end of the working hours.</p></td></tr></tbody></table>

The accompanying below figure shows the details of a payment transaction featuring both parent and child states for the same payment transaction process. Here are the main elements depicted:

1. The total payment amount.
2. Parent state `authorized`.
3. Child state `paid` and `refunded`.

Take a closer look at the figure to gain a comprehensive understanding of how these states interact in the payment transaction process.

![](<../../.gitbook/assets/5 (2).gif>)

## [**Payment-Attempt**](payment-transactions-states.md#payment-attempt)

A payment attempt refers to the customer's attempt to complete a payment when the transaction fails. Please note that customers can make multiple attempts to complete their payment.\
Don't let a single failed payment ruin the payment experience. The Payment Retry feature is the perfect solution for customers who encounter payment failures during their transactions. With Payment Retry, customers have the power to try again and again until they complete their payment successfully. With our innovative approach, we ensure that you won't miss out on the things you love. No longer will you have to start the entire process from scratch. We've simplified things for you, making it easier than ever to finalize your customer payment and giving you the flexibility and peace of mind you deserve. It's time to say goodbye to frustration and hello to a smoother payment experience. Say goodbye to payment failures and hello to successful transactions.

### [**Attempt States**](payment-transactions-states.md#attempt-states)

Payment attempt states represent the payment state at the customer end.

| Payment-Attempt State |             Description                                                                                                                |                    Note                                            |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Pending**           | The customer has viewed the transaction details.                                                                                       | <p><br></p>                                                        |
| **Failed**            | Payment failed due to various reasons, such as insufficient funds and other related issues.                                            | The number of attempts is determined by the payment configuration. |
| **Canceled**          | The customer has chosen to click the cancel button.                                                                                    | <p><br></p>                                                        |
| **Success**           | The payment attempt was successful (i.e., successful pay).                                                                             | <p><br></p>                                                        |
| **Error**             | When a payment error has been encountered from PG side, such as the PG Service being unavailable, experiencing a network issue, … etc. | <p><br></p>                                                        |
| **COD**               | Cash on Delivery                                                                                                                       |                                                                    |



{% hint style="info" %}
When **COD** (Cash on Delivery) is used for cash payments, a payment attempt is created. This is because every change in the final state of the payment transaction must be associated with a payment attempt.
{% endhint %}

## [State Combinations](payment-transactions-states.md#state-combinations)

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

![](<../../.gitbook/assets/6 (2).png>)
