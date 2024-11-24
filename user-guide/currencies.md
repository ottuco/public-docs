# Currencies

Discover Ottu, the groundbreaking multi-currency Online Payment Management System (OPMS) developed to revolutionize your online business. Ottu empowers merchants with unprecedented flexibility, allowing them to effortlessly accept payments in multiple currencies. With our advanced [currency exchange](currencies.md#currency-exchanges) options, businesses can seamlessly manage online payments with a wide array of currency options across borders, ensuring a smooth and user-friendly experience for customers worldwide, with the ability to customize the markup fees according to their preferences. Say goodbye to limitations and unlock a world of endless payment possibilities with Ottu, your ultimate solution to currency restrictions.

## [Currency Configurations](currencies.md#currency-configurations)

Ottu offers a convenient [payment gateway](payment-gateway.md) with preconfigured currency settings. For instance, the default currency for KNET Payment Gateway is the Kuwaiti Dinar (KWD). This feature guarantees smooth transactions and enhances user experience, simplifying the management of online payments.

## [Currency Configuration Page](currencies.md#currency-configuration-page)

Once you access the Ottu Dashboard, click on the three dots positioned in the right corner of the page. This action will lead you to the **Administration Panel**, where you'll find a range of exciting options. Within the Administration Panel, you'll discover the Currency Tab awaiting your attention.

<figure><img src="https://lh6.googleusercontent.com/urGtHZvozcu5GUVIX5V3oUPURSXZfMp6e91ZopAcTYW2bd0YMz9zt4tcxwxzFvUV5fsYGjBJ-1vizF2YQWZrVRJ-zZs2raf-La9OXa0es7B8fVtMCJqtbt3h6pGU0lGPEw1HjoKmB-FQDfWTcucudow" alt=""><figcaption></figcaption></figure>

There are three types of currency configuration:

* [Currencies](currencies.md#currencies) (adding and managing different currencies)
* [Currency exchanges](currencies.md#currency-exchanges) (setting up currency exchange)
* [Exchanges config](currencies.md#exchange-configuration) (defining currency rules for the [payment gateway](payment-gateway.md))

Each type serves a distinct purpose and aspect in enabling smooth and efficient transactions. These configurations empower merchants to navigate the complexities of multiple currencies, facilitate currency exchanges, and define specific currency rules for payment gateways.

<figure><img src="../.gitbook/assets/1 copy (2).gif" alt=""><figcaption></figcaption></figure>

### [Currencies](currencies.md#currencies)

In Ottu dashboard, you can find a wide range of currencies to choose from. If you wish to add a new currency, just follow these simple steps:

**Step 1:**

Click on the `Add Currency` button to include a new currency.

<figure><img src="../.gitbook/assets/2 (2) copy (2).png" alt=""><figcaption></figcaption></figure>

**Step 2:**

Mark the active option and then click on `Save` to finalize the process.

<figure><img src="../.gitbook/assets/3 (2) copy (1).png" alt=""><figcaption></figcaption></figure>

### [Currency Exchanges](currencies.md#currency-exchanges)

Sometimes, merchants may prefer receiving payments in their local currency, even when the customer resides in a different country. To facilitate this payment process, Ottu offers a convenient currency exchange service that benefits merchants and customers, enhancing seamless payment processes. Occasionally, when the bank acts as an intermediary between the merchant and the customer, they may impose a markup fee to cover the currency exchange fees.\
To configure the currency exchange feature, simply follow these **three steps:**

**Step 1**

Click on `Add Currency Exchange`.

<figure><img src="../.gitbook/assets/4 (2) copy (1).png" alt=""><figcaption></figcaption></figure>

**Step 2**

Enter the necessary parameters as required.

<figure><img src="https://lh3.googleusercontent.com/_LLoqGD7wYxqAstnAJB1IXs3rvCRVwf1uslT-7oaXkQeQ9rZxxuTv-FDZEuy_ZkNq50eCxQGbXmJIYPbvg_2HOWZ8m7ddciiaO_7-L8NDJ1aWfhsPAPU3fCznDSoW25COGlz47PF5xtLtm6gst7RC80" alt=""><figcaption></figcaption></figure>

**Step 3**

&#x20; Finally, click on `Save`.

<figure><img src="../.gitbook/assets/6 (1) copy (1).png" alt=""><figcaption></figcaption></figure>

### [Exchange Configuration](currencies.md#exchange-configuration)

The configuration of the [payment gateway](payment-gateway.md) is closely tied to the configuration of the exchange. This configuration determines the currency rules that should be applied to any payments made through that gateway.\
Adding an exchange configuration is a straightforward process consisting of just **two steps:**

**Step 1**

Click on the `Add Exchange Config` button.

<figure><img src="../.gitbook/assets/7 (1) copy (1).png" alt=""><figcaption></figcaption></figure>

**Step 2**

Provide the necessary information and click on the `Save` button.

<figure><img src="../.gitbook/assets/8 (1) copy (1).png" alt=""><figcaption></figcaption></figure>

#### Field Descriptions

<table><thead><tr><th width="178">Field</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>This field is used to identify the <a href="currencies.md#currency-exchanges">exchange configuration</a>.</td></tr><tr><td>Default currency</td><td>The default currency of the <a href="payment-gateway.md">payment gateway</a>.</td></tr><tr><td>Currencies</td><td>These are the payment currencies that the merchant wants to deal with.</td></tr><tr><td>Fee type</td><td><p>There are two types of fees, <strong>Fixed fees</strong> and <strong>Percent fees</strong>.</p><p><strong>Fix fee:</strong> A predetermined amount that is added to the payment.</p><p><strong>Percent fee:</strong> An additional amount calculated based on a percentage of the payment.</p></td></tr><tr><td>Fees Description</td><td>Identification of the fee according to the merchant’s purposes</td></tr><tr><td>Work as</td><td><p>The exchange rate can be determined in two ways: <strong>Online</strong> or <strong>Local</strong>.</p><p><strong>Online:</strong> Utilizing online services to define the exchange rate.</p><p><strong>Local:</strong> Allows the merchant to manually define the exchange rate.</p></td></tr><tr><td>Fix fee</td><td>Fixed amount</td></tr><tr><td>Percentage fee</td><td>Dynamic amount</td></tr><tr><td>Charge default currency</td><td>When this event is activated, a fee will be added to the original amount, even if the payment transaction is conducted in the default currency of the <a href="payment-gateway.md">payment gateway</a>.</td></tr></tbody></table>

{% hint style="warning" %}
Foreign currencies are not functioning properly in [operations](../developer/operations.md).
{% endhint %}
