# Transaction Report Configuration

Learn how to configure Ottu's transaction reports for valuable insights into your payment activities. This section guides you through the setup process, allowing you to generate comprehensive reports tailored to your business needs.

## [**Report Generation Configuration Walkthrough**](https://app.gitbook.com/o/zPYVxVRFHGcXaJwu5Lii/s/6jPPS6SrHa77njezUydM/\~/changes/1/user-guide/configuration/transaction-report-configuration#report-generation-configuration-walkthrough)

To access the Configuration of Transaction Report Generator, navigate to Ottu Dashboard > Administration Panel > Report > Periodic Transaction Report Config.

<figure><img src="../../.gitbook/assets/report (1).png" alt=""><figcaption></figcaption></figure>

### [General](transaction-report-configuration.md#general)

<figure><img src="../../.gitbook/assets/Periodic transaction report general configuration.png" alt=""><figcaption></figcaption></figure>

#### **Description of Fields:**

* **Period:** Specify how often the report generation process should recur.
* **Report generation date/time:** Set the date and time when the report generation process should start.
* **Plugins:** Which plugin the report belongs to.
* **Send transaction report by email:** If this checkbox is checked, the report will be sent to the merchant's email address automatically.
* **Emails:** The email addresses to which the report should be sent.
* **Email notification template:** The template used for email notifications.
* **File name prefix:** The filename prefix can be used to specify the format in which reports are saved.

### [Periodic Transaction Report Fields](transaction-report-configuration.md#periodic-transaction-report-fields)

To include a new field in the report, navigate to the **Periodic Transaction Report Fields** tab.

<figure><img src="../../.gitbook/assets/Periodic transaction report fields.png" alt=""><figcaption></figcaption></figure>

#### Description of Fields:

{% hint style="info" %}
Each field added here will be included in the report.
{% endhint %}

{% hint style="info" %}
The required fields are categorized based on the source type from which the data is extracted. The categories are as follows: [**Config**](transaction-report-configuration.md#type-config-required-field-is-from-plugins-configuration), [**Static**](transaction-report-configuration.md#type-static-required-field-is-one-of-the-constant-fields), [**Gateway Response**](transaction-report-configuration.md#type-gateway-response-required-field-is-from-pg-response), and [**Common**](transaction-report-configuration.md#type-common-required-field-is-other-than-fields-from-pg-static-and-plugin-configuration.-such-as-pay), and each type requires different information.
{% endhint %}

<details>

<summary><strong>Type:</strong> Config <br>(Required field is from Plugins configuration)</summary>

* **Is active:** If you check this box, the field will be used and displayed.
* **Field:** A pre-populated list of fields where the required field should be selected.
* **Label \[en]**: Create a custom label for the field in English, if needed.
* **Label \[ar]:** Create a custom label for the field in Arabic, if needed.
* **Name:** The name of the HTML field, which is used for backend validation. It will not be displayed anywhere.
* **Order:** The location of the field in the generated report

</details>

<details>

<summary><strong>Type: Static</strong>  <br><strong>(Required field is one of the constant fields)</strong></summary>

* **Is active:** If checked, the field will be used and displayed.
* **Label \[en]:** Create a custom label for the field in English, if needed.
* **Label \[ar]:** Create a custom label for the field in Arabic, if needed.
* **Name:** The name of the HTML field, which is used for backend validation. It will not be displayed anywhere.
* **Static value:** Assign a value to the constant field. It will be the same for all generated reports.
* **Order:** The location of the field in the generated report.

</details>

<details>

<summary><strong>Type: Gateway response</strong> <br><strong>(Required field is from PG response)</strong></summary>

* **Is active:** If checked, the field will be used and displayed.
* **Label \[en]:** Create a custom label for the field in English, if needed.
* **Label \[ar]:** Create a custom label for the field in Arabic, if needed.
* **Name:** The name of the HTML field, which is used for backend validation. It will not be displayed anywhere.
* **Gateway response keys:** Since the PG (Payment Gateway) response is sent in a dictionary format, i.e., a set of key-value pairs {Key:Value}, so here you specify the keys of the values you need.
* **Order:** The location of the field in the generated report.

</details>

<details>

<summary><strong>Type: Common</strong> <br>(Required field is other than fields from PG, static, and plugin configuration. Such as payment_date).</summary>

* **Is active:** If checked, the field will be used and displayed.
* **Label \[en]:** Create a custom label for the field in English, if needed.
* **Label \[ar]:** Create a custom label for the field in Arabic, if needed.
* **Name:** The name of the HTML field, which is used for backend validation. It will not be displayed anywhere.
* **Order:** The location of the field in the generated report.

</details>
