# Operation request flow

Ottu enables merchant to give his permission to different users to proceed refund or void operation, then the users can perform the approved operation either from the dashboard or over the API.

## ****[**Maker &  checker definition**](operation-request-flow.md#maker-and-checker-definition)****

**Maker:** Is the one who performs a transaction for refund or void and send the approval request.\
**Checker:** Is the one who is authorized to approve or reject the received request.&#x20;

{% hint style="info" %}
Merchant can have many makers, but only one checker.\
Checker can proceed refund/void directly.
{% endhint %}

## ****[**Operation request flow key features**](operation-request-flow.md#operation-request-flow-key-features)****

* This feature will be activated by adding operations approval plugin\
  Ottu dashboard> administration panel > Plugins > installed plugins.

<figure><img src="../../../.gitbook/assets/installed_plugins.png" alt=""><figcaption></figcaption></figure>

Then add operation approval in plugins window.

<figure><img src="../../../.gitbook/assets/activate_request.png" alt=""><figcaption></figcaption></figure>

* Only one authorized user can assign a checker user.\
  To assign checker:\
  Ottu dashboard> administration panel >Operations Approval Plugin>Operations Approval Plugin Config

<figure><img src="../../../.gitbook/assets/Assign_checker (1).png" alt=""><figcaption></figcaption></figure>

* A void, or a refund operation request, will remain pending until the checker makes his decision of approval or rejection. Then the operation request will transit to rejection state or done state. Within the approval state, any user with permission could trigger an attempt by retry button until it is succeeded or turns to **Expired** state once the expiration time gets passed.
* Any operation request in pending state could be canceled from the [operation request table](operation-request-flow.md#operation-request-table) by all users except the checker.

{% hint style="info" %}
[Operation request table](operation-request-flow.md#operation-request-table) should be auto-synced updated.
{% endhint %}

* Another operation request can't be submitted if there is a pending operation request for the same transaction.\
  The below message should be displayed\
  **(Requested {Operation} is pending for approval).**
* &#x20;If the checker manages somehow to approve a canceled request, just return an error message that this request has been canceled.
* After getting the approval from the checker, the operation will be executed automatically,  then the state will transit to **Done** state. If it fails, will go to **Manual Action Required** state.
* After the approval decision is made, the maker should be notified that manual action is required if the process fails.
* Expiration time should be given after the transaction state transited to **Manual Action Required**. If the expiration time passed with no change, the transaction state will transit to **Expired** state.

{% hint style="info" %}
The default time will be 48 hrs, defined from backend.
{% endhint %}

* &#x20;For the same transaction, the refund or void request is not allowed once the operation request has turned to **Done** state, unless there is still remaining funds, then only **refund** request can be processed.
* The authorized user who has the authority to assign checker can add whitelisted users, this could be implemented from: administration panel > operations approval plugin > operations approval plugin config.
* An email notification will be triggered after the process ends.

{% hint style="info" %}
**In case the transaction transit to manual action required state,**\
**** the maker should be notified. \
**In case of refund / void operation with Done state,** \
an email will be sent to the customer and the maker could be received it after implementing required configuration Ottu dashboard > Administration panel > unit > unit configs> select unit configs, then scroll down to Bcc initiator check box and tick it.\
**In case of refund / void operation with rejection state,** \
an email should be sent to the maker only.
{% endhint %}

## ****[**Operation request table**](operation-request-flow.md#operation-request-table)****

A table for operation requests has been created under the ticket tab, where the checker can approve/reject operation requests, and makers can cancel operation request there, along with the history of operation request transactions.\
A filter will be utilized based on state, operations, date, pg, and currency to show the required request details.

<figure><img src="../../../.gitbook/assets/Operations_Requests.png" alt=""><figcaption></figcaption></figure>

| Header               | Description                                                                                                                                                                                                             |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ID**               | **ID of the operation request.**                                                                                                                                                                                        |
| **DATE**             | **The date of creating the request.**                                                                                                                                                                                   |
| **REQUESTED BY**     |  **Request initiator.**                                                                                                                                                                                                 |
| **FOR TRANSACTION**  | **Original transaction ID (by clicking on, popup will be shown the payment transaction details).**                                                                                                                      |
| **OPERATION**        | **Refunded/Voided.**                                                                                                                                                                                                    |
| **AMOUNT**           | **The amount of the payment transaction.**                                                                                                                                                                              |
| **OPERATION AMOUNT** | **The amount of the requested operation.**                                                                                                                                                                              |
| **STATUS**           | **Operation request status (pending, approved, rejected, manual action or expired).**                                                                                                                                   |
| **CURRENCY**         | **Currency of the payment transaction amount.**                                                                                                                                                                         |
| **PAYMENT GATEWAY**  | **Payment gateway involved.**                                                                                                                                                                                           |
| **ACTION**           | <p><strong>Approve.</strong> <br><strong>Reject.</strong> <br><strong>Retry,</strong> in case of manual action required for all users. <strong></strong> <br><strong>Cancel,</strong> for all users except checker.</p> |

## ****
