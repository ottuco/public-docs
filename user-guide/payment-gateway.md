# Payment Gateway

## [Getting Started](payment-gateway.md#getting-started)

Now, let's explore the world of payment gateways. A payment gateway is an advanced technology utilized by merchants that enable them to effortlessly accept debit and credit card transactions from their customers, whether in physical retail stores (e.g., physical card readers typically found in retail establishments) or online portals (e.g., user-friendly online payment processing portals integrated into e-commerce platforms).

Elevate your business to a new height of convenience and customer satisfaction with our state-of-the-art Online Payment Management System (OPMS). Experience hassle-free transactions and seamless integration, empowering you to expand your customer base and boost your sales. Leave your competitors in the dust as you unleash the full potential of _Ottu's_ exceptional prowess in seamlessly connecting and tuning the payment gateway. Ignite your business's success by harnessing the power of our meticulously optimized online payment management system (OPMS), meticulously crafted to meet the dynamic needs of today's thriving businesses. Discover a new era of convenience, efficiency, and growth with _Ottu_ at your side.

## [Available Operations](payment-gateway.md#available-operations)

With _Ottu_, merchants gain the power to seamlessly carry out essential operations, including capture, refund, and void, across various payment gateways. It's important to note that specific conditions must be met to execute these operations successfully. Furthermore, it's worth mentioning that not all payment gateways support every operation. Choosing _Ottu_ means an exceptional online payment management experience that keeps your business secure, streamlined, and successful. To understand what the Automatic Inquiry feature is and why it's crucial for ensuring transaction reliability, please visit [Payment Status-Inquiry](../developer/rest-api/payment-status-inquiry.md). It offers a detailed explanation on how this feature safeguards your transactions from unforeseen disruptions.

{% tabs %}
{% tab title="Amazon Pay" %}
PG Name: amazon\_pay

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_six:

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Bambora" %}
PG Name: bambora

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :x:

Minutes after auto inquiry is called: N/A

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Benefit" %}
PG Name: benefit

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_eight:

Refund: :heavy\_check\_mark:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Benefit Pay" %}
PG Name: benefit\_pay

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_eight:

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Beyon Money" %}
PG Name: beyon\_money

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_eight:

Refund: :heavy\_check\_mark:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Bookey" %}
PG Name: bookey

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_eight:

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Burgan" %}
PG Name: burgan

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :x:

Minutes after auto inquiry is called: N/A

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="CBK" %}
PG Name: cbk

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_eight:

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Ccavenues" %}
PG Name: ccavenues

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :x:

Minutes after auto inquiry is called: N/A

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Cybersource" %}
PG Name: cybersource

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :x:

Minutes after auto inquiry is called: N/A

Refund: :heavy\_check\_mark:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="FSS" %}
PG Name: fss

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_eight:

Refund: :heavy\_check\_mark:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="Hesabe" %}
PG Name: hesabe

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_one::digit\_zero:

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="HyperPay" %}
PG Name: hyperpay

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_three::digit\_one:

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="KNET" %}
PG Name: Knet

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_eight:

Refund: :heavy\_check\_mark:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="MIGS" %}
PG Name: migs

Purchase: ::heavy\_check\_mark:

Authorize: :x:

Inquiry: :x:

Minutes after auto inquiry is called: N/A

Refund: :x:

Void: :x:

Capture: :x:
{% endtab %}

{% tab title="MPGS" %}
PG Name: MPGS

Purchase: ::heavy\_check\_mark:

Authorize: :heavy\_check\_mark:

Inquiry: :heavy\_check\_mark:

Minutes after auto inquiry is called: :digit\_one::digit\_one:

Refund: :heavy\_check\_mark:

Void: :heavy\_check\_mark:

Capture: :heavy\_check\_mark:
{% endtab %}
{% endtabs %}

## [Operations Definitions & Conditions](payment-gateway.md#operations-definitions-and-conditions)

Operations don't support foreign [currencies](currencies.md). If a customer pays using a different currency than the MID (e.g., MID is KWD but payment is in USD via Ottu's [currency exchange](currencies.md#currency-exchanges)), [operations](../developer/rest-api/operations.md#external-operations) won't work. They only function when the payment currency matches the MID currency.

| **Operation** | **Definition**                                                                                  | **Conditions**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | **Available in**                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| Void          | Canceling or rolling back an authorized payment transaction.                                    | <ol><li>The payment transaction must be <strong>authorized</strong>.</li><li><strong>No capture</strong> operation was performed.</li></ol>                                                                                                                                                                                                                                                                                                                                                | MPGS                                                                                                                   |
| Capture       | Collecting a full or partial authorized amount and crediting it to the merchant's bank account. | <ol><li>The payment transaction must be <strong>authorized</strong>.</li><li>The authorized amount must be <strong>sufficient</strong>.</li></ol>                                                                                                                                                                                                                                                                                                                                          | <p>MPGS</p><p>Tabby</p>                                                                                                |
| Refund        | Returning the full or partial amount paid (or captured) to the customer's bank account.         | <p>There are different requirements for refunding payments based on the <strong>type of payment transaction</strong> (i.e., operation):</p><ul><li><strong>For Auth transactions:</strong></li></ul><ol><li>A capture operation must be done before refunding.</li><li>The captured amount must be enough to cover the refund amount.</li></ol><ul><li><strong>For Purchase transactions:</strong></li></ul><ol><li>The paid amount must be sufficient for the requested refund.</li></ol> | <p>FSS</p><p>MPGS</p><p>MyFatoorah</p><p>NGenius</p><p>PayU India</p><p>QPay</p><p>Tabby<br>Cybersource<br>STC Pay</p> |
