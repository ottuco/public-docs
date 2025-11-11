# Installation

## [Minimum Requirements](installation.md#minimum-requirements) <a href="#minimum-requirements" id="minimum-requirements"></a>

The SDK is supported on devices running iOS 14 or higher.

### [**Installation with CocoaPods**](installation.md#installation-with-cocoapods)

Ottu is available via [CocoaPods](http://cocoapods.org/). To install it, the following line must be added to the Podfile:

{% code overflow="wrap" %}
```ruby
pod 'ottu_checkout_sdk', :git => 'https://github.com/ottuco/ottu-ios.git', :tag => '2.1.7'
```
{% endcode %}

{% hint style="info" %}
* When `ottu_checkout_sdk` is added to the **Podfile**, the **GitHub repository** must also be specified as follows:
* If CocoaPods returns an error like _"could not find compatible versions for pod"_, try running the `pod repo update` command to resolve it.
{% endhint %}

```ruby
pod 'ottu_checkout_sdk', :git => 'https://github.com/ottuco/ottu-ios'
```

### [**Installation with Swift Package Manager**](installation.md#installation-with-swift-package-manager)

The [Swift Package Manager](https://swift.org/package-manager/) (SPM) is a tool designed for automating the distribution of Swift code and is integrated into the `Swift` compiler.

Once the Swift package has been set up, adding Alamofire as a dependency requires simply including it in the `dependencies` value of the `Package.swift` file.

```swift
dependencies: [
    .package(url: "https://github.com/ottuco/ottu-ios.git", from: "2.1.7")
]
```
