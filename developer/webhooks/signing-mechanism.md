# Signing Mechanism

To ensure the integrity and authenticity of the [webhook notifications](payment-webhooks.md) sent to the merchant, Ottu employs a signing mechanism based on HMAC (Hash-based Message Authentication Code). By leveraging HMAC, Ottu can guarantee that the webhook's content remains untampered during transmission.

## [Important Components](signing-mechanism.md#important-components)

#### [**1. HMAC Key (Secret Key)**](signing-mechanism.md#1.-hmac-key-secret-key)

* This is the backbone of the signing and verification process. Merchants can retrieve their unique HMAC Key from the Webhook Configuration panel within Ottu's admin dashboard [here](../../user-guide/configuration.md#hmac-key).
* It's paramount that this key remains confidential. Always store it securely and avoid exposing it to the public.

#### [**2. Fields for Signature** ](signing-mechanism.md#2.-fields-for-signature)

The signature is not derived from every field in the webhook payload. See payload example [here](payment-webhooks.md#payload-example-attempted-1). Only specific fields are considered. These are:

* amount
* currency\_code
* customer\_first\_name
* customer\_last\_name
* customer\_email
* customer\_phone
* customer\_address\_line1
* customer\_address\_line2
* customer\_address\_city
* customer\_address\_state
* customer\_address\_country
* customer\_address\_postal\_code
* gateway\_name
* gateway\_account
* order\_no
* reference\_number
* result
* state

**Key Considerations**:

1. Fields not present in the webhook payload or those with an empty string value are not considered when constructing the signature.
2. Only fields present in the above list and in the payload with valid non-empty values are considered for signature generation.

This update ensures that developers understand the significance of field presence and their values in the payload when constructing the HMAC signature.

#### [**3. Signature Creation**](signing-mechanism.md#3.-signature-creation)

* Fields from the payload are extracted based on the aforementioned list, sorted alphabetically by key name, and then concatenated to form a unique message string.
* This string, combined with the HMAC Key, is used to create the **HMAC-SHA256** signature. This resultant signature is then dispatched with the [webhook notification](payment-webhooks.md).

#### [**4. Verification by Merchant**](signing-mechanism.md#4.-verification-by-merchant)

* On receipt of the webhook, merchants should rebuild the message string, using the listed fields.
* Generate an HMAC signature using their stored [HMAC Key](../../user-guide/configuration.md#hmac-key).
* If the computed signature corresponds to the provided one, the payload's authenticity is confirmed. Any discrepancies suggest potential tampering.

## [Example](signing-mechanism.md#example)

For illustration purposes, let's consider a sample webhook payload and a hypothetical HMAC key.

**Webhook Payload**:

```json
{
   "amount":"86.000",
   "currency_code":"KWD",
   "customer_first_name":"example-customer"
}
```

**HMAC Key**: `pu9MpX3yPR`

Given this payload and key, the steps to construct the HMAC signature are:

1. Sort the payload keys.
2. Use the list of specific fields (as defined in the [Fields for Signature](signing-mechanism.md#2.-fields-for-signature) section) to extract values from the payload.
3. Concatenate the key-value pairs.
4. Apply the HMAC algorithm using the **SHA256** hash function and the provided HMAC key.

Following these steps, the resulting signature is:

`6143b8ad4bd283540721ab000f6de746e722231aaaa90bc38f639081d3ff9f67`

Developers should compare this generated signature to the signature received in the webhook payload to validate its authenticity.

## [Signature Generation](signing-mechanism.md#signature-generation)

Ensuring the integrity and authenticity of webhook payloads is paramount for the security of both the service provider and the merchants. To achieve this, an HMAC (Hash-Based Message Authentication Code) signature is generated and sent along with the payload. This signature needs to be validated at the merchant's end to confirm that the data has not been tampered with. For the convenience of developers working with different programming languages, we provide ready-to-use code snippets in various popular languages to generate and verify this HMAC signature. This section showcases how to compute the HMAC signature for the payload in languages like [Python](signing-mechanism.md#python), [PHP](signing-mechanism.md#php), [Java](signing-mechanism.md#java), [.NET (C#)](signing-mechanism.md#.net-c), [Node.js,](signing-mechanism.md#node.js) [Ruby](signing-mechanism.md#ruby), and [Go](signing-mechanism.md#go).

## [Specific Examples](signing-mechanism.md#specific-examples)

### Python

Python function example for generating the HMAC signature given a payload and an HMAC key:

```python
import hmac
import hashlib

def generate_hmac_signature(payload, hmac_key):
    # List of fields that are considered for the HMAC signature
    keys = [
        "amount",
        "currency_code",
        "customer_first_name",
        "customer_last_name",
        "customer_email",
        "customer_phone",
        "customer_address_line1",
        "customer_address_line2",
        "customer_address_city",
        "customer_address_state",
        "customer_address_country",
        "customer_address_postal_code",
        "gateway_name",
        "gateway_account",
        "order_no",
        "reference_number",
        "result",
        "state",
    ]

    # Extract and sort the payload keys based on the 'keys' list, and ignore any missing or empty string values
    message = [(k, payload[k]) for k in sorted(payload) if k in keys and payload[k]]

    # Concatenate the key-value pairs
    message_str = "".join([f"{k}{v}" for (k, v) in message])

    # Compute the HMAC signature
    digest = hmac.new(
        bytes(hmac_key, encoding="utf8"),
        bytes(message_str, encoding="utf8"),
        digestmod=hashlib.sha256
    ).hexdigest()

    return digest

# Test
payload = {
   "amount":"86.000",
   "currency_code":"KWD",
   "customer_first_name":"example-customer"
}
hmac_key = "pu9MpX3yPR"

print(generate_hmac_signature(payload, hmac_key))
```

When you run this code, the printed result should match the provided HMAC signature: `6143b8ad4bd283540721ab000f6de746e722231aaaa90bc38f639081d3ff9f67`.

### PHP

```php
<?php

function generateHmacSignature($payload, $hmacKey) {
    $keys = [
        "amount", "currency_code", "customer_first_name",
        "customer_last_name", "customer_email", "customer_phone",
        // ... [add all the other keys here] ...
        "reference_number", "result", "state"
    ];
    
    $message = "";
    foreach ($keys as $key) {
        if (isset($payload[$key]) && $payload[$key] !== "") {
            $message .= $key . $payload[$key];
        }
    }
    
    return hash_hmac('sha256', $message, $hmacKey);
}

// Test
$payload = [
    "amount" => "86.000",
    "currency_code" => "KWD",
    "customer_first_name" => "example-customer"
];
$hmacKey = "pu9MpX3yPR";

echo generateHmacSignature($payload, $hmacKey);
?>
```

### Java

```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.util.HashMap;
import java.util.Map;

public class SignatureGenerator {

    public static String generateHmacSignature(Map<String, String> payload, String hmacKey) throws Exception {
        String[] keys = {
            "amount", "currency_code", "customer_first_name",
            // ... [add all the other keys here] ...
            "reference_number", "result", "state"
        };

        StringBuilder message = new StringBuilder();
        for (String key : keys) {
            if (payload.containsKey(key) && !payload.get(key).isEmpty()) {
                message.append(key).append(payload.get(key));
            }
        }

        Mac sha256HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secretKey = new SecretKeySpec(hmacKey.getBytes(StandardCharsets.UTF_8), "HmacSHA256");
        sha256HMAC.init(secretKey);

        byte[] hashBytes = sha256HMAC.doFinal(message.toString().getBytes(StandardCharsets.UTF_8));
        StringBuilder sb = new StringBuilder();
        for (byte b : hashBytes) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }

    public static void main(String[] args) throws Exception {
        Map<String, String> payload = new HashMap<>();
        payload.put("amount", "86.000");
        payload.put("currency_code", "KWD");
        payload.put("customer_first_name", "example-customer");

        String hmacKey = "pu9MpX3yPR";
        System.out.println(generateHmacSignature(payload, hmacKey));
    }
}
```

### Node.js

```javascript
const crypto = require('crypto');

function generateHmacSignature(payload, hmacKey) {
    const keys = [
        'amount', 'currency_code', 'customer_first_name',
        // ... [add all the other keys here] ...
        'reference_number', 'result', 'state'
    ];
    
    let message = '';
    keys.forEach(key => {
        if (payload[key] && payload[key] !== '') {
            message += key + payload[key];
        }
    });
    
    const hmac = crypto.createHmac('sha256', hmacKey);
    hmac.update(message);
    
    return hmac.digest('hex');
}

// Test
const payload = {
    amount: "86.000",
    currency_code: "KWD",
    customer_first_name: "example-customer"
};
const hmacKey = "pu9MpX3yPR";

console.log(generateHmacSignature(payload, hmacKey));
```

### .NET (C#)

```csharp
using System;
using System.Collections.Generic;
using System.Security.Cryptography;
using System.Text;

public class SignatureGenerator {
    public static string GenerateHmacSignature(Dictionary<string, string> payload, string hmacKey) {
        string[] keys = {
            "amount", "currency_code", "customer_first_name",
            // ... [add all the other keys here] ...
            "reference_number", "result", "state"
        };

        StringBuilder message = new StringBuilder();
        foreach (string key in keys) {
            if (payload.ContainsKey(key) && !String.IsNullOrEmpty(payload[key])) {
                message.Append(key).Append(payload[key]);
            }
        }

        using (var hmac = new HMACSHA256(Encoding.UTF8.GetBytes(hmacKey))) {
            byte[] hashBytes = hmac.ComputeHash(Encoding.UTF8.GetBytes(message.ToString()));
            return BitConverter.ToString(hashBytes).Replace("-", "").ToLower();
        }
    }

    static void Main(string[] args) {
        var payload = new Dictionary<string, string> {
            {"amount", "86.000"},
            {"currency_code", "KWD"},
            {"customer_first_name", "example-customer"}
        };
        string hmacKey = "pu9MpX3yPR";

        Console.WriteLine(GenerateHmacSignature(payload, hmacKey));
    }
}
```

### Ruby

```ruby
require 'openssl'

def generate_hmac_signature(payload, hmac_key)
    keys = [
        'amount', 'currency_code', 'customer_first_name',
        # ... [add all the other keys here] ...
        'reference_number', 'result', 'state'
    ]

    message = ""
    keys.each do |key|
        if payload[key] && payload[key] != ''
            message += key + payload[key]
        end
    end

    digest = OpenSSL::HMAC.hexdigest('sha256', hmac_key, message)
    return digest
end

# Test
payload = {
    "amount" => "86.000",
    "currency_code" => "KWD",
    "customer_first_name" => "example-customer"
}


hmac_key = "pu9MpX3yPR"

puts generate_hmac_signature(payload, hmac_key)
```

### Go

```go
package main

import (
	"crypto/hmac"
	"crypto/sha256"
	"encoding/hex"
	"fmt"
)

func generateHmacSignature(payload map[string]string, hmacKey string) string {
	keys := []string{
		"amount", "currency_code", "customer_first_name",
		// ... [add all the other keys here] ...
		"reference_number", "result", "state",
	}

	message := ""
	for _, key := range keys {
		if value, ok := payload[key]; ok && value != "" {
			message += key + value
		}
	}

	h := hmac.New(sha256.New, []byte(hmacKey))
	h.Write([]byte(message))

	return hex.EncodeToString(h.Sum(nil))
}

func main() {
	payload := map[string]string{
		"amount":              "86.000",
		"currency_code":       "KWD",
		"customer_first_name": "example-customer",
	}
	hmacKey := "pu9MpX3yPR"

	fmt.Println(generateHmacSignature(payload, hmacKey))
}
```

The above examples provide a way for developers in different languages to generate the HMAC signature from a payload using the provided HMAC key.

**Need Further Assistance?** Our dedicated support team is always on hand to help. Reach out to us at [support@ottu.com](mailto:support@ottu.com).

**Your Feedback Matters**: We continually strive to improve, and your feedback is invaluable to us. Please let us know if you found this guide helpful or if there are areas you feel could benefit from more detail.
