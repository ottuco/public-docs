# Customization Theme

The class responsible for defining the theme is called `CheckoutTheme`. It utilizes additional component classes, including:

* `ButtonComponent`
* `LabelComponent`
* `TextFieldComponent`

The `CheckoutTheme` class consists of objects representing various UI components. While the component names generally align with those listed above, they also contain platform-specific fields for further customization.

{% embed url="https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG?content-scaling=fixed&fuid=819188572429138268&kind=proto&node-id=1-624&scaling=scale-down" %}

Below is the structure of the Customization `Theme` class:

```swift
class CheckoutTheme extends Equatable {
  @_UiModeJsonConverter()
  final CustomerUiMode uiMode;
  
  final TextStyle? mainTitleText;
  final TextStyle? titleText;
  final TextStyle? subtitleText;
  final TextStyle? feesTitleText;
  final TextStyle? feesSubtitleText;
  final TextStyle? dataLabelText;
  final TextStyle? dataValueText;
  final TextStyle? errorMessageText;

  final TextFieldStyle? inputTextField;

  final ButtonComponent? button;
  final RippleColor? backButton;
  final ButtonComponent? selectorButton;
  final SwitchComponent? switchControl;
  final Margins? margins;

  final ColorState? sdkBackgroundColor;
  final ColorState? modalBackgroundColor;
  final ColorState? paymentItemBackgroundColor;
  final ColorState? selectorIconColor;
  final ColorState? savePhoneNumberIconColor;
}

```

#### [**uiMode**](customization-theme.md#uimode)

Specifies the device `Theme` mode, which can be set to one of the following:

* `light` – Forces the UI to use light mode.
* `dark` – Forces the UI to use dark mode.
* `auto` – Adapts the UI based on the device's system settings.

{% hint style="info" %}
The `uiMode` parameter only affects Flutter Android and not Flutter iOS, as the flutter follows the behavior of the native implementation
{% endhint %}

## [Properties Description](customization-theme.md#properties-description) <a href="#properties-description" id="properties-description"></a>

All properties in the `CheckoutTheme` class are optional, allowing users to customize any of them as needed.

If a property is not set, the default value (as specified in the Figma design [here](https://www.figma.com/proto/BmLOTN8QCvMaaIteZflzgG?content-scaling=fixed\&kind=proto\&node-id=1-624\&scaling=scale-down)) will be applied automatically.

#### [**Texts**](customization-theme.md#texts)

#### **General**

| Property Name   |                            Description                            |              Data Type              |
| --------------- | :---------------------------------------------------------------: | :---------------------------------: |
| `mainTitleText` |                 Font and color for all “Captions”                 | [Text](customization-theme.md#text) |
| `titleText`     |           Font and color for payment options in the list          | [Text](customization-theme.md#text) |
| `subtitleText`  | Font and color for payment options details (like expiration date) | [Text](customization-theme.md#text) |

#### **Fees**

| Property Name      |                           Description                          |              Data Type              |
| ------------------ | :------------------------------------------------------------: | :---------------------------------: |
| `feesTitleText`    |    Font and color of fees value in the payment options list    | [Text](customization-theme.md#text) |
| `feesSubtitleText` | Font and color of fees description in the payment options list | [Text](customization-theme.md#text) |

#### **Data**

| Property Name   |                        Description                       |              Data Type              |
| --------------- | :------------------------------------------------------: | :---------------------------------: |
| `dataLabelText` | Font and color of payment details fields (like “Amount”) | [Text](customization-theme.md#text) |
| `dataValueText` |         Font and color of payment details values         | [Text](customization-theme.md#text) |

**Other**

| Property Name                   |                           Description                          |              Data Type              |
| ------------------------------- | :------------------------------------------------------------: | :---------------------------------: |
| `errorMessageText`              |         Font and color of error message text in pop-ups        | [Text](customization-theme.md#text) |
| `selectPaymentMethodHeaderText` | The text of "Select Payment Method" in the bottom sheet header | [Text](customization-theme.md#text) |

#### [**Text Fields**](customization-theme.md#text-fields)

| Property Name    |                              Description                             |                   Data Type                   |
| ---------------- | :------------------------------------------------------------------: | :-------------------------------------------: |
| `inputTextField` | Font and color of text in any input field (including disabled state) | [TextField](customization-theme.md#textfield) |

#### [**Colors**](customization-theme.md#colors)

| Property Name                              |                       Description                      |               Data Type               |
| ------------------------------------------ | :----------------------------------------------------: | :-----------------------------------: |
| `sdkbackgroundColor`                       |      The main background of the SDK view component     | [Color](customization-theme.md#color) |
| `modalBackgroundColor`                     |           The background of any modal window           | [Color](customization-theme.md#color) |
| `paymentItemBackgroundColor`               |    The background of an item in payment options list   | [Color](customization-theme.md#color) |
| `selectorIconColor`                        |          The color of the icon of the payment          | [Color](customization-theme.md#color) |
| `savePhoneNumberIconColor`                 | The color of “Diskette” button for saving phone number | [Color](customization-theme.md#color) |
| `selectPaymentMethodHeaderBackgroundColor` |    The background of an item in payment options list   | [Color](customization-theme.md#color) |

#### [**Buttons**](customization-theme.md#buttons)

| Property Name    |                            Description                            |                     Data Type                     |
| ---------------- | :---------------------------------------------------------------: | :-----------------------------------------------: |
| `button`         |           Background, text color and font for any button          |      [Button](customization-theme.md#button)      |
| `backButton`     |               Color of the “Back” navigation button               | [RippleColor](customization-theme.md#ripplecolor) |
| `selectorButton` | Background, text color and font for payment item selection button |      [Button](customization-theme.md#button)      |

#### [**Switch**](customization-theme.md#switch)

| Property Name |                                        Description                                        |                 Data Type                 |
| ------------- | :---------------------------------------------------------------------------------------: | :---------------------------------------: |
| `switch`      | Colors of the switch background and its toggle in different states (on, off and disabled) | [Switch](customization-theme.md#switch-1) |

#### [**Margins**](customization-theme.md#margins)



| Property Name |                      Description                      |                Data Type                |
| ------------- | :---------------------------------------------------: | :-------------------------------------: |
| margins       | Top, left, bottom and right margins between component | [Margin](customization-theme.md#margin) |

## [Data Types Description](customization-theme.md#data-types-description) <a href="#data-types-description" id="data-types-description"></a>

#### [**Color**](customization-theme.md#color)

| Property Name   |             Description             | Data Type |
| --------------- | :---------------------------------: | :-------: |
| `color`         |       Main color integer value      |    Int    |
| `colorDisabled` | Disabled stated color integer value |    Int    |

#### [**RippleColor**](customization-theme.md#ripplecolor)

| Property Name  |             Description             | Data Type |
| -------------- | :---------------------------------: | :-------: |
| `color`        |       Main color integer value      |    Int    |
| `rippleColor`  |      Ripple color integer value     |    Int    |
| `colorDisaled` | Disabled stated color integer value |    Int    |

#### [**Text**](customization-theme.md#text)

| Property Name |        Description       |               Data Type               |
| ------------- | :----------------------: | :-----------------------------------: |
| `textColor`   | Main color integer value | [Color](customization-theme.md#color) |
| `fontType`    |     Font resource ID     |                  Int                  |

#### [**TextField**](customization-theme.md#textfield)

|  Property Name | Description                    |               Data Type               |
| :------------: | ------------------------------ | :-----------------------------------: |
|  `background`  | Background color integer value | [Color](customization-theme.md#color) |
| `primaryColor` | Text color                     | [Color](customization-theme.md#color) |
| `focusedColor` | Selected text color            | [Color](customization-theme.md#color) |
|     `text`     | Text value                     |  [Text](customization-theme.md#text)  |
|     `error`    | Text value                     |  [Text](customization-theme.md#text)  |

#### [Button](customization-theme.md#button)

| Property Name |       Description       |                     Data Type                     |
| ------------- | :---------------------: | :-----------------------------------------------: |
| `rippleColor` | Button background color | [RippleColor](customization-theme.md#ripplecolor) |
| `fontType`    |   Button text font ID   |                        Int                        |
| `textColor`   |    Button text color    |       [Color](customization-theme.md#color)       |

#### [Switch](customization-theme.md#switch-1)

| Property Name                   |             Description             | Data Type |
| ------------------------------- | :---------------------------------: | :-------: |
| `checkedThumbTintColor`         |    Toggle color in checked state    |    Int    |
| `uncheckedThumbTintColor`       |   Toggle color in unchecked state   |    Int    |
| `checkedTrackTintColor`         |     Track color in checked state    |    Int    |
| `uncheckedTrackTintColor`       |    Track color in unchecked state   |    Int    |
| `checkedTrackDecorationColor`   |  Decoration color in checked state  |    Int    |
| `uncheckedTrackDecorationColor` | Decoration color in unchecked state |    Int    |

#### [**Margin**](customization-theme.md#margin)

| Property Name | Data Type |
| ------------- | :-------: |
| `left`        |    Int    |
| `top`         |    Int    |
| `right`       |    Int    |
| `bottom`      |    Int    |

## [Example](customization-theme.md#example.1) <a href="#example.1" id="example.1"></a>

To build the `theme`, the user must follow similar steps as described in the corresponding file of the test app.

Here is a **code snippet** demonstrating the process:

```swift
final checkoutTheme = ch.CheckoutTheme(
  uiMode: ch.CustomerUiMode.dark,
  titleText: ch.TextStyle(),
  modalBackgroundColor: ch.ColorState(color: Colors.amber));
```
