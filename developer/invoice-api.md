# Invoice API

Ottu provides businesses and developers with the capability to incorporate its Invoice functionality into their existing systems, streamlining the process of invoice generation for both online and walk-in customers.

**But what exactly is an Invoice?** An invoice is a document dispatched by a seller to a buyer, itemizing not only the quantity and cost of products or services rendered by the seller but also detailing any applicable taxes, discounts, and shipping costs. Whether issued for a traditional walk-in customer at a brick-and-mortar establishment or an online purchaser, the invoice stands as both a record of the sale and a call for payment.

Imagine you're a business owner selling a range of products online. As orders pour in, you're faced with the arduous task of creating, sending, and tracking invoices manually. This not only consumes precious time but can also lead to errors, missed payments, and dissatisfied customers.

With Ottu's advanced `Invoice API`, these challenges become a thing of the past. This robust system automates invoicing, ensuring timely dispatch, clear communication, and streamlined payment collection. Whether you're billing for a one-time purchase, tracking bulk orders, or managing advance payments, the `Invoice API` has got you covered. Through its intuitive integration, you can elevate the efficiency of your business operations and enhance the overall customer experience.

Additionally, it's worth noting that this API bears significant similarities to Ottu's [Checkout API,](checkout-api.md) offering seamless integration and familiarity for businesses already leveraging Ottu's suite of tools.

## [Setup](invoice-api.md#setup)

Before you can integrate with Ottu's `Invoice API`, several prerequisites need to be fulfilled. These prerequisites are essential to ensure that the `Invoice API` functions correctly and securely.&#x20;

1. **Payment Gateway:**\
   The `Invoice API` operates seamlessly with a [Payment Gateway](../user-guide/payment-gateway.md). Think of this gateway as the essential bridge that authorizes and facilitates transactions between your establishment and your customers. However, it's crucial to know that the `Invoice API` is explicitly tailored for a **Purchase** Payment Gateway and does not support the Authorize type.\
   For a comprehensive understanding of Payment Gateways and their inner workings, do give our [Payment Gateway](../user-guide/payment-gateway.md) User Guide a read. It provides valuable insights and clarifications.\

2. **Optional:** `round` function\
   When offering customer-facing invoice generation, it's essential to display precise figures supported by clear formulas. The `round` function is pivotal in achieving this accuracy.&#x20;

#### How `round` funtion works?

In order to ensure the precision and consistency of the financial calculations across various international transactions, Ottu relies on a standardized rounding method known as "[ROUND\_HALF\_UP](https://docs.python.org/3.8/library/decimal.html#decimal.ROUND_HALF_UP)" from Python. This method adheres to specific standards established by reputable international organizations:

* [**SIX Financial Information:** ](https://www.six-group.com/en/products-services/financial-information/data-standards.html#scrollTo=current-historical-lists)Ottu relies on currency decimal specifications provided by the SIX Group through SIX Financial Information.
* [**ISO 4217 Currency Codes:**](https://www.iso.org/iso-4217-currency-codes.html) ISO 4217 provides currency codes and their corresponding minor units, indicating the decimal precision.

The "`ROUND_HALF_UP`" method follows a careful procedure to ensure precision:

1. Establish Precision: Begin by determining the number of decimal digits permitted for the currency involved, according to [**ISO 4217 Currency Codes**](https://www.iso.org/iso-4217-currency-codes.html)**.**
2. Identify the Rounding Digit: Locate the digit immediately to the right of the desired precision. This digit is pivotal in determining the rounding outcome.
3. Apply Rounding Criteria:
   * If the digit to the right of the rounding digit is 5 or greater, the rounding digit is increased by one to ensure rounding up.
   * Conversely, if the digit to the right of the rounding digit is less than 5, the rounding digit remains unchanged, maintaining the current value.

**Why round Matters for Your Interface:** Ottu's backend utilizes round for precise financial workings. For merchants building a frontend for this Endpoint, it's vital to mirror round in real-time calculations. This ensures users will avoid discrepancy errors when interfacing with Ottu's backend. Certain optional fields in the API are designed to maintain this alignment between frontend input and backend processing, streamlining user experience and bolstering trust.\
Sources of Decimal Standards:

3. **Optional:** `Payment Methods API`\
   The [Payment Methods API](payment-methods.md) serves as an initial phase aimed at collecting dynamic parameters, particularly the [pg\_codes](checkout-api.md#pg_codes-array-required), essential for subsequent calls to the [Checkout API](checkout-api.md). Consider it a preparatory API call that furnishes you with the necessary information for the primary transactional API.\
   **Why is it considered optional?** If you are already familiar with the specific `pg_code` you plan to use consistently, you have the option to skip this step by directly storing that `pg_code` in your system. However, utilizing the `Payment Methods API` guarantees that you stay informed about the most recent payment gateways available or any modifications in the configuration. This dynamic approach ensures a more robust and adaptable integration with Ottu.

Once you've fulfilled these prerequisites, you're ready to integrate the Invoice API. The following sections of this document will guide you through the authentication process and the usage of various API endpoints.

### [Boost Your Integration](invoice-api.md#boost-your-integration)

Optimize your integration with the `Invoice API` using our official packages, designed to significantly simplify the process. These packages efficiently handle the complexities of API integration, allowing you to focus on the critical elements of your project.

**Available Packages:**

* **Python SDK:** Enhances access to invoice functionalities through a Pythonic interface, effectively abstracting the intricacies of API interactions to boost developer efficiency. [Learn more](https://github.com/ottuco/ottu-py?tab=readme-ov-file#ottu-py)
* **Django SDK:** Seamlessly integrates invoice features into Django-based applications, providing Django-specific enhancements that streamline invoice transactions. [Continue exploring](https://github.com/ottuco/ottu-py?tab=readme-ov-file#django-integration)

While these packages enhance speed and convenience, understanding the foundational concepts detailed in the documentation is essential for robust and sustainable integration.&#x20;

## [Authentication](invoice-api.md#authentication)

**Supported Methods**

* [Private API Key](authentication.md#private-key-api-key)
* [Basic Authentication](authentication.md#basic-authentication)

For detailed information on authentication procedures, please refer to the [Authentication documentation](invoice-api.md#authentication).

## [Permissions](invoice-api.md#permissions)

Below, we are explaining how permissions are managed using [Basic Authentication](authentication.md#basic-authentication) and [Private API Key](authentication.md#private-key-api-key).

**When using Basic Authentication,** it is important to ensure that the appropriate level of permissions is assigned to each user or application using the APIs. This can help to prevent unauthorized access to sensitive data.

Ottu `Invoice API` supports different levels of permissions for the [Payment Request](../user-guide/plugins/#payment-request) and[ E-Commerce](../user-guide/plugins/#e-commerce) plugins.

To create a transaction, user requires specific permissions that vary depending on the plugin in use:

* “**Can add payment requests**” for the Payment Request plugin
* “**Can add e-commerce payments**” for the E-Commerce plugin

And also needs permission to use the required Payment Gateway

* "**Can use `pg_code`**”

To add invoice, user needs to have below permission:

* “**Can add Invoice**”&#x20;

**When using the Private API Key**, all permissions are granted by default, as the private `API-Key` is considered to have admin permissions.\
\
It is recommended to regularly rotate `API keys` and follow secure password storage practices when using `Basic Authentication`.

## [How it Works](invoice-api.md#how-it-works)

The Ottu Invoice API serves as a robust mechanism for merchants to seamlessly issue invoices and manage transactions on the Ottu platform. Here's a detailed breakdown of how the Ottu `Invoice API` functions:

1. **Initiating the Invoice:**
   * Merchants utilize the `Invoice API`, passing the necessary parameters to initiate the creation of an invoice. Please check [Invoice API Parameters Specifications](invoice-api.md#invoice-api-parameters-specification).
2. **Integration with Ottu Checkout API:**
   * `Invoice API` seamlessly integrates with the Ottu [Checkout API](checkout-api.md) to generate a [checkout\_url](checkout-api.md#checkout_short_url-string-conditional).
3. **Communication to Customers:**
   * Merchants can choose to send the `checkout_url` to customers via Email or SMS using Ottu's communication channels.
   * Alternatively, merchants have the flexibility to handle the communication from their internal systems.
4. **Customer Interaction:**
   * Customers receive the `checkout_url` and, upon clicking, are redirected to Ottu's Checkout Page.
   * Here, customers can conveniently view and download the detailed invoice and proceed to make a direct payment.
5. **Transaction Completion:**
   * After completing the transaction, Ottu posts a response to the designated [webhook\_url](checkout-api.md#webhook_url-string-optional), providing real-time updates on the transaction status. Please refer to [Payment Webhooks](webhooks/payment-notification.md).
6. **Post-Transaction Customer Experience:**
   * Following transaction completion, customers are redirected either to Ottu's Response Page or a specified [redirect\_url](checkout-api.md#redirect_url-string-optional). Please refer to [Redirectional Diagram](webhooks/payment-notification.md#redirectional-diagram).
   * Ottu's Response Page offers customers the option to download the invoice along with the payment receipt for their records.

This end-to-end process ensures a seamless and secure transaction experience for both merchants and customers, integrating Ottu's `Invoice API` with Ottu `Checkout API` for efficient invoice generation, and payment processing.

### [Invoice Information](invoice-api.md#invoice-information)

In this section, we delve into the comprehensive details encompassing the creation of an invoice through Ottu's robust system. The invoicing process involves multiple layers, each designed to provide flexibility and customization to the merchant. Here's a breakdown of the key components:

**Header/Seller Information:** Merchants have the opportunity to tailor the invoice header, adding a distinctive touch to their brand. This customization includes the provision of a logo of their choice, seamlessly incorporated into the invoice.

**Invoice Information:** The essence of the invoice lies in the information provided by the merchant, encompassing Mandatory, Optional, or Calculated values.This information is systematically organized at two distinct levels—the [Item level](invoice-api.md#item-level) and the [Invoice level](invoice-api.md#invoice-level).

#### What do the Calculated values encompass?

These values, referred as `VAL CALC`, serve as validators to ensure that the real-time calculations, typically conducted by the client-side system or application, align with the values computed by Ottu's backend system. Validating these fields at both the item and invoice levels helps identify any disparities between the two, thereby ensuring accuracy and reliability throughout the invoicing process.

* #### **Item Level:**
  * Mandator&#x79;_:_ Stock Keeping Unit (SKU), Description, Quantity, Unit Price.
  * Optiona&#x6C;_:_ Tax Rate, Discount Amount, Discount Percentage.
  * VAL CAL&#x43;_:_ Total Excluding Tax, Total Including Tax, Tax Amount.
* #### **Invoice Level:**
  * Mandator&#x79;_:_ Type, Currency Code, PG Codes, Invoice Number, Due Date.
  * Optiona&#x6C;_:_ Tax Rate, Discount Amount, Discount Percentage, Shipping Excluding Tax, Shipping Method, Shipping Tax Rate.
  * VAL CAL&#x43;_:_ Total Excluding Tax, Total Including Tax, Tax Amount, Shipping Including Tax, Subtotal, Amount.

**Address Information:** Similar to the [Checkout AP](checkout-api.md)I, Ottu's invoicing system supports both [Billing ](checkout-api.md#billing_address-object-optional)and [Shipping](checkout-api.md#shipping_address-object-optional) addresses, providing a seamless and unified experience for users.

**Buyer Information:** Tailored to accommodate diverse business needs, the Buyer Information section allows for custom fields configured based on the type of the invoice `payment_request`, `e_commerce`. This flexibility ensures that merchants can capture essential buyer details relevant to their specific invoicing context.

<figure><img src="../.gitbook/assets/invoice API 1.png" alt=""><figcaption></figcaption></figure>

### [**Invoice API Parameters Specification**](invoice-api.md#invoice-api-parameters-specification)

In the previous section, we discussed the different sections and levels of invoice information, where details are sourced either from the merchant's input in the `Invoice API` request or calculated automatically. Below table categorizes the parameters in the Invoice API based on whether they are mandatory, optional, or calculated automatically ([VAL CALC](invoice-api.md#what-do-the-calculated-values-encompass)), at both the item and invoice levels.

{% hint style="info" %}
Details at the item level are encapsulated within the `invoice_items` object. The merchant can append as many items to this object as there are distinct products or items that the customer purchases.
{% endhint %}

<table data-full-width="true"><thead><tr><th width="120">Level</th><th width="211">   Mandatory Fields</th><th width="244">            VAL CALC</th><th width="330">                              Optional </th></tr></thead><tbody><tr><td>Invoice</td><td><ul><li><code>type</code></li><li><code>currency_code</code></li><li><code>pg_codes</code></li><li><code>invoice_number</code></li><li><code>due_date</code></li></ul></td><td><ul><li><code>tax_amount</code></li><li><code>shipping_inc_tax</code></li><li><code>total_excl_tax</code></li><li><code>total_incl_tax</code></li><li><code>subtotal</code></li><li><code>amount</code></li></ul></td><td><ul><li><code>company_name</code></li><li><code>tax_rate</code></li><li><code>discount_amount</code></li><li><code>discount_percentage</code></li><li><code>shipping_excl_tax</code></li><li><code>shipping_tax_rate</code></li><li><code>shipping_method</code></li></ul></td></tr><tr><td>Item</td><td><ul><li><code>sku</code></li><li><code>description</code></li><li><code>quantity</code></li><li><code>unit_price</code></li></ul></td><td><ul><li><code>tax_amount</code></li><li><code>total_excl_tax</code></li><li><code>total_incl_tax</code></li></ul></td><td><ul><li><code>tax_rate</code></li><li><code>discount_amount</code></li><li><code>discount_percentage</code></li></ul></td></tr></tbody></table>

\


**It's important to note that:**

* For optional properties, if they are not relevant, exclude them entirely from the payload. Avoid sending null or zero values; it's better to omit the property altogether if unnecessary.
* All rate or percentage-related fields are limited to two decimal places. For other monetary fields, the number of decimal places is determined by the currency of the invoice, generally according to [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html).&#x20;
* The `Invoice API` differs from the [Checkout API](checkout-api.md) in that it does not support the [`PATCH`](checkout-api.md#update-payment-transaction) method. Once an invoice is created, it is immutable and cannot be modified. Should any updates be necessary, a new invoice must be created in place of the original.

For a more detailed technical understanding and the implementation specifics of these operations, please refer to the OpenAPI schema in the API Schema Reference section below.

{% openapi-operation spec="july" path="/b/invoice/v1/invoice/" method="post" %}
[OpenAPI july](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/2c797c8a017d6378230381558926cadbdf6af082f709c84989e1306f34f8bec9.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250729%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250729T144122Z&X-Amz-Expires=172800&X-Amz-Signature=c84460f61628c6c59d8cf47f0ab6feb292ab479a8b842a614d9635a7e35ebf97&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [Guide: Step by Step](invoice-api.md#guide-step-by-step)

Dive into integrating the `Invoice API` with our concise, step-by-step guide. Designed for seamless adoption into your existing systems, this guide will take you through the essentials of setup and utilization. It's tailored to enhance your invoicing process, ensuring accuracy and efficiency in your financial transactions. Get ready to transform and streamline your billing operations with ease.

The objective of the `POST` request is to facilitate the creation of payment transactions and the subsequent generation of payment links, each of which is associated with a unique [session\_id](checkout-api.md#session_id-string-mandatory). These links can be effortlessly shared with customers through various communication channels, including email, WhatsApp, and SMS. Additionally, it is possible to incorporate the customer's [billing](checkout-api.md#billing_address-object-optional) and [shipping](checkout-api.md#shipping_address-object-optional) information into the transaction. Moreover, users of this API can include diverse forms of data and information within the body request.

#### Creating the Invoice:

#### 1. [Retrieving pg\_codes (optional)](invoice-api.md#id-1.-retrieving-pg_codes-optional)

Prior to initiating the initial payment, you can choose to invoke the [Payment Methods API](payment-methods.md) to obtain the required [pg\_codes](checkout-api.md#pg_codes-array-required).

#### 2. [Initiating the Payment via Invoice API](invoice-api.md#id-2.-initiating-the-payment-via-invoice-api)

Using a payload as minimal as the one below, you can obtain an invoice.

#### Request

```json
{
   "type":"e_commerce",
   "due_date":"2025-12-29",
   "currency_code":"KWD",
   "pg_codes":[
      "credit-card"
   ],
   "invoice_number":"A00001",
   "invoice_items":[
      {
         "sku":"ABC111",
         "description":"Test",
         "quantity":1.111,
         "unit_price":5.234
      }
   ]
}
```

#### Response

```json
{
   "amount":5.815,
   "checkout_url":"https://sandbox.ottu.net/b/checkout/redirect/start/?session_id=9ba4c834f55f1e5476b9d9ea47ec7b12f61f9511",
   "currency_code":"KWD",
   "due_datetime":"29/12/2025 00:00:00",
   "invoice_id":3,
   "language":"en",
   "operation":"purchase",
   "payment_methods":[
      {
         "code":"credit-card",
         "name":"Credit Card",
         "pg":"Ottu PG",
         "type":"sandbox",
         "amount":"5.815",
         "currency_code":"KWD",
         "fee":"0.000",
         "fee_description":"",
         "icon":"https://sandbox.ottu.net/media/gateway/settings/logos/MASTER_q6sxwtA_md4lBKv.jpeg",
         "flow":"redirect",
         "redirect_url":"https://pg.ottu.dev/checkout/c2FuZGJveC5vdHR1Lm5ldA==/Z0F"
      }
   ],
   "payment_type":"one_off",
   "pg_codes":[
      "credit-card"
   ],
   "session_id":"9ba4c834f55f1e5476b9d9ea47ec7b12f61f9511",
   "state":"created",
   "type":"e_commerce",
   "invoice_number":"A00001",
   "due_date":"2025-12-29",
   "invoice_items":[
      {
         "sku":"ABC111",
         "description":"Test",
         "quantity":1.111,
         "unit_price":5.234,
         "total_excl_tax":5.815,
         "tax_amount":0.0,
         "total_incl_tax":5.815
      }
   ],
   "shortify_checkout_url":false,
   "expiration_time":null,
   "shortify_attachment_url":false,
   "generate_qr_code":false,
   "include_sdk_setup_preload":false,
   "data":{
      "fields":[
         
      ],
      "api_version":"invoice_v1"
   },
   "subtotal":5.815,
   "total_excl_tax":5.815,
   "tax_amount":0.0,
   "shipping_incl_tax":0.0,
   "total_incl_tax":5.815,
   "invoice_pdf_url":"https://e.pay.kn/HWiDBBAQ8WBn"
}
```

The URL for the invoice PDF in the above [response](invoice-api.md#response) is:

```json
"invoice_pdf_url":"https://e.pay.kn/HWiDBBAQ8WBn"
```

<figure><img src="../.gitbook/assets/Invoice API 2.png" alt=""><figcaption></figcaption></figure>

Applying a discount percentage of 12 to the `invoice_items` object within the `Invoice API` request payload:

```json
{
   "type":"e_commerce",
   "due_date":"2025-12-29",
   "currency_code":"KWD",
   "pg_codes":[
      "credit-card"
   ],
   "invoice_number":"A00002",
   "invoice_items":[
      {
         "sku":"ABC111",
         "description":"Test",
         "quantity":1.111,
         "unit_price":5.234,
         "discount_percentage":12
      }
   ]
}
```

&#x20;  **Invoice:**

<figure><img src="../.gitbook/assets/Invoice API 3.png" alt=""><figcaption></figcaption></figure>

## [Best Practices](invoice-api.md#best-practices)

**Understanding Ottu's Invoice API:** Ottu's `Invoice API` automates invoice generation, saving time and reducing errors for both online and walk-in customers. Please check [Invoice Generation Logic](invoice-api.md#invoice-generation-logic).

**Prerequisites for Integration:** Ensure a **Purchase** [Payment Gateway](../user-guide/payment-gateway.md) and understand Ottu's[ rounding function](invoice-api.md#how-round-funtion-works). Optionally explore the [Payment Methods API](payment-methods.md) for dynamic updates.

**Authentication Best Practices:** Use [Private API Key](authentication.md#private-key-api-key) or [Basic Authentication](authentication.md#basic-authentication). Regularly rotate `API keys` for security.

**Updating Invoice:** Invoice is immutable; create a new one for updates.

**VAL CALC Fields:** [VAL CALC](invoice-api.md#what-do-the-calculated-values-encompass) fields ensure accurate figures, like Tax Amount and Shipping Including Tax.

**Documentation References:** Refer to Ottu's OpenAPI[ schema](invoice-api.md#b-invoice-v1-invoice) for technical details.

**Technical Implementation Guide:** Follow Ottu's [step-by-step guide](invoice-api.md#guide-step-by-step) for a seamless integration.

## [Invoice Generation Logic](invoice-api.md#invoice-generation-logic)

The Invoice Generation Logic is a critical component of Ottu's invoicing system, designed to ensure precision, accuracy, and consistency in financial transactions. At the core of this logic is the utilization of the `round` function. For more information about round function, please check [here](invoice-api.md#how-round-funtion-works).&#x20;

### [**Key Steps in Invoice Generation**](invoice-api.md#key-steps-in-invoice-generation)

The invoice generation process involves two levels: the invoice items level and the invoice level. The logic begins at the invoice items level and progresses through a series of calculations, incorporating discounts, taxes, and total values. The results are then aggregated at the invoice level to derive a comprehensive view of the entire transaction

#### **Invoice Items Level:**

1. Calculate `quantity_price`: round(`quantity` \* `unit_price`)
2. Calculate the discount on the item:
   * If both `discount_percentage` and `discount_amount` are present, throw an error message.
   * If only `discount_percentage` is present, calculate the total discount for this item: `total_discount` = (`quantity_price` \* `discount_percentage`/100).
   * If only `discount_amount` is present, ensure it is not more than `quantity_price` and store it in `total_discount`.
   * `total_discount` = round(`total_discount`) for proper number handling.
3. Calculate total values:
   * `total_excl_tax` = round(`quantity_price` - `total_discount`)
   * `total_excl_tax`: `VAL CALC`: If provided in the payload, check for discrepancies.
   * `tax_amount` = round(`total_excl_tax` \* `tax_rate` / 100)
   * `tax_amount`: `VAL CALC`
   * `total_incl_tax` = round(`total_excl_tax` + `tax_amount`)
   * `total_incl_tax`: `VAL CALC`
4. Store results for use on the Invoice level.

#### **Invoice Level:**

1. Calculate subtotal by:
   * `subtotal = 0`
   * Loop over items: `subtotal` = round(`subtotal` + `item.total_incl_tax`)
   * `subtotal`: `VAL CALC`
2. Calculate the discount on the invoice:
   * If both `discount_percentage` and `discount_amount` are present, throw an error message.
   * If only `discount_percentage` is present, calculate the total discount for the invoice: `total_discount` = (`subtotal` \* `discount_percentage`/100).
   * If only `discount_amount` is present, ensure it is not more than the `subtotal` and store it in `total_discount`.
   * `total_discount` = round(`total_discount`) for proper number handling.
3. Calculate total values:
   * `total_excl_tax` = round(`subtotal` - `total_discount`)
   * `total_excl_tax`: `VAL CALC`: If provided in the payload, check for discrepancies.
   * `tax_amount` = round(`total_excl_tax` \* `tax_rate` / 100)
   * `tax_amount`: `VAL CALC`
   * `shipping_tax` = round(`shipping_excl_tax` \* `shipping_tax_rate` / 100)
   * `shipping_incl_tax` = round(`shipping_excl_tax` + `shipping_tax`)
   * `shipping_incl_tax`: `VAL CALC`
   * `total_incl_tax` = round(`total_excl_tax` + `tax_amount` + `shipping_incl_tax`)
   * `total_incl_tax`: `VAL CALC`
   * `amount` = `total_incl_tax`
   * `amount`: `VAL CALC`: The `amount` field is not mandatory, but if included, it serves as a verification measure. It acts as a final check to ensure that the total amount calculated on the frontend aligns with the amount the endpoint will use for the invoice creation. Any mismatch halts the process to prevent inconsistencies.

## [FAQ](invoice-api.md#faq)

#### :digit\_one: [**What is Ottu's Invoice API?**](invoice-api.md#what-is-ottus-invoice-api)

Ottu's `Invoice API` is a tool provided by Ottu for businesses and developers to integrate invoice functionality into their systems. It automates the process of invoice generation, making it efficient for both online and walk-in customers.&#x20;

#### :digit\_two: [**Why should I use Ottu's Invoice API?**](invoice-api.md#why-should-i-use-ottus-invoice-api)

Ottu's `Invoice API` automates the invoicing process, saving time and reducing errors associated with manual creation. It provides a seamless experience for customers and merchants, ensuring timely communication and streamlined payment collection.

#### :digit\_three: [**What are the prerequisites for integrating with Ottu's Invoice API?**](invoice-api.md#what-are-the-prerequisites-for-integrating-with-ottus-invoice-api)

Before integration, ensure you have a **Purchase** [Payment Gateway](../user-guide/payment-gateway.md), understand the [round](invoice-api.md#how-round-funtion-works) function, and optionally explore the [Payment Methods API](payment-methods.md). For more information, please refer to [Setup](invoice-api.md#setup).

#### :digit\_four: [**Can I skip using the Payment Methods API?**](invoice-api.md#can-i-skip-using-the-payment-methods-api)

Yes, if you are familiar with a specific `pg_code`, you can skip using the[ Payment Methods API](payment-methods.md). However, using it ensures you stay informed about recent payment gateways and configurations, enhancing adaptability.

#### :digit\_five: [**How do I handle authentication and permissions for Ottu's Invoice API?**](invoice-api.md#how-do-i-handle-authentication-and-permissions-for-ottus-invoice-api)

[Authentication](invoice-api.md#authentication) can be done using [Private API Key](authentication.md#private-key-api-key) or [Basic Authentication](authentication.md#basic-authentication). For more information please refer to [Permissions](invoice-api.md#permissions) section.

#### :digit\_six: [**What are VAL CALC fields in the Invoice API?**](invoice-api.md#what-are-val-calc-fields-in-the-invoice-api)

They are optional and fields associated with the calculation of prices in an invoice. They play a crucial role in displaying accurate figures during the invoice creation process, such as `Tax Amount` and `Shipping Including Tax`. For more information, please check [here](invoice-api.md#how-round-funtion-works).

#### :digit\_seven: [**Can I modify an invoice after creation?**](invoice-api.md#can-i-modify-an-invoice-after-creation)

No, Ottu's `Invoice API` does not support the PATCH method. Once an invoice is created, it is immutable and cannot be modified. Any updates require creating a new invoice.

#### :digit\_eight: [**How does Ottu calculate totals and taxes in the invoice?**](invoice-api.md#how-does-ottu-calculate-totals-and-taxes-in-the-invoice)

Ottu uses a specific logic for calculating totals and taxes at both the item and invoice levels. Details are provided in the [Invoice Generation Logic](invoice-api.md#invoice-generation-logic) section of the documentation.

#### :digit\_nine: [**Where can I find more technical details and implementation specifics?**](invoice-api.md#where-can-i-find-more-technical-details-and-implementation-specifics)

For detailed technical understanding and implementation specifics, refer to Ottu's OpenAPI schema in the API Schema Reference section of the documentation.

**To conclude,** Ottu's `Invoice API` presents a powerful tool for businesses and developers, offering a seamless and automated solution for invoice generation. With a focus on user-friendly integration, the API streamlines the invoicing process, providing flexibility and customization for merchants. The comprehensive features, including detailed invoice information and generation logic, ensure accuracy and efficiency. Ottu's `Invoice API` stands as a valuable asset for those seeking to enhance their invoicing workflows, providing a reliable and advanced solution for a diverse range of businesses.
