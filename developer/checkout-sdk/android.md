# Android

## [Ottu Checkout](android.md#ottu-checkout)

Over The Ottu Checkout which is Android SDK, helps you to make your payment process easy and quick within your Android app, in addition it provides UI screen and elements  customizable empowering you to collect payment details of your user.

{% hint style="warning" %}
For optimal security, call REST APIs from server-side implementations, not client-side applications such as mobile apps or web browsers.
{% endhint %}

## [**Features**](android.md#features)

**Simplified security:** Sensitive data will be collected easily according to PCI-compliant, by sending it directly to Ottu instead of sending it to your server.

**Native UI:** Ottu offers native screens and elements for collecting payment details.

<figure><img src="../../.gitbook/assets/PayoutScreen (1).png" alt=""><figcaption></figcaption></figure>

**Localized:** Both English, Arabic localizations are supported.

### [**Privacy**](android.md#privacy)

The Ottu Checkout SDK collects data to help in improving the products and services and prevent fraud. This data is never used for advertising and is not rented, sold, or given to advertisers.

### [**Requirements**](android.md#requirements)

IDE is required to develop an android app. SDK is compatible with minimum SDK 22 and above.

## [**Getting started**](android.md#getting-started)

Firstly, a session token should be created  by Ottu public API, then SDK could be initialized. See [Rest API](../rest-api/).\


{% hint style="info" %}
For "Api\_Key" API [Public key](../rest-api/authentication.md#public-key) sohuld be used.
{% endhint %}

## [Installation](android.md#installation)

### [**Installation with dependency**](android.md#installation-with-dependency)

Put below dependency into your Gradle.

```java
allprojects {
  repositories {
	...
	maven { url 'https://jitpack.io' }
  }
}
    
dependencies {
       implementation 'com.github.ottuco:ottu-android-private-sdk:1.0.10'
}
```

### [Sample code](android.md#sample-code)

Below is the sample code of how you can use Ottu Payment SDK.

```java
	
	
  Ottu ottuPaymentSdk = new Ottu(MainActivity.this);
                        ottuPaymentSdk.setApiId("Api_Key"); // set Api Key which is get from Ottu merchant account
                        ottuPaymentSdk.setMerchantId("Merchant_Id");
                        ottuPaymentSdk.setSessionId("Session_id"); // Retrive from public API
                        ottuPaymentSdk.setAmount("100.00"); // String Value
                        ottuPaymentSdk.setLocal("en"); // en or ar
                        ottuPaymentSdk.build();
	

```

### [Get payment result in on Activity Result method in Activity.](android.md#get-payment-result-in-on-activity-result-method-in-activity.)

```java
	  @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK ){
            if (requestCode == OttuPaymentResult ){
                SocketRespo paymentResult = (SocketRespo) data.getSerializableExtra("paymentResult");
		String Status = paymentResult.getStatus();
                if (Status.equals("success")){
                    	// handle success result
                	textView.setText(paymentResult.status);   
	        	textView.setText(paymentResult.message);
	        	textView.setText(paymentResult.order_no);
	        	textView.setText(paymentResult.operation);
                }else if (Status.equals("failed")){
                    	// handle failed result
                } else if (Status.equals("cancel")){
			// handle cancel result
		}
            }

        }
    }
	
```

if you are using fragments you will need to add broadcastReseiver in OnActivityResult to add your success-failure logic in your fragment.

```java
PAYMENT_SUCCESS = "paymentSuccess" // add in your constant accordingly
 @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK ){
            if (requestCode == OttuPaymentResult ){
                SocketRespo paymentResult = (SocketRespo) data.getSerializableExtra("paymentResult");
                textView.setText(paymentResult.status);   // success || failed || cancel
		String Status = paymentResult.getStatus();
                if (Status.equals("success")){
                    	// handle success result
 			Intent  intent = Intent(PAYMENT_SUCCESS)
               		sendBroadcast(intent)
                }else if (Status.equals("failed")){
                    	// handle failed result
                } else if (Status.equals("cancel")){
                   	// handle cancle result
                }
            }
        }
    }

// register your broadcast
activity.registerReceiver(
                paymentReceiver,
                IntentFilter(PAYMENT_SUCCESS)
)
	
// then you will receive it in your fragment with your action PAYMENT_SUCCESS
BroadcastReceiver paymentReceiver = new BroadcastReceiver() {
           @Override
            public void onReceive(Context context, Intent intent) {
               if(intent.getAction().equals(PAYMENT_SUCCESS)){
                  // add your code 
              }
            }
};
```

## [**ProGuard**](android.md#proguard)

You may need to include the following lines in your progard-rules.pro file if enable progard or minifyEnble.\


```java
-keep class Ottu** { *; }
```

