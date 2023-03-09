# Configuration

## [Global Configuration](configuration.md#global-configuration)

Ottu empowers the merchant to make his own configuration easily.

## [Global <mark style="color:blue;"></mark> Configuration walkthrough](configuration.md#global-configuration-walkthrough)

Ottu dashboard> administration panel > Config

![](<../.gitbook/assets/1 (5).png>)

### [Configuration](configuration.md#configuration)

Ottu dashboard> administration panel > Config > Configuration

<figure><img src="../.gitbook/assets/Configuration.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Global configuration.png" alt=""><figcaption></figcaption></figure>

#### ****[**Merchant Name or title**](configuration.md#merchant-name-or-title)****

Merchant’s name to be displayed on the dashboard.

#### ****[**Merchant subheader**](configuration.md#merchant-subheader)****

A merchant's most important information is briefly highlighted in this field.

#### ****[**Logo**](configuration.md#logo)****

Merchant trade market logo

#### ****[**Favicon**](configuration.md#favicon)****

An icon displayed beside URL in the browser tab or next to the site name in a user's list of bookmarks.

#### ****[**Merchant Website URL**](configuration.md#merchant-website-url)****

URL address of the merchant website.

#### [**Email**](configuration.md#email)****

This email address gets the request for authorizing the permission. Should be only one email address. Normally, it is the email of the installation owner.

#### ****[**Merchant phone**](configuration.md#merchant-phone)****

Merchant contact phone number.

#### ****[**Fixer access key**](configuration.md#fixer-access-key)****

Fixer.io, is an online service to get currency exchanging rate on real time base, when the merchant creates an account on fixer.io he will get a fixer access key. To be effective, the merchant should enable the online conversion feature.\
Ottu dashboard > Administration panel > currency > currency exchange configs \[works as should choose online].

#### ****[**Is paused**](configuration.md#is-paused)****

Enabling it, will lead to freeze all the transaction process related to payment request link till it gets disabled.

#### [**Enable 2FA on**](configuration.md#enable-2fa-on)****

Enabling it, will send OTP to the dedicated email address for every dashboard logging in action.

#### ****[**Live update**](configuration.md#live-update)****

It is the date of the first live transaction.

#### ****[**Renewal update**](configuration.md#renewal-update)****

Installation renewal date, It is determined by the signed contract between Ottu and the merchant.

#### [**Expire payment transaction?**](configuration.md#expire-payment-transaction)****

It is the time of proceeding the payment by the customer through payment request URL.\
When expiry date passed, then the payment will be transited to expired state and payment request URL will be expired.\
By default, is 24 hours and could be changed from Ottu dashboard > Administration panel > payment request > payment request configuration\[Transaction expiration time].

#### ****[**Notes**](configuration.md#notes)****

Where the merchant can write a note for any additional configuration other than default configuration.

#### ****[**SSL expiry date**](configuration.md#ssl-expiry-date)****

Security socket layer, it is used to secure a merchant’s server and should be updated according to the SSL expiry date which is populated automatically based on the date of the installation.

#### ****[**Reference Prefix**](configuration.md#reference-prefix)****

It is for Ottu operations team, If a merchant is having two Ottu installations and one PG then one of the installation should have the reference prefix configured to avoid duplicate track ID for the PG.

#### [Enable Session Timeout](configuration.md#enable-session-timeout)

If ticked, a user without refund/void permissions will be logged out after passing the defined session timeout.

#### [Enable URL Shortener](configuration.md#enable-url-shortener)

If ticked, URL shortener will be activated for public link. See [URL shortener configuration](configuration.md#url-shortener-configurations).

### ****[**URL shortener configurations**](configuration.md#url-shortener-configurations)****

Ottu dashboard> administration panel > Config > URL shortener configurations

<figure><img src="../.gitbook/assets/URL.png" alt=""><figcaption></figcaption></figure>

Then click on **Add URL shortener configuration.**

<figure><img src="../.gitbook/assets/URL_Confg.png" alt=""><figcaption></figcaption></figure>

#### <mark style="color:green;">Fields description</mark>

#### [URL shortening tool ](configuration.md#url-shortening-tool)

Determine the shortening utility.

#### ****[**API URL**](configuration.md#api-url)****

API endpoint link.

#### [User & Password](configuration.md#user-and-password)&#x20;

Credential.

#### ****[**Is global**](configuration.md#is-global)

If ticked, this configuration will be applied for all cased.

## ****[**Transaction report generation Configuration**](configuration.md#transaction-report-generation-configuration)****

Ottu dashboard> administration panel > Report > Periodic Transaction Report Config

<figure><img src="../.gitbook/assets/report (1).png" alt=""><figcaption></figcaption></figure>

### [General](configuration.md#general)

<figure><img src="../.gitbook/assets/Periodic transaction report general configuration.png" alt=""><figcaption></figcaption></figure>

#### <mark style="color:green;"></mark>[<mark style="color:green;">Fields description</mark>](configuration.md#undefined)

#### [Period](configuration.md#period)

How often report generating process recurs.

#### [Report Generation Date/Time](configuration.md#report-generation-date-time)

When report generation process starts.

#### [Plugins](configuration.md#plugins)

Which plugin the report belong.

#### [Send transaction report by email](configuration.md#send-transaction-report-by-email)

If ticked, the report will send to merchant email automatically.

#### [Emails](configuration.md#emails)

Define the email addresses, where the report required to be sent to.

#### [Email notification template ](configuration.md#email-notification-template)

The used template for email notifications.

#### [File name prefix](configuration.md#file-name-prefix)

&#x20;Determine the format in which reports should be saved.

### [Periodic Transaction report fields](configuration.md#periodic-transaction-report-fields)

To add a new field to the required report, go to **Periodic Transaction report fields** Tab.

<figure><img src="../.gitbook/assets/Periodic transaction report fields.png" alt=""><figcaption></figcaption></figure>

#### [<mark style="color:green;">Fields description</mark>](configuration.md#undefined)<mark style="color:green;"></mark>

{% hint style="info" %}
The report will export each field that is added here.
{% endhint %}

{% hint style="info" %}
Based on the source type where the field data is extracted, the required fields are categorized, **Config**, **Static**, **Gateway response**, and **Common**.\
For each type of added field, different information is required.
{% endhint %}

<details>

<summary><strong>Type:</strong> Config <br>(Required field is from Plugins configuration)</summary>

**Is active?:** If ticked, It would be used and displayed.&#x20;

**Field:** Built in field list where the required field would be inserted.

**Label \[en]:** Create a custom label if the built-in fields do not meet your needs. For English language.

**Label \[ar]:** Create a custom label if the built-in fields do not meet your needs. For Arabic language.

**Name:** HTML field name, used for backend validation, and will not be shown anywhere.

**Order:** The location of the required field in the generated report.

</details>

<details>

<summary><strong>Type: Static</strong>  <br><strong>(Required field is one of the constant fields)</strong></summary>

**Is active?:**If ticked, It would be used and displayed.

**Label \[en]:** Create a custom label of the required field. For English language.

**Label \[ar]:** Create a custom label of the required field. For Arabic language

**Name:** HTML field name, used for backend validation, and will not be shown anywhere.

**Static value:** Define the value of constant field. It will be the same for all generated reports.

**Order:** The location of the required field in the generated report.

</details>

<details>

<summary><strong>Type: Gateway response</strong> <br><strong>(Required field is from PG response)</strong></summary>

**Is active?:**If ticked, It would be used and displayed.

**Label \[en]:** Create a custom label of the required field. For English language.

**Label \[ar]:** Create a custom label of the required field. For Arabic language

**Name:** HTML field name, used for backend validation, and will not be shown anywhere.

**Gateway response keys:** Since PG response sent in Dict {Key:Value}\
Here, the keys of the required value are determined.

**Order:** The location of the required field in the generated report

</details>

<details>

<summary><strong>Type: Common</strong> <br><strong></strong>(Required field is other than fields from PG, static, and plugin configuration. Such as payment_date).</summary>

**Is active?:**If ticked, It would be used and displayed.

**Label \[en]:** Create a custom label of the required field. For English language.

**Label \[ar]:** Create a custom label of the required field. For Arabic language.

**Name:** HTML field name, used for backend validation, and will not be shown anywhere.

**Order:** The location of the required field in the generated report

</details>

## ****[**Webhook configuration**](configuration.md#webhook-configuration)****

Ottu Dashboard > administration panel > Webhook >Webhook Config

<figure><img src="../.gitbook/assets/webhook.png" alt=""><figcaption></figcaption></figure>

### [General](configuration.md#general-1)

<figure><img src="../.gitbook/assets/webhook config.png" alt=""><figcaption></figcaption></figure>

#### <mark style="color:green;"></mark>[<mark style="color:green;">Fields description</mark>](configuration.md#undefined)<mark style="color:green;"></mark>

#### [HMAC Key](configuration.md#hmac-key)

&#x20;For API payloads, this key is used to generate [hash signatures](../developer/hash-signature.md).

#### [Ignore ssl](configuration.md#ignore-ssl)&#x20;

When ticked, SSL certificate will not be verified while calling [webhook\_url](../developer/rest-api/checkout-api.md#webhook\_url-url-optional)

#### [Notify on error](configuration.md#notify-on-error)&#x20;

When ticked, an email will be sent in case of any error occurred while calling [webhook\_url](../developer/rest-api/checkout-api.md#webhook\_url-url-optional).

#### [Email list](configuration.md#email-list)&#x20;

Email address list, where the calling [webhook\_url](../developer/rest-api/checkout-api.md#webhook\_url-url-optional) error notification email should be sent to.

#### [Timeout](configuration.md#timeout)&#x20;

the period of the time that Ottu server waits for the merchant server to response.

#### [Retries](configuration.md#retries)&#x20;

Number of the times that Ottu server retries to send the request to merchant server

#### [Backoff factor](configuration.md#backoff-factor)&#x20;

Period of time between two retries.

#### [Example:](configuration.md#example)&#x20;

**Timeout = 20 sec, retries= 3 and Back off factor =5sec.**

Merchant server is down for 30 sec.&#x20;

1- First try, Ottu sends the request, then waits for response 20 sec and 5 sec more as back off factor. Failed \
2- Second try, Ottu sends another request, then waits for 5 sec then gets response from the merchant order, since the server downtime is over and returned to normal mode. **Succeeded**

#### [Version](configuration.md#version-1)

API webhook. Version

#### [Enable webhook notifications?](configuration.md#enable-webhook-notifications)

&#x20;When ticked, webhook notification will be activated.

{% hint style="info" %}
**Redirect behavior based on webhook\_url response on payment events and payment operation:**

* status code 200, the customer will be redirected to redirect\_url
* status code 201, the customer will be redirected to Ottu payment summary page
* status code any other code, the customer will be redirected to Ottu payment summary page. For this particular case, Ottu can notify on the email, when Enable webhook notifications? Activated..
{% endhint %}

#### [Enable retry webhook mechanism?](configuration.md#enable-retry-webhook-mechanism)

When ticked, Ottu will another request in case first one gets failed. See [example](configuration.md#example).

#### [Operations webhook\_url](configuration.md#operations-webhook\_url)&#x20;

Where the transaction data will get disclosed once operation transaction flow triggered.

#### [Enable webhook notifications if transaction initiated from API](configuration.md#enable-webhook-notifications-if-transaction-initiated-from-api)

When ticked, webhook notification will be activated even the transaction gets created over API.

### [Webhook Plugin Configs](configuration.md#webhook-plugin-configs)

In this Tab, the merchant can define which and how webhook works in the required plugin.

<figure><img src="../.gitbook/assets/Webhook plugin configs.png" alt=""><figcaption></figcaption></figure>

#### [Webhook Plugin](configuration.md#webhook-plugin)

Define which plugin webhook works for.

#### [Webhook Url](configuration.md#webhook-url)

In case of a payment event or payment operation, Ottu triggers an HTTP request to this URL, to disclose transactional data.

#### [Enable transaction state webhook notifications?](configuration.md#enable-transaction-state-webhook-notifications)

If ticked, webhook notification will be push against defined [status](configuration.md#notification-status).

#### [Notification status](configuration.md#notification-status)

Define the status where the webhook notification will be triggered.

{% hint style="info" %}
Status: Paid, Failed, Authorized, and Canceled. See [payment transaction states](payment-tracking.md#states-of-parent-payment-transaction).
{% endhint %}

#### [Delete](configuration.md#delete)

To remove defined plugin webhook configuration.

{% hint style="info" %}
The [webhook\_Url](configuration.md#webhook-url) specified in the [webhook plugin configuration](configuration.md#webhook-plugin-configs) serves as the endpoint for receiving notifications related to both [payments](../developer/webhook/payment-notification.md) and [operations](../developer/webhook/operation-notification.md). If we provide values for both the [operation webhook\_url](configuration.md#operations-webhook\_url) and the [webhook\_Url](configuration.md#webhook-url) in the plugin configuration, the system will transmit data to both URLs.
{% endhint %}

For more information about how and where webhook works in Ottu see [webhook](../developer/webhook/).
