# User Cards

## [**Authentication**](user-cards.md#authentication)

[API Private Key](authentication.md#private-key)

## [**Fetch cards**](user-cards.md#fetch-cards)

#### [**Base URL**](user-cards.md#base-url)

<mark style="color:blue;">**GET:**</mark>

**For list of tokens**

```http
https://<ottu-url>/b/pbl/v2/card 
```

**For detail of token**&#x20;

```http
https://<ottu-url>/b/pbl/v2/card/{{token}}
```

#### [**Request parameters**](user-cards.md#request-parameters)

#### [type](checkout-api.md#type-string-required) <mark style="color:red;">`required`</mark>

<mark style="color:blue;">**Sandbox:**</mark> An intermediate environment built for testing purposes, will be excluded from the live reports.\
Or\
<mark style="color:blue;">**Production:**</mark> live environment where the real action(s) going on.

#### [customer\_id](checkout-api.md#customer\_id-string-optional)[ ](checkout-api.md#customer\_id-string-optional)<mark style="color:red;">`required`</mark>

#### [pg\_codes](checkout-api.md#pg\_codes-list-required) <mark style="color:blue;">`optional`</mark>

#### [**Response body showcase**](user-cards.md#response-body-showcase)

In case there is a card for the requested  [customer\_id](user-cards.md#customer\_id)

```json

 {
        "customer_id": "test",
        "brand": "VISA",
        "name_on_card": "test",
        "number": "**** 0026",
        "expiry_month": "expiry_month",
        "expiry_year": "expiry_year",
        "token": "token",
        "pg_code": "pg_code",
        "is_preferred": true,
        "is_expired": true, 
        "will_expire_soon": false, 
        "can_be_charged_offline": false,
        "cvv_required": true
    },
```

{% hint style="info" %}
```json
"is_expired"  true or false 
If the expiration date has passed the current date
"will_expire_soon" true or false 
If the expiration is in the current or future month
"can_be_charged_offline" true or false 
It means PG MID is enabled for offline recharge and CVV and OTP checks are
disabled for subsequent payments
```
{% endhint %}

## [**Delete cards**](user-cards.md#delete-cards)

#### [**Base URL**](user-cards.md#base-url-1)

<mark style="color:red;">**DEL:**</mark>

```http
https://<ottu-url>/b/pbl/v2/card/{{token}}
```

#### [**Request parameters**](user-cards.md#request-parameters-1)

#### [type](checkout-api.md#type-string-required) <mark style="color:red;">`required`</mark>

#### [customer\_id](user-cards.md#customer\_id-required-1) <mark style="color:red;">`required`</mark>
