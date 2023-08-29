# Global Configuration

Ottu puts the power of configuration in the hands of merchants, ensuring a hassle-free experience. Our mission is to provide you with seamless configuration options, enabling you to take absolute control like never before! With Ottu, merchants can customize their dashboard, logo, favicon, website URL, email address, phone number, and more. Embrace the power of Ottu and unlock a world of possibilities for your business. Get ready to revolutionize your experience with our user-friendly tools and elevate your online presence to new heights!

## [Global Configuration Walkthrough](global-configuration.md#global-configuration-walkthrough)

**Fine-Tune Your Settings with Ottu's Configuration Options**\
**To access the configurations:** from the Ottu Dashboard > navigate to the Administration Panel > head to Config > then select `Configuration`.

<figure><img src="../../.gitbook/assets/Configuration.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Global configuration.png" alt=""><figcaption></figcaption></figure>

**Here is a brief explanation of each field:**

#### **Merchant Identity - Stand Out in Style**

* **Merchant name or title:** Display your preferred name on the dashboard.
* **Merchant subheader:** Highlight vital information concisely to grab attention.

#### **Branding - Making Your Mark**

* **Logo:** Showcase your unique market logo.
* **Favicon:** Capture attention with an eye-catching icon next to your site's name in bookmarks and browser tabs.

#### **Connectivity - Stay in the Loop**

* **Merchant website URL:** The URL address of the merchant's website.
* **Email:** The email address that will receive the permission authorization request. Typically, this is the installation owner's email.
* **Merchant phone:** The merchant's contact phone number.

#### **Seamless Currency Exchange**

* **Fixer access key:** The fixer access key is used to get currency exchange rates from Fixer.io API. **Fixer.io** is an online service that provides real-time currency exchange rates. Obtain your Fixer Access Key by creating an account on their website.

{% hint style="info" %}
For this service (i.e., Fixer.io) to be active, remember to enable the online conversion feature: from Ottu Dashboard > Administration Panel > Currency > Currency Exchange Configs \[set the field “Work as” to the value “Online”].
{% endhint %}

#### **Control Your Functionality**

* **Is paused:** Allows merchants to temporarily disable or freeze all their payment request links (i.e., freeze all payment transaction processes). This can be useful if the merchant is experiencing technical problems or if they are taking a break from accepting payments.
* **Enable 2FA:** Allows merchants to enable two-factor authentication for their dashboard logins.

{% hint style="info" %}
In 2FA, An OTP (One-Time Passcode)—a single-use numerical passcode—will be sent to your email for every dashboard login. This adds an extra layer of security by requiring merchants to enter the code in addition to their password for each login action.
{% endhint %}

#### **Time-Sensitive**

* **Live date:** The date of the first live transaction.
* **Renewal date:** The renewal date is the date when the merchant's Ottu subscription will end. It is important for merchants who want to renew their subscriptions to keep using Ottu. It is specified in the contract signed between Ottu and the merchant.
* **Expire payment transaction:** If activated, an expiration date will be set for the customer to complete the payment through the payment request URL.

{% hint style="info" %}
* When the expiration date passes, the payment will be transitioned to the Expired state, and the payment request URL will no longer be valid.
* By default, the expiration date is 24 hours, but it can be modified: From the Ottu Dashboard > Administration Panel > Payment Request > Payment Request Configuration \[Transaction Expiration Time]
{% endhint %}

#### **Customization and Extra Notes**

* **Notes:** A place for the merchant to write any additional configuration notes.

#### **Security and Efficiency**

* **SSL expiry date:** The SSL (Security Socket Layer) expiry date is the date when the merchant's SSL certificate will expire, and It is automatically populated based on the installation date. SSL guarantees the security of the merchant server.

#### **Effortless Management of Multiple Installations**

* **Reference prefix:** A unique prefix that can be used by the Ottu Operations Team to avoid duplicate track IDs for the PG ([Payment Gateway](../payment-gateway.md)), as duplicating IDs can cause problems with tracking and reporting.

{% hint style="info" %}
If you have multiple Ottu installations and one PG (Payment Gateway), one of the installations should have the Reference Prefix so that each installation has a unique track ID, even if they use the same PG.
{% endhint %}

#### **Fine-Tune User Experience**

* **Enable session timeout:** If checked, users who do not have refund/void permissions will be automatically logged out after passing the defined session timeout.
* **Enable URL shortener:** Activate the URL shortener for public links, making sharing links with others hassle-free. Check [URL Shortener Configuration](global-configuration.md#url-shortener-configurations) for more details.

Empower your online payment management with Ottu's remarkable configuration options. Let positive impressions, seamless transactions, and engaging experiences define your journey to success.
