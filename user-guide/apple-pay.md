# Apple Pay

## <mark style="color:blue;"></mark>[Introduction](apple-pay.md#introduction)

Generally, payment services refer to two main services: mobile payment and digital wallets.

## <mark style="color:blue;"></mark>[Ottu payment services](apple-pay.md#ottu-payment-services)

Payment services are like different channels on top of bank integration that speed up charging customers.\
Currently, Ottu enables Apple Pay payment services, but there can be more and will add soon Samsung Pay.

## [Apple Pay](apple-pay.md#apple-pay)

Ottu supports Apple Pay in KSA and Bahrain.

![](<../.gitbook/assets/Apple Pay.png>)

## [Apple Pay configuration](apple-pay.md#apple-pay-configuration)

### [Apple Pay setup](apple-pay.md#apple-pay-setup)

### [Creating merchant ID](apple-pay.md#creating-merchant-id)

Merchant needs to have a developer account in Apple.

#### ****:digit\_one: **** [**Login to the account**](apple-pay.md#login-to-the-account)****

#### ****:digit\_two:****[**Go to “certificates, Identifiers & Profile” section** ](apple-pay.md#go-to-certificates-identifiers-and-profile-section.)

![](../.gitbook/assets/creating-merchant-id.png)

#### ****:digit\_three:****[**Go to Identifier and Click on + (new Identifier)**](apple-pay.md#go-to-identifier-and-click-on-+-new-identifier)****

![](../.gitbook/assets/new-identifier.png)

#### ****:digit\_four:****[**Register a new Merchant ID- enter the installation URL and description**](apple-pay.md#register-a-new-merchant-id-enter-the-installation-url-and-description)****

![](<../.gitbook/assets/merchant-ids (1).png>)

#### ****:digit\_five:****[**Go inside the identifier and add the domain under Merchant domain section**](apple-pay.md#go-inside-the-identifier-and-add-the-domain-under-merchant-domain-section)****

&#x20;**Enter the** installation. **** Merchant[**.althawaq.com** ](http://merchant.althawaq.com)**and save.**&#x20;

![](../.gitbook/assets/5-register-merchant-id.png)

#### ****:digit\_six:****[**Click on register**](apple-pay.md#click-on-register)&#x20;

![](<../.gitbook/assets/6-clickon-reg (1).png>)

### <mark style="color:blue;"></mark>[Adding and verifying a domain](apple-pay.md#adding-and-verifying-a-domain)

#### ****:digit\_seven:****[**Click on Althawaqh** Apple Pay](apple-pay.md#click-on-althawaqh-apple-pay)

![](../.gitbook/assets/7-clickon-althawaqh.png)

#### ****:digit\_eight:****[**Add merchant domain**](apple-pay.md#add-merchant-domain)****

![](../.gitbook/assets/8-add-merchant-domain.png)

#### &#x20; :digit\_nine:****[**Enter the domain you want to register**](apple-pay.md#enter-the-domain-you-want-to-register)****

![](../.gitbook/assets/9-domain-reg.png)

#### ****:digit\_one:****:digit\_zero:****[**Download the text file and provide it to Ottu**](apple-pay.md#download-the-text-file-and-provide-it-to-ottu)****

![](../.gitbook/assets/10-download-file.png)

#### ****:digit\_one:****:digit\_one:****[**Ottu will update and configure the file in the installation backend**](apple-pay.md#ottu-will-update-and-configure-the-file-in-the-installation-backend.)****

#### :digit\_one::digit\_two:[Domain verification](apple-pay.md#domain-verification)

**After Ottu configures the file, click on verify under the merchant domain section for the verification of the domain. Ottu will confirm on this**

![](../.gitbook/assets/12-verfication-domain.png)

#### &#x20;:digit\_one:****:digit\_three:****[**The domain is verified**](apple-pay.md#the-domain-is-verified)****

### [Creating Apple Pay certificates](apple-pay.md#creating-apple-pay-certificates)

#### ****:digit\_one:****:digit\_four:****[**Go again to Certificates, Identifiers & profiles. Scroll down**](apple-pay.md#go-again-to-certificates-identifiers-and-profiles.-scroll-down)****

![](../.gitbook/assets/14-certifcate.png)

#### &#x20;:digit\_one::digit\_five: [**Go to Apple Pay Merchant Identity Certificate and click on “create certificate”**](apple-pay.md#go-to-apple-pay-merchant-identity-certificate-and-click-on-create-certificate)****

![](../.gitbook/assets/15-create-cert.png)

#### ****:digit\_one::digit\_six: [**Ottu will provide CSR certificate**](apple-pay.md#ottu-will-provide-csr-certificate)****

#### ****:digit\_one:****:digit\_seven:****[**Upload the CSR certificate that Ottu will provide under create certificate**](apple-pay.md#upload-the-csr-certificate-that-ottu-will-provide-under-create-certificate)****

![](<../.gitbook/assets/17-get-certfile (1).png>)

****:digit\_one:****:digit\_eight:**Click on Continue and then click download to get your .**CER **file**

![](../.gitbook/assets/18-download-certfile.png)

#### ****:digit\_one:****:digit\_nine:****[**Download the Certificate**](apple-pay.md#download-the-certificate) &#x20;

![](../.gitbook/assets/19-download-file.png)

#### ****:digit\_two:****:digit\_zero:****[**Provide the certificate (.cer) file to Ottu**](apple-pay.md#provide-the-certificate-.cer-file-to-ottu)****

## [Creating apple payment processing certificate:](apple-pay.md#creating-apple-payment-processing-certificate)

#### ****:digit\_two:****:digit\_one:****[**Login to MPGS portal, then go to Device Payments**](apple-pay.md#login-to-mpgs-portal-then-go-to-device-payments)****

![](../.gitbook/assets/22-device-payment.png)

#### :digit\_two:****:digit\_two:****[**Click on Add new certificate**](apple-pay.md#click-on-add-new-certificate)****

#### ****:digit\_two:****:digit\_three:**Download the file- “Apple Pay Certificate Signing Request” file**

![](../.gitbook/assets/24-cert-signingreq-file.png)

&#x20;:digit\_two:****:digit\_four:**Create a certificate**

**Login to Apple Pay developer account, go to Merchant ID that was created and create a certificate under Apple Pay Payment processing certificate.**

![](../.gitbook/assets/25-configure-merchantid.png)

&#x20;**26. Click No on this page and continue.**

![](../.gitbook/assets/26-click-no.png)

&#x20;**27. Upload the CSR file obtained from MPGS portal.**

![](../.gitbook/assets/27-uplpoad-csrfile.png)

**28. Download the file**, **which is the Apple Pay Payment processing certificate file.** &#x20;

![](../.gitbook/assets/28-download-processingfile.png)

**29. Upload the file in MPGS portal.**

![](../.gitbook/assets/29-upload-mpgsfile.png)
