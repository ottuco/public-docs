# Refund & Void Access Control

At Ottu, our cutting-edge online payment management systems (OPMS), we have established a simplified permission approval process to enhance security, efficiency and ensure smooth operations. The process entails obtaining permissions (i.e., **do refund/void permissions**.) to perform operation requests via [Two-Step Refund & Void Authorization](two-step-refund-and-void-authorization.md), which is mandatory for Staff users. On the other hand, Super users enjoy greater freedom, they skip this flow by default.

{% hint style="success" %}
This convenient feature will be activated by adding the permissions approval plugin. Just head to Ottu Dashboard > Administration Panel > Plugins > Installed Plugin.
{% endhint %}

However, to ensure accountability and security, only a single user should be authorized to approve or reject **refund/void** permission requests. If you are worried about pending requests cluttering your system, then fear no anymore! By default, all requests will automatically be canceled after 48 hours. And guess what? You can even customize this expiration time from the backend to suit your needs!

## [Refund & Void Access Control W**alkthrough**](https://app.gitbook.com/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/408/user-guide/plugins/features/refund-and-void-access-control#refund-and-void-access-control-walkthrough)

Access the Ottu Dashboard > Navigate to the Administration Panel > Go to the User section and select Users.

<figure><img src="../../../.gitbook/assets/Users (1).png" alt=""><figcaption></figcaption></figure>

Select the target user. Here is the **Example** user.

<figure><img src="../../../.gitbook/assets/Select_user (2).png" alt=""><figcaption></figcaption></figure>

Within the user's profile, go to the **Permissions** tab. Scroll down to the section titled **User Permissions"** In the **Available User Permissions** window, select the following permissions:

**— payment | Payment transaction | Can do refund**

**— payment | Payment transaction | Can do void**

Move them from the **Available User Permissions** window to the **Chosen User Permissions** window. Finally, click on the `Save` button to save the changes.

<figure><img src="../../../.gitbook/assets/permissions.png" alt=""><figcaption></figcaption></figure>

## [Table of Permission Requests](https://app.gitbook.com/s/iUKrMb9zLt5ZzGPUYDsK/\~/changes/408/user-guide/plugins/features/refund-and-void-access-control#table-of-permission-requests)

In the Ottu Dashboard, under the **Tickets Tab**, there is a **Permissions Requests Table** where authorized users are listed. A permission request will be sent to the authorized user via this table, and the authorized user can then approve or reject the request, as illustrated below.

<figure><img src="../../../.gitbook/assets/permissions_request.png" alt=""><figcaption></figcaption></figure>

