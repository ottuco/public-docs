# Customization Theme

The main class describing theme is called `CheckoutTheme`.

It uses additional component classes like:

* `ButtonComponent`
* `LabelComponent`
* `TextFieldComponent`

The `CheckoutTheme` class consists of objects that define various UI components. While the names of these components largely correspond to those listed [here](customization-theme.md#properties-description), they also include platform-specific fields for further customization.

{% embed url="https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG/Ottu-SDK---Components-Documentation?node-id=1-624&t=7reOLSB5zVdlOwz0-1" %}

## [Properties Description](customization-theme.md#properties-description) <a href="#properties-description" id="properties-description"></a>

All properties in the `CheckoutTheme` class are optional, allowing users to customize any of them as needed.

If a property is not specified, the default value (as defined in the Figma design [here](https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG?content-scaling=fixed\&kind=proto\&node-id=1-624\&scaling=scale-down)) will be automatically applied.

#### [**Texts**](customization-theme.md#texts)

#### **General**

| Property Name |                            Description                            |                        Data Type                        |
| ------------- | :---------------------------------------------------------------: | :-----------------------------------------------------: |
| `mainTitle`   |                 Font and color for all “Captions”                 | [LabelComponent](customization-theme.md#labelcomponent) |
| `title`       |           Font and color for payment options in the list          | [LabelComponent](customization-theme.md#labelcomponent) |
| `subtitle`    | Font and color for payment options details (like expiration date) | [LabelComponent](customization-theme.md#labelcomponent) |

#### **Fees**

| Property Name  |                           Description                          |                        Data Type                        |
| -------------- | :------------------------------------------------------------: | :-----------------------------------------------------: |
| `feesTitle`    |    Font and color of fees value in the payment options list    | [LabelComponent](customization-theme.md#labelcomponent) |
| `feesSubtitle` | Font and color of fees description in the payment options list | [LabelComponent](customization-theme.md#labelcomponent) |

#### **Data**

| Property Name |                        Description                       |                        Data Type                        |
| ------------- | :------------------------------------------------------: | :-----------------------------------------------------: |
| `dataLabel`   | Font and color of payment details fields (like “Amount”) | [LabelComponent](customization-theme.md#labelcomponent) |
| `dataValue`   |         Font and color of payment details values         | [LabelComponent](customization-theme.md#labelcomponent) |

**Other**

| Property Name                   |                           Description                          |                        Data Type                        |
| ------------------------------- | :------------------------------------------------------------: | :-----------------------------------------------------: |
| `errorMessageText`              |         Font and color of error message text in pop-ups        | [LabelComponent](customization-theme.md#labelcomponent) |
| `selectPaymentMethodTitleLabel` | The text of "Select Payment Method" in the bottom sheet header | [LabelComponent](customization-theme.md#labelcomponent) |

#### [**Text Fields**](customization-theme.md#text-fields)

| Property Name    |                              Description                             |                            Data Type                            |
| ---------------- | :------------------------------------------------------------------: | :-------------------------------------------------------------: |
| `inputTextField` | Font and color of text in any input field (including disabled state) | [TextFieldComponent](customization-theme.md#textfieldcomponent) |

#### [**Colors**](customization-theme.md#colors)

| Property Name                             |                            Description                            | Data Type |
| ----------------------------------------- | :---------------------------------------------------------------: | :-------: |
| `backgroundColor`                         |           The main background of the SDK view component           |  UIColor  |
| `backgroundColorModal`                    |                 The background of any modal window                |  UIColor  |
| `iconColor`                               |                The color of the icon of the payment               |  UIColor  |
| `paymentItemBackgroundColor`              |         The background of an item in payment options list         |  UIColor  |
| `selectPaymentMethodTitleBackgroundColor` | The background of the "Select Payment Method" bottom sheet header |  UIColor  |

#### [**Buttons**](customization-theme.md#buttons)

| Property Name    |                            Description                            |                         Data Type                         |
| ---------------- | :---------------------------------------------------------------: | :-------------------------------------------------------: |
| `button`         |           Background, text color and font for any button          | [ButtonComponent](customization-theme.md#buttoncomponent) |
| `selectorButton` | Background, text color and font for payment item selection button | [ButtonComponent](customization-theme.md#buttoncomponent) |

#### [**Switch**](customization-theme.md#switch)

| Property Name       |              Description             | Data Type |
| ------------------- | :----------------------------------: | :-------: |
| `switchOnTintColor` | The color of switch (toggle) control |  UIColor  |

#### [**Margins**](customization-theme.md#margins)

| Property Name |                     Description                     |                      Data Type                      |
| ------------- | :-------------------------------------------------: | :-------------------------------------------------: |
| margins       | Top, left, bottom and right margins between compone | [UIEdgeInsets](customization-theme.md#uiedgeinsets) |

#### [Payment Details](customization-theme.md#payment-details)

| Property Name        |                                            Description                                            | Data Type |
| -------------------- | :-----------------------------------------------------------------------------------------------: | :-------: |
| `showPaymentDetails` | Boolean variable determining whether the “Payment Details” section should be displayed or hidden. |  Boolean  |

## [Data Types Description](customization-theme.md#data-types-description) <a href="#data-types-description" id="data-types-description"></a>

#### [**LabelComponent**](customization-theme.md#labelcomponent)

| Property Name | Data Type  |
| ------------- | :--------: |
| `color`       |   UIColor  |
| `font`        |   UIFont   |
| `fontFamily`  |   String   |

#### [**TextFieldComponent**](customization-theme.md#textfieldcomponent)

| Property Name     |                                                                    Data Type                                                                    |
| ----------------- | :---------------------------------------------------------------------------------------------------------------------------------------------: |
| `label`           |                                             [LabelComponent](customization-theme.md#labelcomponent)                                             |
| `text`            | [LabelComponent](https://app.gitbook.com/o/RxY0H8C3fNw3knTb5iVs/s/XdPwy0yrnZJ8gfKCUk9E/~/changes/601/developer/checkout-sdk/ios#labelcomponent) |
| `backgroundColor` |                                                                     UIColor                                                                     |

#### [**ButtonComponent**](customization-theme.md#buttoncomponent)

| Property Name             | Data Type |
| ------------------------- | :-------: |
| `enabledTitleColor`       |  UIColor  |
| `disabledTitleColor`      |  UIColor  |
| `font`                    |   UIFont  |
| `enabledBackgroundColor`  |  UIColor  |
| `disabledBackgroundColor` |  UIColor  |
| `fontFamily`              |   String  |

#### [**UIEdgeInsets**](customization-theme.md#uiedgeinsets)

| Property Name | Data Type |
| ------------- | :-------: |
| `left`        |    Int    |
| `top`         |    Int    |
| `right`       |    Int    |
| `bottom`      |    Int    |

## [Example](customization-theme.md#example.1) <a href="#example.1" id="example.1"></a>

To configure the `theme`, similar steps must be followed as described in the test app file.

**Code Snippet:**

```swift
func createTheme() - > CheckoutTheme {
    var theme = CheckoutTheme()
    theme.backgroundColor = .systemBackground
    theme.backgroundColorModal = .secondarySystemBackground
    theme.margins = UIEdgeInsets(top: 8, left: 2, bottom: 8, right: 2)
    theme.mainTitle.color = .label
    theme.mainTitle.fontFamily = "Arial"
    theme.button.enabledTitleColor = .payButtonTitle
    theme.button.disabledTitleColor = .payButtonDisabledTitle
    theme.button.fontFamily = "Arial"
    theme.button.enabledBackgroundColor = .payButtonBackground
    theme.button.disabledBackgroundColor = .payButtonDisabledBackground
    return theme
}
```

The theme object is passed to the SDK initialization as shown below:

**Code Snippet:**

```swift
self.checkout = Checkout(
    theme: theme,
    sessionId: sessionId,
    merchantId: merchantId,
    apiKey: apiKey,
    delegate: self
)
```
