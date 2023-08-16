# Apple Pay

## [Introduction](apple-pay.md#introduction)

Apple Pay is a comprehensive payment service that encompasses two primary categories: mobile payment and digital wallets. These services provide convenient and secure methods for customers to make transactions, enhancing the overall payment experience.

Mobile payments are payments that are made using a mobile device, such as a smartphone or tablet, Mac or MacBook device. Digital wallets are online accounts that store payment information, such as credit card numbers and bank account information. Both mobile payments and digital wallets offer many benefits over traditional payment methods (i.e., cash and checks). These benefits include convenience, security, and speed.

Ottu supports Apple Pay in Kuwait, Saudi Arabia (KSA), and Bahrain. This means that users in these countries can leverage the benefits of Apple Pay to make payments conveniently and securely.

![](<../.gitbook/assets/Apple Pay.png>)

## [Apple Pay Configuration Guide](apple-pay.md#apple-pay-configuration-guide)

To set up Apple Pay, you will need to:

* [Create a merchant ID](apple-pay.md#creating-merchant-id).
* [Add and verify a domain](apple-pay.md#adding-and-verifying-a-domain).
* [Create Apple Pay certificates](apple-pay.md#creating-apple-pay-certificates).

In addition, a merchant must have an Apple developer account to accept Apple Pay payments.

### [Creating Merchant ID](apple-pay.md#creating-merchant-id)

Merchant needs to have a developer account in Apple.

#### **1.** Log in to your Apple Developer account

#### **2.** Go to the Certificates, IDs & Profiles section

![](../.gitbook/assets/creating-merchant-id.png)

#### 3. From “App IDs” dropdown list ![](<../.gitbook/assets/image (11).png>)choose “Merchant IDs”, then click on ![](<../.gitbook/assets/image (14).png>) &#x20;

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

#### 4. From the identifier types, choose the Merchant IDs type and click Continue.

<div data-full-width="true">

<img src="../.gitbook/assets/merchant-ids (1).png" alt="">

</div>

#### [**5. Enter the Merchant Information**](apple-pay.md#5.-enter-the-merchant-information)

Fill out the fields, such as display name, description, and so on. The identifier field should include your Ottu installation URL in reverse order; For example, if the domain is `demo.ottu.net`, enter `net.ottu.demo`. Then click `Continue`.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

#### 6. Review the provided details and click on the Register button.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### [Adding and Verifying a Domain](apple-pay.md#adding-and-verifying-a-domain)

#### 1. From the Identifiers section, select the merchant ID that you created in the previous step (e.g., “Ottu Apple Pay”).

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### 2. Click on `Add Domain`

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

#### 3. Enter the domain URL you want to register and click Save.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

#### 4. Download the .text file and make sure it is ready for upload, as you will provide it to Ottu later (i.e., in the next step), which will be used for domain verification.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### 5. On the Ottu side, you should also add a new Apple Pay service and upload the .text file that you downloaded in the previous step, i.e., step 4:

&#x20;   5.1. Log in to the Ottu Dashboard and click on the three dots in the upper right corner to access    the Administration Panel.\
&#x20;   5.2. From the left-hand sidebar, select `Payment Service`.

<figure><img src="../.gitbook/assets/16-1 (1).png" alt=""><figcaption></figcaption></figure>

&#x20;  5.3. Click on `Add payment service`.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

&#x20;  5.4. Fill out the fields and click `Save`.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="233">Field</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>The name that will be displayed in the drop-down menu or any other location where the settings are displayed.</td></tr><tr><td>Code</td><td>A code that identifies the service in APIs, URLs, and other places.</td></tr><tr><td>Apple Merchant Identifier</td><td>The unique identifier that Apple assigns to merchants when they sign up for Apple Pay.</td></tr><tr><td>Display Name</td><td>The name that will be displayed on the payment sheet for Apple transactions</td></tr><tr><td>Domain</td><td>The domain that is configured for Apple Pay. For example, ksa.ottu.dev.</td></tr><tr><td>Domain Verification File</td><td><p>A file that contains a unique code used to verify the ownership of a specific domain name.</p><p>Here you should upload the <code>.text file</code> that you downloaded in the previous step, i.e., <a href="apple-pay.md#4.-download-the-.text-file-and-make-sure-it-is-ready-for-upload-as-you-will-provide-it-to-ottu-later">step 4</a>.</p></td></tr><tr><td>PG</td><td>The payment gateway</td></tr></tbody></table>

&#x20; 5.5. The new Apple Pay service for "merchant.net.ottu.demo" and "demo.ottu.net" has been successfully added.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

#### 6. Once you have added the Apple Pay service and submitted the .text file on the Ottu side, you should verify the domain on the Apple side:

On the Apple side, from the **Certificates, IDs & Profiles** section, scroll down to the **Merchant Domains** portion, and click `Verify`.\
Ottu will then confirm the completion of the verification.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### [Creating Apple Pay Certificates](apple-pay.md#creating-apple-pay-certificates)

#### **1.** Once again, from your Apple Developer account, go to the Certificates, IDs & Profiles section

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

#### &#x20;2. Scroll down to the Apple Pay Merchant Identity Certificate portion.

#### 3. Click Create Certificate.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

#### 4. Ottu will provide you with a Certificate Signing Request (CSR) file, which you will need in the next step:

&#x20;    4.1. Log in to the **Ottu Dashboard**, go to the **Administration Panel**, then from the left-hand   sidebar, select `Payment Service`.

&#x20;    4.2. Select the `payment service` that is associated with your `Merchant ID` & `Merchant Domain`.

If the payment service has not been added previously, follow the instructions above to [add a new Apple Pay payment service](apple-pay.md#5.-on-the-ottu-side-you-should-also-add-a-new-apple-pay-service-and-upload-the-.text-file-that-you-d).

<figure><img src="https://lh3.googleusercontent.com/PWC0M59qxYBSDRAgSWH5xMPH4nO7LTG9gb8MPHMN2vLqYYW97Ouuz_0YAhZO5zmGyqwwbALDfxvSU2pIbdskU10G1kApn8opySUs5bQgSDxbcx3owUEvJ82dYktpGd2D8ciwDu6cnkHkeAFUCZGmzEo" alt=""><figcaption></figcaption></figure>

&#x20;     4.3. Click `Download CSR file`.

<figure><img src="https://lh6.googleusercontent.com/U9aYBEaYkPnbzrmn-VpEZfdrSCUwT1_FH-ZswDnRvP6PFfur4TM5LkOwLlr33Q6InuIXg6SwLiVVkhuh0pmwcSkNvGOFuOCW3ctAZPQBuZGC5OoPBexHu5vzza-LNE_vYMY0Ofh8kPdCL9XrmF7Hetk" alt=""><figcaption></figcaption></figure>

#### 5. Upload the CSR file provided to you by Ottu in the previous step, i.e., step 4.

<figure><img src="https://lh4.googleusercontent.com/Xv1D2HLulUyGVijMN-RnBu4ka2e3gJfHQ9iMnGu-78Fd-Zex205M4PDSAo-PZuDkkEYj1UUF5JzTnyuC6tRKJOWQMckhNpCvYxQspjP3lLuFg1ZRLGo8zvaVQ1_Tj8pI8bbfTlf_IijZ8dHJa37iGwM" alt=""><figcaption></figcaption></figure>

#### 6. Click Continue, then click on Download to get the (.CER) file.

<figure><img src="https://lh5.googleusercontent.com/hwnUA8iIWXwKk7EYiGm2wn67vZoL6yCgVx4ZwEQMz95WXZXzt9l4s141-GXx78KWqbhGKSS3dkWp38c-bIdB0q2bOloam7ABL8L_I5bTJev7kxK2wM-T0M1ep6tqi7nbY6VeBOTuPe2an5qpy2aDmBs" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/f6JpVl5oULlBU4UGQCdSRPzYrvuRN5isnpljqHFqlP6cZy_5a5l7F6kAQfpMCiGZbPohaWNmI4V4bTvyNuQ97NeIpwfU201Ng9o_IUZMLpTr4XfAm5eiV6XhTWr1-fOdhIfM-zWmsS0ee6DxoeMS7XE" alt=""><figcaption></figcaption></figure>

#### 7. Upload the certificate (.CER) file to Ottu:

&#x20;   7.1. From the **Ottu Dashboard**, go to the **Administration Panel**, then from the left-hand sidebar, select `Payment Service`.

&#x20;   7.2. Select the same Apple Pay service that you selected in Step 4.2 under [Step 4](apple-pay.md#4.-ottu-will-provide-you-with-a-certificate-signing-request-csr-file-which-you-will-need-in-the-next).

&#x20;    7.3. Upload the `.CER` file that you downloaded in the previous step (i.e., [step 6](apple-pay.md#6.-click-continue-then-click-on-download-to-get-the-.cer-file.)) to **Apple Pay Identifier CER** and click `Save`.

<figure><img src="https://lh5.googleusercontent.com/V5lo5Az8NOcdJUcimM028lQvFZhe6iKYDv6swvb3LD88VH1KH7Qkn9EbT5LA0-YId0wb76Tt3UIOxfKLWmhyVp9UDahEzdIJQlyimS8n2RcaxRnapQtXyZ1HWAs4dx8zD07_OuoWmG6qXVLokME84cA" alt=""><figcaption></figcaption></figure>

After saving, the `PEM Certificate` and `Key File` will be generated automatically.

<figure><img src="https://lh6.googleusercontent.com/sf3X1kb6pbPR1lVABC9DT3egqzUyXSiknEuqFdYZY7vBNYygpRMUOdh2ajp2bj3ge7u-6RxWQluM65igjRuhMhJ390lPY2AgbwV3MGAHALLoIH_cRxFLV1AC2OzRG4Nh5mqLZFBMnZ1iIuF6NUTnqLw" alt=""><figcaption></figcaption></figure>

## [Creating Apple Payment Processing Certificate](apple-pay.md#creating-apple-payment-processing-certificate)

Ottu empowers businesses with a seamless payment process through Apple Pay across [MPGS](https://docs.ottu.com/user-guide/apple-pay/setup-mpgs), [Cybersource](https://docs.ottu.com/user-guide/apple-pay/setup-cybersource), and KNET gateways. Below, discover the effortless setup process for integrating different payment gateways with Apple Pay through Ottu. Experience simplified payments and enhanced user experience with our seamless integration process.

{% content-ref url="apple-pay/setup-mpgs.md" %}
[setup-mpgs.md](apple-pay/setup-mpgs.md)
{% endcontent-ref %}

{% content-ref url="apple-pay/setup-cybersource.md" %}
[setup-cybersource.md](apple-pay/setup-cybersource.md)
{% endcontent-ref %}
