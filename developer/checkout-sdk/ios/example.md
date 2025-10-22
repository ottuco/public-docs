# Example

There are both UIKit and SwiftUI samples available at the sample repo:

* UIKit: [<img src="https://github.com/fluidicon.png" alt="" data-size="line">ottu-ios/Example at main · ottuco/ottu-ios](https://github.com/ottuco/ottu-ios/tree/main/Example)
* SwiftUI: [<img src="https://github.com/fluidicon.png" alt="" data-size="line">ottu-ios/Example\_SwiftUI at main · ottuco/ottu-ios](https://github.com/ottuco/ottu-ios/tree/main/Example_SwiftUI)

The SDK initialization process and the callback delegate remain identical for both implementations.

**Code Sample:**

{% code overflow="wrap" %}
```swift
do {
  self.checkout =
    try Checkout(
      formsOfPayments: formsOfPayment,
      theme: theme,
      displaySettings: PaymentOptionsDisplaySettings(
        mode: paymentOptionsDisplayMode,
        visibleItemsCount: visibleItemsCount,
        defaultSelectedPgCode: defaultSelectedPgCode
      ),
      sessionId: sessionId,
      merchantId: merchantId,
      apiKey: apiKey,
      setupPreload: transactionDetailsPreload,
      delegate: self
    )
} catch
let error as LocalizedError {
  showFailAlert(error)
  return
} catch {
  print("Unexpected error: \(error)")
  return
}

if let paymentView = self.checkout?.paymentView() {
  paymentView.translatesAutoresizingMaskIntoConstraints = false
  self.paymentContainerView.addSubview(paymentView)

  NSLayoutConstraint.activate([
        paymentView.leadingAnchor.constraint(equalTo: self.paymentContainerView.leadingAnchor),
        self.paymentContainerView.trailingAnchor.constraint(equalTo: paymentView.trailingAnchor),
        paymentView.topAnchor.constraint(equalTo: self.paymentContainerView.topAnchor),
        self.paymentContainerView.bottomAnchor.constraint(equalTo: paymentView.bottomAnchor)
      ])
}
extension OttuPaymentsViewController: OttuDelegate {
  func errorCallback(_ data: [String: Any] ? ) {
    paymentContainerView.isHidden = true

    let alert = UIAlertController(title: "Error", message: data?.debugDescription ?? "", preferredStyle:
      .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
  }

  func cancelCallback(_ data: [String: Any] ? ) {
    var message = ""

    if let paymentGatewayInfo = data ? ["payment_gateway_info"] as ? [String: Any],
      let pgName = paymentGatewayInfo["pg_name"] as ? String,
        pgName == "kpay" {
          message = paymentGatewayInfo["pg_response"].debugDescription
        } else {
          message = data?.debugDescription ?? ""
        }

    paymentContainerView.isHidden = true

    let alert = UIAlertController(title: "Canсel", message: message, preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
  }

  func successCallback(_ data: [String: Any] ? ) {
    paymentContainerView.isHidden = true
    paymentSuccessfullLabel.isHidden = false

    let alert = UIAlertController(title: "Success", message: data?.debugDescription ?? "", preferredStyle:
      .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    present(alert, animated: true)
  }
}

```
{% endcode %}
