# Callbacks

The callbacks are handled by the native frameworks. Please see the links here:

* [Android Callbacks](../android/#callbacks)
* [iOS Callbacks](../ios/#callbacks)

It is not necessary to modify anything for the callbacks, as they are managed by the native SDK.

However, the following examples demonstrate how they function on both platforms.

It is needed to add a section like below in a `*.dart` file of the app:

```swift
@override
void didChangeDependencies() {
  super.didChangeDependencies();
  _methodChannel.setMethodCallHandler((call) async {
    switch (call.method) {
      case _methodPaymentSuccessResult:
      {
        String message = call.arguments as String;
        logger.d("didChangeDependencies, success: $message");
        _checkoutMessage.value = ("Success", message);
      }

      case _methodPaymentCancelResult:
      {
        String message = call.arguments as String;
        logger.d("didChangeDependencies, cancel: $message");
        _checkoutMessage.value = ("Cancel", message);
      }

      case _methodPaymentErrorResult:
      {
        String message = call.arguments as String;
        logger.d("didChangeDependencies, error: $message");
        _checkoutMessage.value = ("Error", message);
      }
    }
  });
}
```

where `_methodPaymentSuccessResult`, `_methodPaymentCancelResult` and `_methodPaymentErrorResult` are defined as constants:

```swift
const _methodPaymentSuccessResult = "METHOD_PAYMENT_SUCCESS_RESULT";
const _methodPaymentErrorResult = "METHOD_PAYMENT_ERROR_RESULT";
const _methodPaymentCancelResult = "METHOD_PAYMENT_CANCEL_RESULT";
```

Also, on the Flutter side, you need to define a `MethodChannel` to listen for events from the native platforms.

```swift
const _methodChannel = MethodChannel('com.ottu.sample/checkout');
```

### [Android](callbacks.md#android-2)

The `callbacks` are defined within the body of the `Checkout.init` function:

```swift
val sdkFragment = Checkout.init(
  context = checkoutView.context,
  builder = builder,
  setupPreload = apiTransactionDetails,
  successCallback = {
    Log.e("TAG", "successCallback: $it")
    showResultDialog(checkoutView.context, it)
  },
  cancelCallback = {
    Log.e("TAG", "cancelCallback: $it")
    showResultDialog(checkoutView.context, it)
  },
  errorCallback = { errorData, throwable ->
    Log.e("TAG", "errorCallback: $errorData")
    showResultDialog(checkoutView.context, errorData, throwable)
  },

```

### [iOS](callbacks.md#ios.1) <a href="#ios.1" id="ios.1"></a>

Here is an example of a delegate:

{% code overflow="wrap" %}
```swift
extension CheckoutPlatformView: OttuDelegate {
    
    public func errorCallback(_ data: [String: Any]?) {
        debugPrint("errorCallback\n")
        DispatchQueue.main.async {
            self.paymentViewController?.view.isHidden = true
            self.paymentViewController?.view.setNeedsLayout()
            self.paymentViewController?.view.layoutIfNeeded()
            self._view.heightHandlerView.setNeedsLayout()
            self._view.heightHandlerView.layoutIfNeeded()
            self._view.setNeedsLayout()
            self._view.layoutIfNeeded()
            
            let alert = UIAlertController(
                title: "Error",
                message: data?.debugDescription ?? "",
                preferredStyle: .alert
            )
            alert.addAction(
                UIAlertAction(title: "OK", style: .cancel)
            )
            debugPrint("errorCallback, show alert\n")
            self.paymentViewController?.present(alert, animated: true)
        }
    }
    
    public func cancelCallback(_ data: [String: Any]?) {
        debugPrint("cancelCallback\n")
        DispatchQueue.main.async {
            var message = ""
            
            if let paymentGatewayInfo = data?["payment_gateway_info"] as? [String: Any],
               let pgName = paymentGatewayInfo["pg_name"] as? String,
               pgName == "kpay" {
                message = paymentGatewayInfo["pg_response"].debugDescription
            } else {
                message = data?.debugDescription ?? ""
            }
            
            self.paymentViewController?.view.isHidden = true
            self.paymentViewController?.view.setNeedsLayout()
            self.paymentViewController?.view.layoutIfNeeded()
            self._view.heightHandlerView.setNeedsLayout()
            self._view.heightHandlerView.layoutIfNeeded()
            self._view.setNeedsLayout()
            self._view.layoutIfNeeded()
            
            let alert = UIAlertController(
                title: "Cancel",
                message: message,
                preferredStyle: .alert
            )
            alert.addAction(
                UIAlertAction(title: "OK", style: .cancel)
            )
            debugPrint("cancelCallback, show alert\n")
            self.paymentViewController?.present(alert, animated: true)
        }
    }
    
    public func successCallback(_ data: [String: Any]?) {
        debugPrint("successCallback\n")
        DispatchQueue.main.async {
            self.paymentViewController?.view.isHidden = true
            self._view.paymentSuccessfullLabel.isHidden = false
            self.paymentViewController?.view.setNeedsLayout()
            self.paymentViewController?.view.layoutIfNeeded()
            self._view.heightHandlerView.setNeedsLayout()
            self._view.heightHandlerView.layoutIfNeeded()
            self._view.setNeedsLayout()
            self._view.layoutIfNeeded()
            
            let alert = UIAlertController(
                title: "Success",
                message: data?.debugDescription ?? "",
                preferredStyle: .alert
            )
            alert.addAction(
                UIAlertAction(title: "OK", style: .cancel)
            )
            debugPrint("successCallback, showing alert\n")
            self.paymentViewController?.present(alert, animated: true)
        }
    }
}

```
{% endcode %}
