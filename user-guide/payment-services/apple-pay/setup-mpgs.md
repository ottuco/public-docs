# Setup MPGS

## [Integration Process for Apple Pay and MPGS](setup-mpgs.md#integration-process-for-apple-pay-and-mpgs)

Merchants can provide customers with a secure and user-friendly payment option for transactions made via Apple devices by integrating Apple Pay with MPGS (Mastercard Payment Gateway Services). Setting up and configuring their MPGS account to accept Apple Pay transactions is a straightforward process for merchants, as detailed in the steps below.

#### [Prerequisites](setup-mpgs.md#prerequisites)

* Valid MPGS account
* Apple Developer account

### [Obtaining Apple Pay (CSR)](setup-mpgs.md#obtaining-apple-pay-csr)

1. Log in to the Mastercard Payment Gateway Services (MPGS) portal and go to **Device Payments**.

<figure><img src="https://lh4.googleusercontent.com/FJyg888ciFPd46yxhaPWIIWaXhRpbEOjdQTIlOD0LiWqy9S9b1WCYlYP-jzpcFNsbZhpL1puwRPE1yrw9AAzXZ1uv98u7iJE5J09a7VxPn4kEP7ukg4I0Cr5_WIb-zUPQZ-2kn1KC2MnMcWqdbZae_U" alt=""><figcaption></figcaption></figure>

2. Click on **Add New Certificate**.
3. Download the **Apple Pay Certificate Signing Request (CSR)** file.

<figure><img src="https://lh3.googleusercontent.com/ujogPgk7aMjxiErrH02IdfnehLi3LqitKPp-aj-kpvRSvAxct1oVneiJANcLqon6tPEo9Dai2jkue68sslzSLYDqnXo8f38nrLfIZAgMHwJ3LKAPSNxih2WIi9SCtFajxMPFhAlhL4iEMHl4z-4FDuY" alt=""><figcaption></figcaption></figure>

### [Obtaining Apple Pay Payment Processing Certificate](setup-mpgs.md#obtaining-apple-pay-payment-processing-certificate)

1. Log in to your Apple Pay Developer account and go to the [Merchant ID](./#creating-merchant-id) that you created. Under Apple Pay Payment Processing Certificate, click **Create Certificate**.

<figure><img src="https://lh5.googleusercontent.com/pNntwm5qNaMIzBNLFAPgoLorHMH8n88zErJXYOL0b8TWi0g97iwVhZocMSm5thmZqOJeZ1GF9NCTmxwrKj7uKUQuZNYVpAKA10KfbBYUGUdHCAZeaCBwaGyAzypC0OT27Yf7LaTLk1hsKtJPElKpYGY" alt=""><figcaption></figcaption></figure>

2. On the next page, choose **No** and **continue**.

<figure><img src="https://lh4.googleusercontent.com/rrA94AED4j1lVhCqPGCAw-9uJYFUJwY4tLOjbXg7zlacrLWmBgD_y9WbUhPYGIkC-e9l6gWKiOLrKA-5x6PZ8na0ZmoilKn1BE3Wd5Q3o8MrlLe6zcD2Rb48NMTHf05Ms2cbexv6-ZjuKlLPqvtxn5Q" alt=""><figcaption></figcaption></figure>

3. **Upload** the [CSR](setup-mpgs.md#obtaining-apple-pay-csr) (i.e., the Certificate Signing Request) file that you downloaded from the MPGS portal.

<figure><img src="https://lh3.googleusercontent.com/mSZZbWOWFwx-GgbJ0vhBIkNMNqGwHB1IYwKXnywvq-yH-afvL5F5HITdYy618kE9y-NwgjDPcPYrJWYmN2jY-oal1PEy4KwVFsERB5Dr9yc2Xg2BHYY5on8RZPPAkiWU9QQuaMchfSEr1FcC2cUje0s" alt=""><figcaption></figcaption></figure>

4. **Download** the resulting file, which is the Apple Pay Payment Processing Certificate file.

<figure><img src="https://lh5.googleusercontent.com/-R8m1wuN1-Jzbilqu5Taja2ggIraqHMOQQMVPb96Aj1JlC-0OzZTSwX5-J2ROXWtmfVXOE4KVFQIGL7R0Vp4pJlcxUDXDdhtdh5cSvLJm12A7xG30NajxolJ7kfbn7ooKPCEt8mFOrOjTa6_kBTcp70" alt=""><figcaption></figcaption></figure>

### [Providing Apple Pay Payment Processing Certificate](setup-mpgs.md#providing-apple-pay-payment-processing-certificate)

1. Log in to the Mastercard Payment Gateway Services (MPGS) portal and go to **Device Payments**.

<figure><img src="https://lh4.googleusercontent.com/FJyg888ciFPd46yxhaPWIIWaXhRpbEOjdQTIlOD0LiWqy9S9b1WCYlYP-jzpcFNsbZhpL1puwRPE1yrw9AAzXZ1uv98u7iJE5J09a7VxPn4kEP7ukg4I0Cr5_WIb-zUPQZ-2kn1KC2MnMcWqdbZae_U" alt=""><figcaption></figcaption></figure>

2. **Upload** this [certificate file](setup-mpgs.md#obtaining-apple-pay-payment-processing-certificate) (i.e., the Apple Pay Payment Processing Certificate file from the previous step) to the MPGS portal. Refer to the instructions provided in **“step 6”**, as shown in this figure.

<figure><img src="https://lh6.googleusercontent.com/chu7LHi4ZhUDrSCfLuxf98fITgX_RWY7rJGdWmj8uHGk5G5QP7asQO9Dkv7_j9IqpEC7vqjRgcqZMNQ-3mZRXpSayS_6MUDwu5nW1WELI9_MjLWqdv6SAijicdK4MEfgkZWcE3k5sEXhQ0B3hGhOebA" alt=""><figcaption></figcaption></figure>
