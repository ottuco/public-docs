# Payment Methods

In the intricate realm of online transactions, a versatile payment experience is essential. \
The `Payment Methods API` streamlines this process, allowing for effortless automation. by simply integrating with specified operation, [customer\_id](checkout-api.md#customer_id-string-optional), [currencies](checkout-api.md#currency_code-string-required), [payment gateway names](../user-guide/payment-gateway.md) and desired [payment plugins](../user-guide/plugins/), merchant can harness the full power of Ottu's payment automation, all while keeping his existing environment intact. no extra effort, just seamless integration.

## [Setup](payment-methods.md#setup)

#### [Enabling the Plugin](payment-methods.md#enabling-the-plugin)

Ensure the relevant plugin ([E-Commerce](../user-guide/plugins/#e-commerce) or [Payment Request](../user-guide/plugins/#bulk-payment-request)) is activated in the Admin Panel under the Plugin Config section. For detailed activation steps, refer to the [Plugin Activation Guide](../user-guide/plugins/#adding-or-removing-plugins).

#### [Activating Payment Gateway Codes](payment-methods.md#activating-payment-gateway-codes)

Activate the [payment gateway](../user-guide/payment-gateway.md) ([pg\_codes](checkout-api.md#pg_codes-array-required)) you intend to use. Ensure all desired `pg_codes` are set to ‘`active`’ status in your configuration.

#### [Understanding the `type` Filter](payment-methods.md#understanding-the-type-filter)

The `type` filter determines the mode of your environment:

* **Development Mode:** Configure the `type` to `sandbox` mode for conducting tests with sandbox transactions instead of live transactions.
* **Production Mode:** Set the `type` to `production` for handling real transactions in a live environment.

{% hint style="info" %}
Ensure you’re in the correct mode before initiating payments.
{% endhint %}

### [Boost Your Integration](payment-methods.md#boost-your-integration)

To accelerate the integration process, consider using our official packages. These packages handle the heavy lifting involved in integrating with our API. They encapsulate much of the API interaction logic, allowing you to focus on core business logic.

**Available Packages:**

* **Python SDK:** Simplifies access to payment methods using a Pythonic interface, enhancing developer productivity by abstracting complex API calls. [Learn more](https://github.com/ottuco/ottu-py#accessing-the-payment-methods).
* **Django SDK:** Integrates payment methods seamlessly into Django projects, providing tools and utilities specific to Django, which streamline payment processes. [Explore details](https://github.com/ottuco/ottu-py?tab=readme-ov-file#django-integration).

While these packages offer substantial speed and convenience benefits, understanding the foundational concepts and structures outlined in the documentation is crucial. This ensures that you can effectively integrate and use the packages in a robust and maintainable manner.

## [Authentication](payment-methods.md#authentication)

**Supported Methods**

* [Private API Key](authentication.md#private-key-api-key)
* [Basic Authentication](authentication.md#basic-authentication)

For detailed information on authentication procedures, please refer to the [Authentication documentation](payment-methods.md#authentication).

## [Permissions](payment-methods.md#permissions)

Ensuring the right level of access is crucial for maintaining the security and integrity of your payment processes. The `Payment Methods API` provides two authentication methods, each with its own set of permissions:

#### Private API Key

* **Privileges:** This method grants super admin privileges to the caller. It provides unrestricted access to all available payment methods and associated functionalities.
* **Usage:** Given its broad access, it’s essential to handle API keys with utmost care. Ensure they are securely stored and only shared with trusted individuals or systems.

#### BasicAuth

* **Privileges:** `BasicAuth` is user-centric, meaning it’s tied to a specific user profile. Access to payment methods can be restricted based on the user’s assigned permissions.
* **Setting Permissions:** To grant a user access to a specific payment method, assign the permission: "**Can use pg\_code**". Here, the [pg\_code](checkout-api.md#pg_codes-array-required) represents the unique code of the [payment gateway](../user-guide/payment-gateway.md) or merchant ID (`MID`).\
  For instance, if you want to grant a user access to a payment gateway with the code ‘`credit-card`’, you would assign them the permission “**Can use Credit Card**”.
* **Usage:** Since `BasicAuth` is user-specific, it’s suitable for scenarios where you want to provide selective access to different payment methods based on user roles or responsibilities.

## [How it Works](payment-methods.md#how-it-works)

The `Payment Methods API` is designed to streamline and automate the end-to-end integration with Ottu, ensuring that your payment processes remain up-to-date and efficient without the need for constant code modifications.

### [Purpose of the Payment Methods API](payment-methods.md#purpose-of-the-payment-methods-api)

1. **Automation and Flexibility:** This API serves as a foundation for other Ottu APIs, providing the necessary data to facilitate seamless transactions. By using this API, you can ensure that your integration remains current, even when there are changes in the [payment gateway](../user-guide/payment-gateway.md) configurations.
2. **Caching for Efficiency:** Given that the data from the `Payment Methods API` doesn’t change frequently, it’s recommended to cache the response for 24 hours. This approach enhances performance and reduces unnecessary API calls. However, ensure you have a mechanism to clear the cache when immediate updates are required.
3. **Tailored Customer Experience:** With various filters available, you can customize the payment methods presented to the customer. For instance, if you’re setting up a customer for recurring payments, you can filter out payment methods that don’t support [tokenization ](tokenization.md)using the `tokenizable`=`true` filter.
4. **Versatility:** The `Payment Methods API` isn’t just a prerequisite for the [Checkout API](checkout-api.md). It’s also essential for other APIs, such as the [User Cards API](user-cards.md).

### [Usage Scenarios](payment-methods.md#usage-scenarios)

* **Currency-Specific Payments:** If you want to allow payments only in a specific currency, like USD, use the filter `currencies`=\["`USD`"]. This will return only the payment methods that support USD.
* **Recurring Payments:** When setting up a customer for recurring payments, use the `tokenizable`=`true` filter. This ensures that the customer can set up a recurring payment using methods that support tokenization.
* **Customized Payment Options:** If you have a history of the payment methods a customer has used, you can tailor the options presented to them during checkout. Instead of showing numerous payment methods, display only those they’ve used in the past.
* **Specific Payment Types:** For purchases where only specific payment types, like Debit Cards, should be allowed, use filters like `pg_names`=\["`kpay`"] to retrieve only the desired payment methods.

#### Example:

To retrieve payment methods that support `USD` and `tokenization` for `e-commerce` purchases:

```json
POST: https://beta.ottu.net/b/pbl/v2/payment-methods/
{
    "plugin": "e_commerce",
    "operation": "purchase",
    "tokenizable": "true",
    "currencies": ["USD"]
}
```

This request will return the `"pg_codes": ["kpay"]`, which can then be used in subsequent API calls.

**For a detailed breakdown of the API response, refer to the Open API block below.**

{% openapi-operation spec="july" path="/b/pbl/v2/payment-methods/" method="post" %}
[OpenAPI july](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/2c797c8a017d6378230381558926cadbdf6af082f709c84989e1306f34f8bec9.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250729%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250729T144122Z&X-Amz-Expires=172800&X-Amz-Signature=c84460f61628c6c59d8cf47f0ab6feb292ab479a8b842a614d9635a7e35ebf97&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [Guide](payment-methods.md#guide)

The `Payment Methods API` provides a wealth of information about each available payment method. It’s not just about determining which payment methods are available; it’s about understanding the capabilities and features of each method in-depth.

#### Key insights you can glean from the API response include

* **Capabilities:** Understand what each payment method is capable of. This includes operations like [refunds](operations.md#refund), [voids](operations.md#void), and [captures](operations.md#capture).
* **Wallet Integrations:** Determine which payment methods support popular digital wallets like Apple Pay and Google Pay.
* **Supported Currencies:** Identify the currencies each payment method can handle. This is crucial for businesses operating in multi-currency environments.
* **And More:** The API provides a plethora of other details that can be instrumental in tailoring the payment experience for your customers.

With such granular control and detailed insights at your fingertips, you can truly customize and optimize the payment process to align with your specific business needs and offer an unparalleled payment experience to your customers.

### [Streamlining Checkout Process by Payment Methods API](payment-methods.md#streamlining-checkout-process-by-payment-methods-api)

**Use Case:**

Streamlined Checkout Experience.

**Scenario:**&#x20;

An e-commerce merchants aim to simplify and accelerate the checkout process for their customers, ensuring that they're presented with the most relevant and up-to-date payment methods.

**Steps:**

1.  #### Initiate the Payment Methods API

    Start by making a call to the `Payment Methods API`. This retrieves the most current and applicable payment methods based on specified parameters such as `customer_id`, `currency`, and `plugin`. For instance, to cater to online shoppers, you could use the plugin filter to retrieve only e-commerce payment methods.
2.  #### Invoke the Checkout API

    Using the payment methods gleaned from the above step, you then call the [Checkout API](checkout-api.md). Here, you ensure the chosen payment method is passed to this API call. \
    For example, if the `Payment Methods API` returned '`PG001`' as an available payment method code, you would incorporate this code into the `Checkout API` call.
3.  #### Outcome

    By integrating this automated process, customers experience a smoother checkout journey. As they proceed to payment, they're presented with the most current and applicable payment options, thereby enhancing their shopping experience and increasing the likelihood of transaction completions.

### [Payment Methods API Workflow Diagram](payment-methods.md#payment-methods-api-workflow-diagram)

<figure><img src="https://documents.lucid.app/documents/cd77bada-241a-4e89-a9a7-8d62e631f669/pages/0_0?a=275&#x26;x=485&#x26;y=-223&#x26;w=353&#x26;h=594&#x26;store=1&#x26;accept=image%2F*&#x26;auth=LCA%206ff3c290a2d531335d860fc723b4d861ba7077e733a91948bace036dd449f88e-ts%3D1698044875" alt=""><figcaption></figcaption></figure>

### [**Use Cases Examples**](payment-methods.md#use-cases-examples)

#### [**Use Case 1:** Expanding Payment Options ](payment-methods.md#use-case-1-expanding-payment-options)

**Scenario:**&#x20;

A merchant aims to provide a diverse array of payment options to accommodate an extensive clientele, particularly those engaged in e-commerce for widespread product purchases.&#x20;

**Steps:**

1.  #### The Merchant Initiate the Payment Methods API

    Call `Payment Methods API` to retrieve all available payment methods.
2.  #### Using the API's Filters

    Utilize filters such as [customer\_id](checkout-api.md#customer_id-string-optional), [currency](checkout-api.md#currency_code-string-required), and [plugin](../user-guide/plugins/), the merchant selects and integrates desired payment methods into their platform in our case merchant should use plugin filter to get only e\_commerce payment methods.
3.  #### Expanding Payment Choices

    As customers check out, they see a broader range of payment  options, increasing the likelihood of successful transactions.

***

#### [**Use Case 2:** Dynamic Payment Updates](payment-methods.md#use-case-2-dynamic-payment-updates)

**Scenario:**&#x20;

The merchant's gateway settings or `MID` configuration has been changed, and they want these updates to be automatically reflected without manually intervening in the payment methods section they will show in customer checkout page.

**Steps:**

Once integrated with the `Payment Methods API`, any change in the gateway settings or `MID` configuration is auto-reflected in the merchant's payment methods section as Ottu will automatically return the updated details of payment method based on changes done on `MID.Merchants` don't have to initiate any manual updates; the API handles the seamless integration.

**Benefits:**

Reduces the manual work involved in updating payment methods. Ensures customers always have the latest and most secure payment options available.

***

#### [Use Case 3: Facilitating Subscription Payments with Tokenization Filter](payment-methods.md#use-case-3-facilitating-subscription-payments-with-tokenization-filter)

**Scenario:**&#x20;

An online subscription service wants to offer their customers the option to have their payments automatically debited at regular intervals, ensuring a seamless and uninterrupted service experience.

**Steps:**

1.  #### Tokenization Filter

    The merchant uses the `Payment Methods API` with the \``tokenizable`\` flag enabled.\
    The API filters and returns only the [payment gateways](../user-guide/payment-gateway.md) (`pg_codes`) that support the tokenization feature.
2.  #### Incorporating Tokenization Gateways into Checkout Process

    The merchant integrates these tokenization gateways into their checkout process.
3.  #### Customer Side

    When a customer opts for a subscription service, they are presented only with the payment methods that support tokenization. The customer chooses their preferred payment method from the filtered list and sets up their subscription.

The chosen payment method is now set to automatically debit the subscription amount at the specified intervals, ensuring uninterrupted service for the customer and consistent revenue for the merchant.

***

#### [Use Case 4: Managing Subsequent Charges with Tokenized Card Filtering](payment-methods.md#use-case-4-managing-subsequent-charges-with-tokenized-card-filtering)

**Scenario:**&#x20;

An e-commerce platform offers customers the convenience of saving their card details for faster checkout in future transactions. To maintain trust and enhance the user experience, the platform wishes to ensure that subsequent charges using saved cards are only done on those cards which are [tokenization](tokenization.md) enabled by the customer.

**Steps:**

1.  #### Fetching Tokenized `pg_codes` with `Payment Methods API`

    When planning for subsequent charges, the merchant uses the `Payment Methods API` to retrieve tokenization supported `pg_codes`.
2.  #### Passing Tokenization `pg_codes` to `User Cards API`

    The retrieved tokenization `pg_codes` are then passed to the [User Cards API](user-cards.md#fetch-cards). The `User Cards API` filters and returns only those saved cards that are enabled for [tokenization](tokenization.md) by the customers.
3.  #### Subsequent Transactions

    For [subsequent transactions](auto-debit.md#subsequent-payments-1), the system only charges the customer's cards that are both saved and have tokenization enabled.

Customers are assured that only their approved and tokenization enabled cards will be charged, ensuring transparency and trust.

## [Best Practices](payment-methods.md#best-practices)

#### [**Caching API Call Responses**](payment-methods.md#caching-api-call-responses)

Given the infrequent changes in values — primarily when new MIDs are issued — it’s prudent to cache the API call response. This minimizes unnecessary and redundant calls to the API, especially considering the rare changes.

**Recommendations**

* Cache the API call response for an extended period, ranging from 24 hours to 1 week. This ensures that you’re not querying the API for every payment, which is inefficient since the data remains largely static.

#### [**Individual Caching for Filters**](payment-methods.md#individual-caching-for-filters)

Always cache responses based on the specific filters you apply during API calls. If you’re employing the API with diverse filters based on various scenarios, it’s crucial to cache each response separately.

**Why is this important?**\
If you cache all responses under a single key, regardless of the filters used, you risk retrieving incorrect or irrelevant data. This can lead to inaccurate payment processing or even transaction failures. Caching each filter’s response under a unique key ensures data integrity and relevance.

#### [**On-Demand Cache Clear Mechanism**](payment-methods.md#on-demand-cache-clear-mechanism)

Implement a mechanism to clear the cache on demand. In instances where a change for `MID` is enacted in the Ottu admin panel, this mechanism should be triggered to force update the cache with the latest data.

**Benefits**

* Guarantees that your system always has the most up-to-date information.
* Prevents potential transaction issues stemming from outdated `MIDs`.

By adhering to these best practices, you’ll ensure efficient API usage, maintain data accuracy, and optimize payment processing.

## [FAQ](payment-methods.md#faq)

#### :digit\_one: [What is the main purpose of the Payment Methods API?](payment-methods.md#what-is-the-main-purpose-of-the-payment-methods-api)

The `Payment Methods API` is designed to streamline the process of online transactions by offering an effortless automation system for payments.

#### :digit\_two: [Do I need to make changes to my existing environment when integrating with the Payment Methods API?](payment-methods.md#do-i-need-to-make-changes-to-my-existing-environment-when-integrating-with-the-payment-methods-api)

No, the `Payment Methods API` allows for a seamless integration without requiring changes to your current environment. You only need to integrate with specific operation, [customer\_id](checkout-api.md#customer_id-string-optional), [currencies](checkout-api.md#currency_code-string-required), and desired payment [plugins](../user-guide/plugins/).

**Conclusion**

In conclusion, the `Payment Methods API` offers dynamic adaptability. Any modifications made to the gateway settings or `MID` configuration will be automatically reflected in the API. This ensures a hassle-free experience, eliminating the need for constant developer intervention. Whether you're adding a new gateway, introducing a new method, or incorporating an additional payment method for your store, there's no need for manual updates in your shop's integration. Once integrated with this API, any changes made will be seamlessly displayed. Simplifying the process, the `Payment Methods API` guarantees efficiency and adaptability for your transaction needs.
