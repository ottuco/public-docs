# Tokenization

One of the key features of Ottu is the ability to streamline the checkout process via tokenization. This feature is currently compatible with MasterCard, Visa, and STC Pay, with further options planned for future inclusion.

## [What is Tokenization?](tokenization.md#what-is-tokenization)

Consider tokenization as a powerful security vault where all sensitive data is replaced with a secure code, or ‘token’. Just like a vault secures precious jewels by keeping them out of sight and replacing them with a secure code, tokenization replaces sensitive card data with unique identifiers, protecting it from prying eyes. This process is paramount for securing payments, offering merchants the luxury of securely processing transactions without having to be PCI DSS compliant.

{% hint style="info" %}
**Tokenization Enhances Security:** Tokenization significantly reduces the risk associated with handling sensitive customer card data. Instead of storing the actual card details, a unique token is generated and used for future transactions, thus enhancing the security of your payment processes.
{% endhint %}

&#x20;With Ottu, we handle this complexity, so you don’t have to! To activate tokenization or for any inquiries, KSA merchants can reach us at support@ottu.com.

## [How Does it Work?](tokenization.md#how-does-it-work)

To kick off the process, use the [Checkout API](rest-api/checkout-api.md) to create a payment session ([session\_id](rest-api/checkout-api.md#session\_id-string-mandatory)) with a customer ID ([customer\_id](rest-api/checkout-api.md#customer\_id-string-optional)) associated with a Merchant Identification Number (MID) that supports and has tokenization enabled. The presence of the customer ID is critical, as it’s the primary way to associate saved cards with a specific customer. The payment you’ve just initiated can then be rendered using the [Checkout SDK](checkout-sdk/). Remember, the Card Verification Value (CVV) may be required based on your [Payment Gateway](../user-guide/payment-gateway.md) configuration – this can be set by contacting our technical team.

## [The Process](tokenization.md#the-process)

When the merchant utilizes the Checkout SDK and fulfills all the necessary requirements, the “Save Card” button will become available. This will enable the customers with the choice to securely save their payment card information for potential future transactions. After a customer decides to save their card and successfully completes a payment, the card details are converted into a tokenized form for added security. For upcoming transactions, if the merchant includes the same [`customer_id`](rest-api/checkout-api.md#customer\_id-string-optional) as in the preceding transaction within the request payload, all cards linked to that `customer_id` are presented to the customer. This streamlines the process of choosing and utilizing any of the stored cards for payment. Only the last four digits of each card are revealed. Subsequently, customers can employ any of these saved cards for payment, with the CVV being optional based on the acquiring bank's policies. Our Checkout SDK seamlessly handles challenges like 3D Secure or One-Time Password (OTP) codes, relieving you of the complexities of payment processing.

### [Implementation](tokenization.md#implementation)

Simplifying your checkout process with tokenization is easy with Ottu. Here’s a step-by-step guide on how to get started:

#### **1. Create a Payment Link**

The first step involves creating a payment link via the [Checkout API](rest-api/checkout-api.md). Make sure to include the [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional) parameter, and the [pg\_code](rest-api/checkout-api.md#pg\_codes-array-required) you’re using is enabled for tokenization.\
Request Example:

```json
{
    "type": "payment_request",
    "pg_codes": ["mpgs", "credit-card", "stc-pay"],
    "amount": "19.000",
    "customer_id":"customer_sample_id",
    "order_no": "token_showcase",
    "currency_code": "KWD"
}
```

#### 2. Access the Checkout Page

Once you’ve created the payment link, the next step is to access the [checkout\_url](rest-api/checkout-api.md#checkout\_url-string-mandatory) from the [Checkout API](rest-api/checkout-api.md) response. This action will open the Ottu checkout page where the [Checkout SDK](checkout-sdk/) will render all available payment methods. **To save a card**, select a hosted payment method that has the save card input enabled, or the Ottu pg hosted page, based on the [pg\_code](rest-api/checkout-api.md#pg\_codes-array-required) you input in the API. If the customer chooses to save the card and performs a successful payment, the card details will be saved, and a token will be associated with the [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional).

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Successful Payment is a Prerequisite:** A saved card, represented by a unique token, can only be created after the customer completes a successful payment transaction. This process ensures the validity of the card and enables tokenization.
{% endhint %}

#### 3. Use a Saved Card

For subsequent transactions, repeat the same API call as in [step 1](tokenization.md#1.-create-a-payment-link) using the same [customer\_id](rest-api/checkout-api.md#customer\_id-string-optional) and [pg\_code](rest-api/checkout-api.md#pg\_codes-array-required). When reaching the Ottu checkout page again, the previously saved card will be available as a new payment method. The customer can simply click on it to process the payment immediately.

<figure><img src="broken-reference" alt=""><figcaption></figcaption></figure>

By following these steps, you can easily streamline your payment process, ensuring that customers have a seamless checkout experience. As mentioned, we’ll be providing visual aids between each step to further guide you in implementing this feature. Remember, at Ottu, we are always here to assist you in navigating the complexities of online transactions, so don’t hesitate to reach out if you need any help.

## [FAQ](tokenization.md#faq)

#### :digit\_one: [Can I use tokenization if my payment gateway doesn’t support it?](tokenization.md#can-i-use-tokenization-if-my-payment-gateway-doesnt-support-it)

The tokenization feature is currently available for specific [payment gateways](../user-guide/payment-gateway.md). Please contact our support team for the most up-to-date information.

#### :digit\_two: [What happens if a customer doesn’t want to save their card details?](tokenization.md#what-happens-if-a-customer-doesnt-want-to-save-their-card-details)

The ‘Save Card’ option is entirely voluntary. If a customer chooses not to save their card, the standard checkout process will proceed without tokenization.

#### :digit\_three: [What if a customer wants to remove a saved card?](tokenization.md#what-if-a-customer-wants-to-remove-a-saved-card)

A customer can remove their saved cards at any point. For detailed information on this, please refer to the [User Cards](rest-api/user-cards.md) and [Checkout SDK](checkout-sdk/) documentation.

#### :digit\_four: [Is tokenization safe?](tokenization.md#is-tokenization-safe)

Absolutely. Tokenization is a security measure that replaces sensitive card details with a unique token, thus ensuring that the actual card information isn’t exposed.

#### :digit\_five: [Can I use tokenization for recurring payments?](tokenization.md#can-i-use-tokenization-for-recurring-payments)

Yes, tokenization is an ideal feature for setting up recurring payments. Please refer to our [Auto Debit](rest-api/auto-debit.md) documentation section for more details.

#### :digit\_six: [Can I use tokenization even if I’m not PCI DSS compliant?](tokenization.md#can-i-use-tokenization-even-if-im-not-pci-dss-compliant)

Yes! That’s the whole point. Ottu does all the heavy lifting for you, so you can simply use tokenization even if you’re not PCI DSS compliant.

#### :digit\_seven: [Can I save the saved card/token in my database even if I’m not PCI DSS compliant?](tokenization.md#can-i-save-the-saved-card-token-in-my-database-even-if-im-not-pci-dss-compliant)

Absolutely. You can safely store tokens in your database even without PCI DSS compliance. It’s important to remember that these tokens are not actual card numbers but are unique identifiers created through the tokenization process. They don’t carry the same security risks as storing raw card details.

#### :digit\_eight: [Can I use the Auto-Debit feature even if I’m not PCI DSS compliant?](tokenization.md#can-i-use-the-auto-debit-feature-even-if-im-not-pci-dss-compliant)

Absolutely. With Ottu, you don’t have to worry about PCI DSS compliance. Our platform securely handles all the sensitive data and never exposes this information to the merchant. This means you can safely implement the [auto-debit](rest-api/auto-debit.md) feature just like any other [REST API](rest-api/).

#### :digit\_nine: [Can I store card tokens in my database if I’m not PCI DSS compliant?](tokenization.md#can-i-store-card-tokens-in-my-database-if-im-not-pci-dss-compliant)

Yes, you certainly can. Ottu uses tokenization to ensure that your customer’s Primary Account Number (PAN) is never exposed. What you receive and can safely store is a token, not an actual card number. It’s structured like a card number but doesn’t carry the same security risks. If you’re curious about how tokenization works, you can check out this [Wikipedia article](https://en.wikipedia.org/wiki/Tokenization\_\(data\_security\)) for a deeper dive.

#### :digit\_one::digit\_zero: [When should I save the card token in my database?](tokenization.md#when-should-i-save-the-card-token-in-my-database)

The optimal time to save the card token in your database is immediately after the first payment against the subscription that you plan to [auto-debit](rest-api/auto-debit.md). While it’s not strictly necessary—you can always fetch this information through the [User Cards](rest-api/user-cards.md) API and Payment Methods APIs—it does streamline your processes and reduce development complexity.

#### :digit\_one::digit\_one: [How can I add a new card for a customer?](tokenization.md#how-can-i-add-a-new-card-for-a-customer)

Currently, the only way to save a new card is by having the customer successfully complete a payment with it. At the moment, it’s not possible to just save new card details directly.

#### :digit\_one::digit\_two: [Do I need to use the Checkout SDK to display payment options to the customer?](tokenization.md#do-i-need-to-use-the-checkout-sdk-to-display-payment-options-to-the-customer)

No, it’s not mandatory to use the Checkout SDK. You can control the payment process using the responses from the Checkout API. However, it’s worth noting that the Checkout SDK simplifies the UI implementation and is necessary for certain payment methods such as Apple Pay, Google Pay, STC Pay, and others. While it’s recommended to use the Checkout SDK for its simplicity and comprehensive features, the choice ultimately lies in your hands based on your specific needs.

## What’s Next?

To explore the full potential of tokenization, check out the [Checkout SDK](checkout-sdk/), [User Cards](rest-api/user-cards.md) Section and the [Auto Debit](rest-api/auto-debit.md) section of our documentation. These sections provide a deep dive into managing saved cards and automating recurring payments respectively, helping you leverage the best of Ottu’s payment solutions. Whether you’re a merchant looking to provide your customers with a seamless checkout experience or a developer eager to integrate state-of-the-art payment solutions, Ottu’s tokenization feature is your go-to choice for secure, hassle-free transactions.

{% hint style="info" %}
**Efficiency and Customer Convenience:** By enabling tokenization, you simplify the checkout process for returning customers, as they won’t need to re-enter their card details for every transaction. This not only speeds up the payment process but also improves the customer experience.
{% endhint %}
