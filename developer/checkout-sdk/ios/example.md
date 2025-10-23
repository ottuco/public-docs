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

if let paymentVC = self.checkout?.paymentViewController(),
  let paymentView = paymentVC.view {

    self.addChild(paymentVC)

    let resizableContainer = ResizableContainerView()
    resizableContainer.translatesAutoresizingMaskIntoConstraints = false
    resizableContainer.addSubview(paymentView)
    paymentVC.didMove(toParent: self)

    paymentView.translatesAutoresizingMaskIntoConstraints = false
    NSLayoutConstraint.activate([
    paymentView.topAnchor.constraint(equalTo: resizableContainer.topAnchor),
    paymentView.bottomAnchor.constraint(equalTo: resizableContainer.bottomAnchor),
    paymentView.leadingAnchor.constraint(equalTo: resizableContainer
        .leadingAnchor, constant: 16),
    paymentView.trailingAnchor.constraint(equalTo: resizableContainer
        .trailingAnchor, constant: -16)
  ])

    contentView.addSubview(resizableContainer)
    NSLayoutConstraint.activate([
    resizableContainer.topAnchor.constraint(equalTo: topLabel.bottomAnchor,
        constant: 16),
    resizableContainer.leadingAnchor.constraint(equalTo: contentView
        .leadingAnchor),
    resizableContainer.trailingAnchor.constraint(equalTo: contentView
        .trailingAnchor)
  ])

    resizableContainer.sizeChangedCallback = {
      [weak self] _ in
      guard
      let self
      else {
        return
      }
      updateScrollEnabled(
        for: scrollView)
    }

    let bottomLabel = UILabel()
    bottomLabel.text = "Some user UI elements"
    bottomLabel.translatesAutoresizingMaskIntoConstraints = false

    contentView.addSubview(bottomLabel)
    NSLayoutConstraint.activate([
    bottomLabel.topAnchor.constraint(equalTo: resizableContainer.bottomAnchor,
        constant: 16),
    bottomLabel.centerXAnchor.constraint(equalTo: contentView.centerXAnchor),
    bottomLabel.bottomAnchor.constraint(equalTo: contentView.bottomAnchor,
        constant: -16)
  ])
  }

// outside `viewDidLoad`
extension OttuPaymentsViewController: OttuDelegate {
  func errorCallback(_ data: [String: Any] ? ) {
    paymentContainerView.isHidden = true

    let alert = UIAlertController(title: "Error", message: data
      ?.debugDescription ?? "", preferredStyle:
      .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
  }

  func cancelCallback(_ data: [String: Any] ? ) {
    var message = ""

    if let paymentGatewayInfo = data ? ["payment_gateway_info"] as ? [
        String: Any],
      let pgName = paymentGatewayInfo["pg_name"] as ? String,
        pgName == "kpay" {
          message = paymentGatewayInfo["pg_response"].debugDescription
        } else {
          message = data?.debugDescription ?? ""
        }

    paymentContainerView.isHidden = true

    let alert = UIAlertController(title: "Canсel", message: message,
      preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    self.present(alert, animated: true)
  }

  func successCallback(_ data: [String: Any] ? ) {
    paymentContainerView.isHidden = true
    paymentSuccessfullLabel.isHidden = false

    let alert = UIAlertController(title: "Success", message: data
      ?.debugDescription ?? "", preferredStyle:
      .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .cancel))
    present(alert, animated: true)
  }
}

```
{% endcode %}
