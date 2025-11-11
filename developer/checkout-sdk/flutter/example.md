# Example

{% code overflow="wrap" %}
```swift
final displaySettings = PaymentOptionsDisplaySettings(
mode: state.paymentOptionsDisplayMode ?? PaymentOptionsDisplayMode.BOTTOM_SHEET,
visibleItemsCount: paymentsListItemCount,
defaultSelectedPgCode: state.defaultSelectedPayment,
);
final args = CheckoutArguments(
merchantId: state.merchantId,
apiKey: state.apiKey,
sessionId: state.sessionId ?? "",
amount: amount,
showPaymentDetails: state.showPaymentDetails,
paymentOptionsDisplaySettings: displaySettings,
setupPreload: state.preloadPayload == true ? _apiTransactionDetails : null,
formsOfPayment: formOfPayments?.isNotEmpty == true ? formOfPayments : null,
theme: _theme,
);
//
Scaffold(
appBar: AppBar(
backgroundColor: Theme.of(context).colorScheme.inversePrimary,
title: Text(widget.title),
),
body: SingleChildScrollView(
child: Column(crossAxisAlignment: CrossAxisAlignment.stretch, children: [
SizedBox(height: 46),
Text(
"Customer Application",
textAlign: TextAlign.center,
style: ts.TextStyle(fontSize: 24),
),
//Start of Merchant content
const Padding(
padding: EdgeInsets.all(12.0),
child: Text(
"Some users UI elements, Some users UI elements, Some users UI elements, Some users UI elements, Some users UI elements")),
//End of Merchant content
Padding(
padding: const EdgeInsets.all(12.0),
child: ValueListenableBuilder < int > (
builder: (BuildContext context, int height, Widget ? child) {
return SizedBox(
height: height.toDouble(),
child: OttuCheckoutWidget(arguments: widget.checkoutArguments),
);
},
valueListenable: _checkoutHeight,
),
),
const SizedBox(height: 20),
//Start of Merchant content
const Padding(
padding: EdgeInsets.all(12.0),
child: Text(
"Some users UI elements, Some users UI elements, Some users UI elements, Some users UI elements, Some users UI elements,"
" Some users UI elements, Some users UI elements, Some users UI elements,"
" Some users UI elements, Some users UI elements, Some users UI elements")),
//End of Merchant content
])),
);
```
{% endcode %}
