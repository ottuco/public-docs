# User Cards

Before diving into this section, it’s recommended to first review the [Tokenization](tokenization.md) section for an understanding of how Ottu securely manages user card data. Optionally, you may also want to familiarize yourself with the [Checkout SDK](checkout-sdk/) for a smoother integration process. With Ottu, managing your customers’ saved cards is straightforward and secure, thanks to our user-friendly API endpoints. These APIs allow you to [fetch](user-cards.md#fetch-cards) all saved cards for a customer or [delete](user-cards.md#delete-card) a specific card. By incorporating this functionality into your system, you’re ensuring a seamless, personalized, and efficient payment experience for your customers.

{% hint style="info" %}
User Cards API is not available in KSA.
{% endhint %}

## [Setup](user-cards.md#setup)

When integrating the User Saved Cards endpoints into your system, here are the key points you need to be aware of:

1. You will not receive the full card number (PAN) of the user. Instead, you’ll be provided with the last 4 digits of the card number and a token. This token is what you’ll use for executing payments or authorizations
2. If you’re using the Ottu [Checkout SDK](checkout-sdk/), customers can delete their saved cards at any point in time. This flexibility gives users more control over their payment information and helps foster trust in your services.
3. When a customer chooses to save their card for payment, the corresponding token will be included in the payload sent to your designated [webhook\_url](checkout-api.md#webhook_url-string-optional). This allows you to retrieve and handle the saved card token as required for your application.

{% hint style="info" %}
**Successful Payment is a Prerequisite:** A saved card, represented by a unique token, can only be created after the customer completes a successful payment transaction. This process ensures the validity of the card and enables tokenization. For a detailed guide on how to implement the card-saving feature, please refer to the [Implementation Process documentation](tokenization.md#implementation).
{% endhint %}

4. Ottu already facilitates the display of saved cards and allows customers to delete their own cards. However, if you wish for a more granular level of control, you can perform these actions yourself using the provided tokens.

### [Boost Your Integration](user-cards.md#boost-your-integration)

Enhance your integration with User Card APIs by utilizing our Python SDK tailored for user card management operations. This SDK facilitates efficient interaction with APIs that manage customer card details.

**Available Package:**

* **Python SDK**: Provides an object-oriented approach to list all cards associated with a customer and delete a customer's card using a token. This SDK encapsulates the complexities of API calls, focusing on efficiency and simplicity. [Learn more](https://github.com/ottuco/ottu-py?tab=readme-ov-file#cards)
* **Django SDK**: Facilitates the integration of payment methods into Django projects by providing Django-specific tools and utilities that streamline payment processing. [Find out more](https://github.com/ottuco/ottu-py?tab=readme-ov-file#django-integration)

For successful implementation, comprehending the fundamental concepts and structures documented is essential. This understanding guarantees robust and maintainable integration of the SDK into your projects.

## [**Authentication**](user-cards.md#authentication)

To access Ottu’s User Saved Cards API, ensure to use the [API-Key](authentication.md#private-key-api-key) authentication method.

## [API Schema Reference](user-cards.md#api-schema-reference)

For a more detailed technical understanding and the implementation specifics of these operations, please refer to the OpenAPI schema in the API Schema Reference section below

#### Fetch Cards

{% openapi-operation spec="ottu-july" path="/b/pbl/v2/card/" method="post" %}
[OpenAPI ottu-july](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/dd13d3360690246daeb6267e9ca119ec953037560d96776a32fefe02cc0fb3ec.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250730%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250730T124145Z&X-Amz-Expires=172800&X-Amz-Signature=df86b16df7388738bff9330f082ae9cd51f3ebb82b9ec3c210c88bb9ca34e9cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

#### Delete Card

{% openapi-operation spec="ottu-api" path="/b/pbl/v2/card/{token}/" method="delete" %}
[OpenAPI ottu-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/388b1be1533dc34f1bbab253943a4688575f58f4fedd70e71971280736d76101.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250730%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250730T124146Z&X-Amz-Expires=172800&X-Amz-Signature=30fc1efb7f096486d600e1fde05fa221e550a514ace6602d826fe4a95f75416a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [FAQ](user-cards.md#faq)

#### :digit\_one:[Can I offer to the customer the possibility to save a card without making a successful payment?](user-cards.md#can-i-offer-to-the-customer-the-possibility-to-save-a-card-without-making-a-successful-payment)

At present, it is not possible to save a card without the customer making a successful payment. However, we continually work to expand and improve our services, so stay tuned for updates.

#### :digit\_two:[How can I delete a card?](user-cards.md#how-can-i-delete-a-card)

You have two methods for removing a card. The first one involves utilizing the User Cards API to [retrieve ](user-cards.md#fetch-cards)the customer's cards and establishing a user interface on your end to display the list of saved cards. Upon a customer's selection of a card for deletion, you can invoke the [Delete API ](user-cards.md#delete-card)for that particular card. Alternatively, you have the option of employing the Ottu [Checkout SDK](checkout-sdk/), which adeptly manages these procedures on your behalf.

#### :digit\_three:[Can I trigger auto-debit payments?](user-cards.md#can-i-trigger-auto-debit-payments)

Yes, but only if the customer has agreed, and their card has been enabled for auto-debit. For more details, refer to our [Auto-Debit](auto-debit.md) documentation.

#### :digit\_four:[Does Ottu save the cards within the Ottu system?](user-cards.md#does-ottu-save-the-cards-within-the-ottu-system)

No, Ottu doesn’t store the cards internally. We utilize an external vault for storing card details. In most cases, the cards are saved at the [Payment Gateway ](../user-guide/payment-gateway.md)with which the merchant has a contract. This approach further ensures the safety and security of the card details.

## [What’s Next?](user-cards.md#whats-next)

Now that you’re familiar with how to use and manage User Cards within the Ottu system, you might want to expand your knowledge and capabilities by exploring additional features.

1. **Checkout SDK:** To make your integration even easier and more seamless, consider utilizing the Ottu Checkout SDK. This tool simplifies many aspects of the payment process, including UI implementation and handling specific payment methods like Apple Pay, Google Pay, STC Pay, among others. You can find more details in our [Checkout SDK](checkout-sdk/) Documentation.
2. **Auto-Debit:** Interested in recurring payments or charging your customers when they are offline? Our Auto-Debit feature enables you to do just that. You can learn more about setting up and using Auto-Debit in our [Auto-Debit](auto-debit.md) Documentation.

Remember, our support team is here to help if you have any questions or need further assistance. Keep exploring, and let’s make your payment process as efficient and user-friendly as possible!
