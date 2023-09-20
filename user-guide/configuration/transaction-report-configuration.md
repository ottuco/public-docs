# Transaction Report Configuration

In this comprehensive guide, we will walk you through the setup and customization of essential reports within Ottu, ensuring that you can efficiently manage your online payment transactions. This section is divided into two subsections, each focusing on a distinct report configuration:

**Periodic Transaction Report Configuration:** This subsection will provide you with step-by-step instructions on how to set up and tailor the `Periodic Transaction Report` to meet your specific business needs. Whether you require daily, weekly, or monthly transaction summaries, we've got you covered.

**Flexible Transaction Report Configuration:** Here, you will find detailed guidance on configuring the `Flexible Transaction Report` within Ottu. This report allows you to create customized transaction reports, filtering and selecting the data that matters most to your organization. We'll help you harness the power of Ottu's flexibility to generate insightful transaction reports.

With these configurations, Ottu empowers you to gain deeper insights into your online payment transactions, facilitating informed decision-making and enhanced financial management. Let's get started on optimizing your Ottu experience with these essential report configurations.

## [**Periodic Transaction Report Configuration**](transaction-report-configuration.md#periodic-transaction-report-configuration)

To access the Configuration of Transaction Report Generator, navigate to Ottu Dashboard > Administration Panel > Report > Periodic Transaction Report Config.

<figure><img src="../../.gitbook/assets/report (1).png" alt=""><figcaption></figcaption></figure>

Click on :heavy\_plus\_sign:`Add Periodic Transaction Report`, Then complete the following fields with the required information.

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

#### **Description of Fields:**

* **Is Enabled?** In order to activate this report configuration, you must define the Report Generation Start Date, specify the Period, set the Cut-off Time, and activate the 'Is Active' option. The report configuration will only become operational and functional once all of these criteria have been satisfied.
* **Is active?** Indicates whether this object is currently active and can be utilized and displayed on the website.
* **Period:** Specify how often the report generation process should recur.
* **Report generation date/time:** Set the date and time when the report generation process should start.
* **Cut off time:** The time at which a transaction is evaluated for inclusion in the next day's report, and also serves as the time for sending the report. If this time is not defined, the report will not be generated, even if the I**s\_active** flag is enabled.
* **Plugins:** Which plugin the report belongs to.
* **Send transaction report by email:** If this checkbox is checked, the report will be sent to the merchant's email address automatically.
* **Emails:** It is used to specify the recipient's email address where transaction reports and will be sent via email.
* **File name prefix:** The filename prefix can be used to specify the format in which reports are saved.

## [Flexible Interval Transaction Reports](transaction-report-configuration.md#flexible-interval-transaction-reports)

To access the Configuration of Transaction Report Generator, navigate to Ottu Dashboard > Administration Panel > Report > Flexible Interval Transaction Reports.

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Click on :heavy\_plus\_sign:`Add Flexible Interval Transaction Reports`, Then complete the following fields with the required information.

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

#### **Description of Fields:**

* **Is Enabled?** In order to activate this report configuration, you must define the Cut-off Time, and activate the 'Is Active' option. The report configuration will only become operational and functional once all of these criteria have been satisfied.
* **Is active?** Indicates whether this object is currently active and can be utilized and displayed on the website.
* **Cut off time:** The time at which a transaction is evaluated for inclusion in the next day's report, and also serves as the time for sending the report. If this time is not defined, the report will not be generated, even if the I**s\_active** flag is enabled.
* **Plugins:** Which plugin the report belongs to.
* **Send transaction report by email:** If this checkbox is checked, the report will be sent to the merchant's email address automatically.
* **Emails:** It is used to specify the recipient's email address where transaction reports and will be sent via email.
* **Send transaction report by sftp:** When this checkbox is marked, it enables the system to send transaction reports securely using the SFTP (Secure File Transfer Protocol) method.
* **SFTP server host address:** It is to specify the hostname or IP address of the SFTP server to which transaction reports and data will be securely transferred.
* **SFTP upload path:** This field designates the directory or location on the SFTP server where transaction reports and data will be uploaded.
* **SFTP server username:** This field requires the username associated with your SFTP server account.
* **SFTP server password:** In this field, input the confidential password associated with the SFTP server account.
* **File name prefix:** The filename prefix can be used to specify the format in which reports are saved.
* **Add breakdown columns:** When this checkbox is selected, it influences the exported reports. In this mode, columns like 'refunded\_amount' and 'voided\_amount' will be presented separately in the exported reports. This option allows for a more detailed representation of these amounts, enhancing the clarity and granularity of the data in the reports.

## [Report Fields](transaction-report-configuration.md#report-fields)

In this section, we will guide you through the process of incorporating extra fields into both [periodic](transaction-report-configuration.md#periodic-transaction-report-configuration) and [flexible](transaction-report-configuration.md#flexible-interval-transaction-reports) transaction reports. These fields will share the same categorization and description across both report types. The required fields are categorized based on the source type from which the data is extracted. The categories are as follows: [Config](transaction-report-configuration.md#config), [Static](transaction-report-configuration.md#static), [Gateway Response](transaction-report-configuration.md#gateway-response), and [Common](transaction-report-configuration.md#common), and each type requires different information.

To add new fields, follow these straightforward instructions:

1. Access the **Transaction Report Field** tab or **Periodic Transaction Report Field** tab.
2. Click on  `Add another transaction report fields`.
3. You will receive a dropdown list containing all the available field type.&#x20;

**Descriptive details for each field type, as well as the required information, are detailed below.**

***

#### [Config](transaction-report-configuration.md#config)

**Config fields:** Represent the values derived from configured items within a flexible, customizable dropdown list.

**Required Information:**

* **Field:** it is user-friendly, dropdown list allows for choosing the required Config fields to be displayed in the report. It offers the flexibility to add or remove built-in or custom fields, tailoring it to your specific needs.

***

#### [**Static**](transaction-report-configuration.md#static)

**Static fields:** Contain unchanging or predefined data elements that are included in the report. These fields do not vary with each transaction and often serve as constants or reference data. Static fields are essential for establishing a baseline or providing consistent information.

**Required Information:**

* **Static value:** Assign a value to the constant field. It will be the same for all generated reports.

***

#### [**Gateway response**](transaction-report-configuration.md#gateway-response)

**Gateway Response** **fields:** Include data obtained from interactions with [payment gateways](../payment-gateway.md). These fields contain information related to the response received from the payment gateway after processing a transaction. They can include details about the transaction's success, authorization, and other gateway-specific information.

**Required Information:**

* **Gateway response keys:** Since the PG (Payment Gateway) response is sent in a dictionary format, i.e., a set of key-value pairs {Key:Value}, so here you specify the keys of the values you need.

***

#### [**Common**](transaction-report-configuration.md#common)

**Common fields:** These are data elements that differentiate themselves from fields that originate from payment gateways (PG), static data, and plugin configurations. These fields serve as foundational components in transaction reports, offering essential information that remains consistent across all report types. Example of such element include “payment\_date.”

***

**Shared Required Information**

Besides the previously mentioned Required Information for each type, all types also require the completion of the following fields.

* **Is active:** If checked, the field will be used and displayed.
* **Label \[en]:** Create a custom label for the field in English, if required.
* **Label \[ar]:** Create a custom label for the field in Arabic, if required.
* **Name:** The name of the HTML field, which is used for backend validation. It will not be displayed anywhere.
* **Order:** The location of the field in the generated report.
