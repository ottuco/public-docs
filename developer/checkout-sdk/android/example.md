# Example

```swift
val theme = getCheckoutTheme()

// Builder class is used to construct an object passed to the SDK initializing function
val builder = Checkout
  .Builder(merchantId!!, sessionId, apiKey!!, amount!!)
    .paymentOptionsDisplaySettings(paymentOptionsDisplaySettings)
    .formsOfPayments(formsOfPayment)
    .theme(theme)
    .logger(Checkout.Logger.INFO)
    .build()


// Actual `init` function calling, returning a `Fragment` object  
checkoutFragment = Checkout.init(
    context = this@CheckoutSampleActivity,
    builder = builder,
    transactionResultCallback = object : Checkout.TransactionResultCallback {
        override fun onTransactionResult(result: TransactionResult) {
            showResultDialog(result)
        }
    }
)
```
