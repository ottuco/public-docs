# Payment Request

Effortlessly create payment requests with Ottu's user-friendly interface, minimizing form details for a streamlined experience. Rest assured with Ottu's security commitment and diverse payment gateway options.

## [**Payment Request Characteristics**](payment-request.md#payment-request-characteristics)

* Effortlessly [create payment requests ](../../#creating-payment-request)with Ottu's user-friendly interface.
* Experience a hassle-free process with minimal information required to fill out the payment request form.
* Rest assured with Ottu's unwavering commitment to security and confidentiality.
* Enjoy the flexibility of choosing from a range of [Payment Gateway options](../payment-gateway.md#payment-gateway-features-summary) provided by Ottu.
* Enjoy the convenience of a sharable link, specially generated for your transactions. The payment process will be seamlessly carried out through a sharable link created by trusted online payment gateways.
* Gain peace of mind as Ottu enables easy tracking and monitoring of your transactions. See [Payment Tracking](../payment-tracking/).

## [Payment Request Configuration](payment-request.md#payment-request-configuration)

Configuring the payment request plugin is straightforward and user-friendly, thanks to its seamless integration within the plugin configuration panel. Follow the steps outlined below to customize your settings efficiently:

1. **Access the Administration Panel**: Navigate to the Ottu Dashboard, click on the three dots to reveal a dropdown list, and select **Administration Panel**.
2. **Locate Payment Request Configuration**: Within the **Administration Panel**, find the **Payment Request Configuration** under the **Payment Request** section.
3. **Configuration Options**: Upon accessing the Payment Request Configuration, you'll have access to a range of options for customization:
   * #### **General**:
     * Define essential settings such as the Default Email, Redirect URL, Webhook URL, Default Currency, Transaction Expiration Time, Default Phone Number Country Code, and QR Code Type.
     * Configure optional settings including disabling the email input, disclosing the merchant URL, enabling payment amount editing, opting for an additional phone number, restricting multiple payments, and enabling individual proxy fields.
   * #### **Fee**:
     * Enable the option to charge additional fees, specifying the type and value of these fees.
   * #### **Security**:
     * Enhance your plugin's security by enabling API access and whitelisting IP addresses.
   * #### **Disclaimer**:
     * Decide whether to include a disclaimer, providing a template, if so.
   * #### **Receipt**:
     * Choose to add a receipt option, including its template.
   * #### **Terms and Conditions**:
     * Display terms and conditions, providing the content for them.
   * #### **Plugin**:
     * Customize further with options like Code Pairing, Notification, and Multi-Step Checkout.
   * #### **Fields**:
     * Add both built-in and custom fields to personalize the plugin further. Please check [next section](payment-request.md#plugin-fields-configuration) for more information.

By following these steps, merchants can tailor the payment request plugin to meet their specific needs, enhancing both security and user experience.

## [Plugin Fields Configuration ](payment-request.md#plugin-fields-configuration)

The Payment Request plugin offers the ability to add new fields and to customize already existing built-in fields.

Adding Custom (new) or Built-in fields can be done effortlessly by the following three steps:

1. **Go** **to** Ottu Dashboard > Administration Panel > Payment Request> Fields&#x20;
2. &#x20;**Click** on **Add another Field**&#x20;
3. **Complete** the information below and proceed to save.

*   #### **Type:**

    The type of the added field can be categorized as either **Custom** or **Builtin**.
*   #### **Itinerary Display:**

    If checked, the new added field will be shown in the “schedule table” or “planning table” (itinerary table) within the invoice PDF.
*   #### **Display Section:**&#x20;

    It determines the placement of the newly added field on the checkout page. \
    Available options include Order Description, From.
*   #### **Is active?:**&#x20;

    It allows for the control and management of the newly added field on a site or platform, providing the ability to enable or disable its usage and visibility.
*   #### **Required?:**&#x20;

    It determines whether a newly added field is mandatory or optional.
*   #### **Validator:**&#x20;

    It imposes constraints or rules on the new field's value. It ensures that the provided value meet certain requirements or conditions specified by the validator.
*   #### **Field:**&#x20;

    A drop-down list of predefined fields. It is presented when the **Builtin** type is selected in the [Type](payment-request.md#type) parameter.
*   #### **Order:**&#x20;

    It determines the display positioning of the fields within the same section.
*   #### **Placeholder \[en]:**&#x20;

    It provides guidance or an example of the expected input, helping users understand what information is required or the format it should take. (Should be In English).
*   #### **Placeholder \[ar]:**&#x20;

    It provides guidance or an example of the expected input, helping users understand what information is required or the format it should take. (Should be In Arabic).
* If the [Type](payment-request.md#type) is marked as **Custom**, the [Field](payment-request.md#field) parameter will disappear, and the following fields will be displayed for filling:
  *   #### **Name:**

      `HTML` field name utilized solely for backend validation. It will not be visible anywhere.
  *   #### **Label \[en]:**&#x20;

      Custom field's label in English.
  *   #### **Label \[ar]:**&#x20;

      Custom field's label in Arabic.

<figure><img src="../../.gitbook/assets/plugin fields.gif" alt=""><figcaption></figcaption></figure>
