# User Saved Cards

## [Getting Started](user-saved-cards.md#getting-started)

Ottu provides two API endpoints for managing user saved cards, making it easy to handle your customers' saved payment information securely. These endpoints allow you to [retrieve](user-saved-cards.md#fetch-cards) all saved cards for a customer or [delete](user-saved-cards.md#delete-cards) a specific card. This functionality is beneficial for providing your customers with a seamless and personalized payment experience, enabling them to manage their saved cards within your application.

**When using the User Saved Cards endpoints:**

* You will not receive the user's full card number (PAN); instead, you'll only get the last 4 digits and a token that can be used for performing payments or authorizations.
* Ottu [Checkout SDK](../checkout-sdk/) already handles the first two points mentioned below, displaying saved cards and allowing customers to delete their own cards. However, you can implement a lower-level orchestration by yourself using the provided tokens.

**By integrating the User Saved Cards endpoints into your application, you can:**

1. Display a list of saved cards to the customer, allowing them to choose their preferred payment method for a faster checkout experience.
2. Allow customers to delete saved cards that are no longer valid or needed, giving them more control over their payment information.
3. Implement additional features based on your customers' saved cards, such as automatic payments or subscription management

With these endpoints, you can enhance the user experience, foster customer trust, and boost conversion rates in your application.

## [**Authentication**](user-saved-cards.md#authentication)

[Fetch ](user-saved-cards.md#fetch-cards)and [delete ](user-saved-cards.md#delete-cards)cards APIs use API private key authentication, where a unique secret code is generated and assigned to authorized users or applications. This key is included in request headers and verified by our system to control access to API resources. This method provides a secure and scalable way to authenticate API requests and ensure only authorized access to our services. For more details, check [API Private Key](authentication.md#private-key)

#### [Fetch Cards](user-saved-cards.md#fetch-cards)

#### [Delete Cards](user-saved-cards.md#delete-cards)
