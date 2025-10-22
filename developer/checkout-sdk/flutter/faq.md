# FAQ

#### :digit\_one:[**What forms of payment are supported by the SDK?**](faq.md#what-forms-of-payment-are-supported-by-the-sdk)

The SDK supports the following [forms of payment](faq.md#formsofpayment-array-optional):

* `tokenPay`
* `redirect`
* `StcPay` &#x20;
* `cardOnsite`
* `applePay` _(iOS only)_

Merchants can configure the forms of payment displayed according to their needs.

For example, to **display only the STC Pay button**, use:

```
formsOfPayment = ["stcPay"]
```

This ensures that only the **STC Pay** button is shown. The same approach applies to other payment methods.

#### :digit\_two:[**What are the minimum system requirements for SDK integration?**](faq.md#what-are-the-minimum-system-requirements-for-sdk-integration)

The SDK requires a device running:

* **Android 8** or higher (**API level 26** or higher)
* **iOS 14** or higher

#### :digit\_three:[**Can I customize the appearance beyond the provided themes?**](faq.md#can-i-customize-the-appearance-beyond-the-provided-themes)

Yes, customization is supported. For more details, refer to the [Customization Theme](faq.md#customization-theme) section.
