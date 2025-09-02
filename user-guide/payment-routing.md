# Payment Routing

**Route every card payment to the best path.**\
Ottu Router evaluates each payment in real time (BIN, country, scheme, amount, recent health) and sends it through the **Payment Connection** most likely to succeed—at the best cost—with a safe fallback.

> **Payment Connection**: your acquiring/gateway connection used to process a payment.

## [Why It Matters](payment-routing.md#why-it-matters)

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><mark style="background-color:green;"><strong>Increased Transaction Success Rates</strong></mark></td><td><em>Routes to the best connection, reducing declines.</em></td></tr><tr><td><mark style="background-color:blue;"><strong>Cost Efficiency</strong></mark> </td><td><em>Steers to lower-fee connections to cut processing costs.</em></td></tr><tr><td><mark style="background-color:orange;"><strong>Enhanced Customer Satisfaction</strong></mark></td><td><em>Faster, more successful checkouts improve the experience.</em></td></tr><tr><td><mark style="background-color:purple;"><strong>Scalability and Flexibility</strong></mark></td><td><em>Adapts to volume spikes and new markets without manual tuning.</em></td></tr><tr><td><mark style="background-color:red;"><strong>Risk Mitigation</strong></mark></td><td><em>Avoids single-point dependence by switching based on real-time health.</em></td></tr><tr><td><mark style="background-color:blue;"><strong>Market Expansion</strong></mark></td><td><em>Prefer local connections to improve cross-border approvals.</em></td></tr></tbody></table>

## [How it works](payment-routing.md#how-it-works)

Ottu Router runs each payment through four clear stages

> **Routing Sequence:**\
> &#xNAN;_&#x50;ayment →_ [_Blockers_](payment-routing.md#blocker-rules) _→_ [_Routing Rules_](payment-routing.md#routing-rules) _→_ [_Routing Strategies_](payment-routing.md#routing-strategies) _→ (Chosen Payment Connection |_ [_Fallback_](payment-routing.md#fallback)_) → Processor._

* [Blockers](payment-routing.md#blocker-rules)**:** Guardrails that can stop a payment outright&#x20;
  * Blocks or allows transactions by card BIN range ([BIN](payment-routing.md#bin-access-control)).
  * Controls transactions by issuing country ([country](payment-routing.md#country-based-routing)).&#x20;
  * Filters by card network (e.g., Visa, Mastercard) ([scheme guardrails](payment-routing.md#card-scheme-restriction)).
* [Routing Rules](payment-routing.md#routing-rules)**:** Decide which payment connections to rout the payment through.
  * Routes by BIN match ([BIN-Based Routing](payment-routing.md#bin-based-routing)).
  * Routes by issuing country ([Country-Based Routing](payment-routing.md#country-based-routing)).
  * Routes by card type (Visa, Mastercard) ([Supported Schemes](payment-routing.md#supported-schemes))**.**&#x20;
  * Backup route if no match ([Fallback Routing](payment-routing.md#fallback-routing)).
* [Routing Strategies](payment-routing.md#routing-strategies)**:** Pick the best Payment Connection from that eligible set.
  * Load balance ([round-robin](payment-routing.md#round-robin)).
  * Value bands ([route by amount](payment-routing.md#transaction-size)).
  * Health-aware ([skip connections with elevated errors](payment-routing.md#exclude-unavailable)).
* [Fallback](payment-routing.md#fallback): If no payment  connection qualifies, route to the default payment connection.

{% hint style="warning" %}
The Router always logs its decision: which blockers applied, which connections were eligible, which strategy was used, and why the final connection was chosen.
{% endhint %}

The below diagram shows how Ottu Router handles a transaction  from the moment it starts, through rule checks and decision logic, to the final payment sent to the bank.

<figure><img src="../.gitbook/assets/Untitled diagram _ Mermaid Chart-2025-09-02-084927.png" alt=""><figcaption></figcaption></figure>

### [**Blocker Rules**](payment-routing.md#blocker-rules)

These rules filter transactions before routing begins.

{% tabs %}
{% tab title="BIN Access Control" %}
Allows or blocks transactions based on the card's BIN range.
{% endtab %}

{% tab title="Geographic Restriction" %}
Restricts or permits transactions based on card-issuing countries.
{% endtab %}

{% tab title="Card Scheme Restriction" %}
Filters transactions by supported card networks such as Visa or Mastercard.
{% endtab %}
{% endtabs %}

### [**Routing Rules**](payment-routing.md#routing-rules)

These rules assign valid transactions to the correct payment connection.

{% tabs %}
{% tab title="BIN-Based Routing" %}
Routes a transaction to a specific payment connection based on BIN match.
{% endtab %}

{% tab title="Country-Based Routing" %}
Directs transactions to payment connections depending on the card's issuing country.
{% endtab %}

{% tab title="Supported Schemes" %}
Routes based on card type like Visa or Mastercard.
{% endtab %}

{% tab title="Fallback Routing" %}
Acts as a backup route when no other rule matches.
{% endtab %}
{% endtabs %}

### [Routing Strategies](payment-routing.md#ro-strategies)

Once Ottu router finds valid payment connections, selection strategies decide how to pick the best one.

#### **Strategy Types:**

* **Multi-Selection**: Considers several payment connections and selects based on rule logic.

{% tabs %}
{% tab title="Exclude Unavailable" %}
- Removes payment connections with too many recent errors.
- Can blacklist for hours or until midnight.
- Automatically expires after a set time.
{% endtab %}

{% tab title="Transaction Size" %}
* Routes based on transaction value.
* Each payment connection can have a minimum and maximum amount.
{% endtab %}

{% tab title="Country Based" %}
Route payments based on card country issuer/acquirer
{% endtab %}

{% tab title="Currency Based" %}
Route payments based on acquirer payment connection vs card currency
{% endtab %}
{% endtabs %}

* **Single-Selection**: Narrows immediately to one best payment connection.

{% tabs %}
{% tab title="Round Robin" %}
- Distributes transactions evenly across payment connections.
- Balances load by cycling through gateways.
{% endtab %}

{% tab title="Fallback" %}
* Acts as a safety net.
* Sends to a default payment connection if no other options are available.
{% endtab %}
{% endtabs %}

## [FAQ](payment-routing.md#faq)

#### :digit\_one:[**What happens when a transaction exceeds the set maximum amount?**](payment-routing.md#what-happens-when-a-transaction-exceeds-the-set-maximum-amount)

If a transaction exceeds the maximum amount set for the payment connection, it will be rerouted according to the configured routing rules or fallback option.

#### :digit\_two:[**How does the Round Robin method distribute transactions?**](payment-routing.md#how-does-the-round-robin-method-distribute-transactions)

The Round Robin method distributes transactions evenly across multiple payment connections, ensuring balanced load by sequentially cycling through available gateways.

#### :digit\_three:[**What is the purpose of a Fallback option?**](payment-routing.md#what-is-the-purpose-of-a-fallback-option)

The Fallback option serves as a safety mechanism to ensure transactions still get processed by sending them to a default payment connection if no other payment connections are available.

#### :digit\_four:[**How does BIN-Based Routing work?**](payment-routing.md#how-does-bin-based-routing-work)

BIN-Based Routing directs a transaction to a specific payment connection based on the card’s BIN (Bank Identification Number). If a match is found, the transaction is routed accordingly.

#### :digit\_five:[**How does Country-Based Routing work?**](payment-routing.md#how-does-country-based-routing-work)

Country-Based Routing uses the issuing country of the card to determine the payment connection. Transactions are routed to gateways configured for that specific country.

#### :digit\_six:[**How does Supported Scheme Routing work?**](payment-routing.md#how-does-supported-scheme-routing-work)

Supported Scheme Routing filters transactions by card type (e.g., Visa, Mastercard). Each scheme can be mapped to one or more payment connections.

#### :digit\_seven:[**What happens if no routing rule matches?**](payment-routing.md#what-happens-if-no-routing-rule-matches)

If no routing rule applies, the system triggers the Fallback Routing option to ensure the transaction is processed by a default connection.
