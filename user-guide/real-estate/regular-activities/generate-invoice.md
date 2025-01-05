# Generate Invoice

In the Real Estate plugin, invoices are automatically created and prepared for all rental payments associated with each contract. These payments occur based on the [payment frequency](tenant-and-contract-management/contract-management/add-new-contract.md#payment-period) defined within the [contract form](tenant-and-contract-management/contract-management/add-new-contract.md#new-contract-form). Through the **Generate Invoice** section, merchants have the ability to explore all prepared invoices. They have the option to specify the desired [property](tenant-and-contract-management/contract-management/add-new-contract.md#property-name), [profile,](property-management.md#invoice-profile) or period to view all invoices prepared thus far.

{% hint style="info" %}
&#x20;Merchants can only choose the period for previewing the invoice within the range of the plugin activation date till the current month.
{% endhint %}

### [Generate Invoices Dashboard](generate-invoice.md#generate-invoices-dashboard)

The **Generate Invoices Dashboard** helps merchants easily manage rental invoices with tools for viewing, printing, and sharing payment links.

#### **Key Features:**

* **Automatic Invoices:** Invoices are generated based on the payment frequency set in each contract.
* **Filter and Display:** View invoices by selecting **Property**, **Profile**, or **Time Period** (from the plugin activation date to the current month).
* **Quick Preview:** Click the **SHOW** button to display all relevant invoices.
* **Print Invoices:** Easily print invoices as professional PDF files using the **Print** button.
* **Send Payment Links:** Share payment links directly with customers using the **Send Payment Link** button.
* **Apply Discounts:** Discounts are automatically calculated based on unit rates and free periods between the contract start and payment start dates.
* **Invoice Details:** Each invoice includes property info, unit details, contract data, tenant information, and financial summaries.

This dashboard simplifies invoice management, making it faster and more efficient.

## [Display Invoices](generate-invoice.md#display-invoices)

This example demonstrates how to display, generate, and send invoice links. The displayed invoices can be filtered by property name, profile, or date. In this case, we specify the period as January 2025 and the property as **Doc Building** and **Doc Building and Villa**.

When the **SHOW** button is clicked, all relevant invoices from the activation date of the Real Estate plugin up to the selected month are displayed, as shown in the figure below.

<figure><img src="../../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

## [Add Discount ](generate-invoice.md#add-discount)

Each invoice will be displayed with information such as [property](tenant-and-contract-management/contract-management/add-new-contract.md#property-name), [unit information](tenant-and-contract-management/contract-management/add-new-contract.md#add-unit), [contract number](tenant-and-contract-management/contract-management/add-new-contract.md#contract-reference), contract [start](tenant-and-contract-management/contract-management/add-new-contract.md#contract-start-date) and [end](tenant-and-contract-management/contract-management/add-new-contract.md#contract-end-date) dates, [due](tenant-and-contract-management/contract-management/add-new-contract.md#payment-start-date) date, [termination details](tenant-and-contract-management/contract-management/contract-action/terminate-contract.md), and [tenant information](tenant-and-contract-management/tenant-management.md), along with the following amount information:

<figure><img src="../../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

Merchant can easily update the collected amount through the **Actual PAID Amt** field.

## [Print Invoices](generate-invoice.md#print-invoices)

Printing invoices is quick and straightforward. After [displaying](generate-invoice.md#display-invoices) the relevant invoices, simply click on the **Print** button. The system will automatically format and generate a printable PDF version of the selected invoices.

## [Generate Invoices and Send Payment Links](generate-invoice.md#generate-invoices-and-send-payment-links)

After clicking the **SHOW** button, check [here](generate-invoice.md#display-invoices), the **Generate Invoices and Send Links** buttons become active. Clicking this button will:

* Create the invoices.
* Send the payment link to the tenant.
