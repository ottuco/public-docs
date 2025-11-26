# Native Payment API

Use the Native Payment API when you want full control of the client experience (web or mobile) and prefer not to use the [Checkout SDK](checkout-sdk/). Your client or backend collects a payment payload and sends it to Ottu to process the payment for a given [session\_id](checkout-api.md#session_id-string-mandatory).

A payment payload can be:

* Wallet payment data (e.g., Apple Pay paymentData, Google Pay paymentMethodData) — typically encrypted by the wallet.
* Gateway token / network token (card-on-file or one-click use cases) — not necessarily encrypted.

Ottu processes the payload with the configured gateway and returns a normalized callback result.

#### [**When to Use**](native-payment-api.md#when-to-use)

* Apple Pay or Google Pay buttons are rendered and managed by you.
* Existing tokenization has already been implemented and needs to be used to charge with a gateway token.
* Granular, SDK-less control of the UX is required, while Ottu’s orchestration and gateway integrations are still leveraged.

## [Quick Start](native-payment-api.md#quick-start)

Send a post request to the payment service endpoint with payment [session\_id](checkout-api.md#session_id-string-mandatory), then the payment service collected  `token` to perform the payment process.

#### Quick Apple Pay Example (cURL)

```json
curl -X POST "https://sandbox.ottu.net/b/pbl/v2/payment/apple-pay/" \
  -H "Authorization: Api-Key GYj5Na8H.29g9hqNjm11nORQMa2WiZwIBQQ49MdAL" \
  -H "Content-Type: application/json" \
  -d '{
    "payload": {
      "pg_code": "apple-pay", 
      "session_id": "str",
      "payload": {
        "paymentData": {
          ...
        }
      } // the apple payment token without modifications
    }
  }'
```

Ottu securely processes the Apple Pay request and returns a unified success response once the payment is completed.

```json
{
  "result": "success",
  "message": "successful payment",
  "pg_response": {}
}
```

Use the response values to reconcile the payment in your backend and update your order state.

Continue with the sections below to learn more about response fields, error handling, and webhook integration.

## [Authentication](native-payment-api.md#authentication)

**Supported Methods**

* [Private API Key](authentication.md#private-key-api-key) (Server-Only)
* [Public API Key](authentication.md#public-key) (Client-Safe)
* [Basic Authentication](authentication.md#basic-authentication) (Server-Only)

For detailed information on authentication procedures, please refer to the [Authentication ](native-payment-api.md#authentication)section.

{% hint style="danger" %}
&#x20;The Private API Key must **never** be exposed in any client-side application.
{% endhint %}

## [Integration Flows](native-payment-api.md#integration-flows)

1. **Client → Ottu (**[Public API Key](authentication.md#public-key) **)**
   1. The client collects the wallet or tokenized payment payload and calls the Native Payment API directly using the [Public API Key](authentication.md#public-key).
   2. The client receives the API callback response.

If the call is made from the client side, the backend must be synchronized with the payment result by ensuring that one of the following actions is performed:

* The API response is forwarded to the backend, **or**
* The [Payment Status (Inquiry) API](payment-status-inquiry.md) is called by the backend after the client confirms that the payment has been completed.

<figure><img src="../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Client → Backend → Ottu (**[Private API Key](authentication.md#private-key-api-key)**– Recommended)**
   1. The client sends the payment payload to Backend
   2. Backend calls Ottu native payment API
   3. Backend receive payment response callback
   4. Backend process callback response and notify client side with payment status

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

## [Setup](native-payment-api.md#setup)

* A valid [session\_id](checkout-api.md#session_id-string-mandatory) obtained from the [Checkout API](checkout-api.md).
* A Merchant Gateway ID (MID) with the payment service activated and properly configured in Ottu.&#x20;

To complete the setup, the **Service Setup** section for the specific payment service being implemented must be followed, such as:

* [Apple Pay](native-payment-api.md#apple-pay)&#x20;
* [Google Pay](native-payment-api.md#google-pay)
* [Auto-Debit (Tokenized Payments)](native-payment-api.md#auto-debit-tokenized-payments)

If multiple gateways are configured, always include the [pg\_code](checkout-api.md#pg_codes-array-required) corresponding to the MID that has the target payment service enabled.\
**Example:**\
If a transaction has `knet` and `mpgs` `pg_code` but only `knet` supports Apple Pay, you must send \
`pg_code`: `knet` when calling the Apple Pay API.

#### [Checklist](native-payment-api.md#checklist)

* [x] &#x20;Created a valid [session\_id](checkout-api.md#session_id-string-mandatory).
* [x] Completed Apple Pay / Google Pay setup (if applicable).
* [x] Selected the correct invocation model (client or backend).
* [x] Used the appropriate API key type ([Public API Key](authentication.md#public-key) vs. [Private API Key](authentication.md#private-key-api-key)).
* [x] Sent wallet payment payload or gateway token (no raw card data).
* [x] Implemented backend sync logic.

## [Apple Pay](native-payment-api.md#apple-pay)

{% hint style="info" %}
The prerequisites and [checklist](native-payment-api.md#checklist) mentioned in the general [Setup](native-payment-api.md#setup) section should be applied. They can be accessed [here](native-payment-api.md#setup).
{% endhint %}

#### [Setup](native-payment-api.md#setup-1)

1. Apple Pay is configured on the client side (iOS / web).
2. The encrypted `paymentData` object is collected from Apple Pay.
3. The payload, along with the [session\_id](checkout-api.md#session_id-string-mandatory), is sent to the Ottu Payment API.
4. Ottu processes via the configured Apple Pay gateway and returns a unified result (`succeeded`, `failed`).

{% hint style="danger" %}
* Do not modify the Apple Pay payload. Any change invalidates token decryption.
* Include [pg\_code](/broken/pages/OCLTqphKqkbEMATf9pam#pg_codes) if multiple gateways are configured.
{% endhint %}

#### [API Schema Reference](native-payment-api.md#api-schema-reference)

{% openapi-operation spec="native" path="/b/pbl/v2/payment/apple-pay/" method="post" %}
[OpenAPI native](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/2a046c326cc6936c62d7bb7e3bfc294aa5597e7a46e44f4375c4ce8f3b3eaca5.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20251126%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20251126T074648Z&X-Amz-Expires=172800&X-Amz-Signature=80e91699251b7008164bd42e9ad572c5043a0489b8602c75ac2b6880bcc339f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [Google Pay](native-payment-api.md#google-pay)

{% hint style="info" %}
The prerequisites and [checklist](native-payment-api.md#checklist) mentioned in the general [Setup](native-payment-api.md#setup) section should be applied. They can be accessed [here](native-payment-api.md#setup).
{% endhint %}

#### [Setup](native-payment-api.md#setup-2)

1. Google Pay is configured on the client side (Android / web).
2. The wallet payment payload (`paymentMethodData`, `email`, `addresses`, etc.) is collected.
3. The payload, along with the [session\_id](checkout-api.md#session_id-string-mandatory) is sent to the Ottu Native Payment API.
4. Ottu processes through the configured gateway and returns a normalized response.

{% hint style="warning" %}
* Do not modify the Google Pay payload. Any change invalidates token decryption.
* Include [pg\_code](/broken/pages/OCLTqphKqkbEMATf9pam#pg_codes) if multiple gateways are configured.
* If the response contains type: "iframe", render it for 3D Secure authentication.Important:
{% endhint %}

#### [API Schema Reference](native-payment-api.md#api-schema-reference-1)

{% openapi-operation spec="native" path="/b/pbl/v2/payment/google-pay/" method="post" %}
[OpenAPI native](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/2a046c326cc6936c62d7bb7e3bfc294aa5597e7a46e44f4375c4ce8f3b3eaca5.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20251126%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20251126T074648Z&X-Amz-Expires=172800&X-Amz-Signature=80e91699251b7008164bd42e9ad572c5043a0489b8602c75ac2b6880bcc339f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [Auto-Debit (Tokenized Payments)](native-payment-api.md#auto-debit-tokenized-payments)

{% hint style="info" %}
The prerequisites and [checklist](native-payment-api.md#checklist) mentioned in the general [Setup](native-payment-api.md#setup) section should be applied. They can be accessed [here](native-payment-api.md#setup).
{% endhint %}

#### [Setup](native-payment-api.md#setup-3)

1. Ensure the token is active and usable for the merchant.
2. Use an existing [session\_id](checkout-api.md#session_id-string-mandatory) created via the [Checkout API](checkout-api.md).
3. Send the token in the `token` field to Ottu.
4. Ottu processes the payment with the configured gateway and returns the callback result.

Supports CIT ([Cardholder Initiated](auto-debit.md#id-1.-customer-initiated-card-change)) and MIT ([Merchant Initiated](auto-debit.md#id-2.-merchant-requested-card-change)) transactions.

#### [API Schema Reference](native-payment-api.md#api-schema-reference-2)

{% openapi-operation spec="native" path="/b/pbl/v2/auto-debit/" method="post" %}
[OpenAPI native](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/2a046c326cc6936c62d7bb7e3bfc294aa5597e7a46e44f4375c4ce8f3b3eaca5.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20251126%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20251126T074648Z&X-Amz-Expires=172800&X-Amz-Signature=80e91699251b7008164bd42e9ad572c5043a0489b8602c75ac2b6880bcc339f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [FAQ](native-payment-api.md#faq)

#### :digit\_one: Can I call the Native Payment API directly from the client?

Yes, but only with the [Public Key,](authentication.md#public-key) and your backend must remain synchronized.

#### :digit\_two: Which model should I use in production?

Always prefer Client → Backend → Ottu using the [Private Key.](authentication.md#private-key-api-key)

#### :digit\_three:How do I verify the payment result?

Use the [Payment Status (Inquiry) API](payment-status-inquiry.md).

#### :digit\_four:What if my transaction has multiple gateway codes?

Include the `pg_code` for the MID that has the corresponding payment service enabled (e.g., Apple Pay, Google Pay).

#### :digit\_five:What happens if I modify wallet data?

The payment will fail — wallet tokens must be sent unmodified.

#### :digit\_six:Can I charge saved tokens automatically?

Yes, use the **Native Payment API** for tokenized or recurring payments.
