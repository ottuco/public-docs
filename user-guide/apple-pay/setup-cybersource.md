# Setup Cybersource

## [Integration Process for Apple Pay and Cybersource](setup-cybersource.md#integration-process-for-apple-pay-and-cybersource)

Merchants can offer their customers a convenient and secure payment method when making purchases using Apple devices by integrating Apple Pay with Cybersource. Merchants can easily set up and configure their Cybersource account to accept Apple Pay transactions by following the steps outlined below.

#### [**Prerequisites**](setup-cybersource.md#prerequisites)

* Valid Cybersource account
* Apple Developer account

### [**Generating a Cybersource RESTful Key**](setup-cybersource.md#generating-a-cybersource-restful-key)

1. Log in to your Cybersource account.
2. Navigate to the **Payment Configuration**.
3. Click on **Key Management**.
4. Click on the **+Generate Key** button to initiate the process.

<figure><img src="https://lh3.googleusercontent.com/PWiVlDT5pkJLiHNvrO4OJPxEzLrw9ONs7iuMOXOKpr9LNPJgCzfwbpLwsytMNDTbwVNtlNGIugDfDUF4wOVSCWUKPpRdJNWX6MQ5vFrtQO660FleSePZLE0krEL4W4pZqkQ0Jsr3hTmrmxjz4vvtb6A" alt=""><figcaption></figcaption></figure>

5. On the configuration page, select the **REST - Shared Secret** option.
6. Click on the **Generate Key** button to generate the required key for the Integration process.

<figure><img src="https://lh6.googleusercontent.com/LXFKWzCLAmvkA4PADZSWyaqxcA3zQcFTBS6aavY9TMxUXJKE3ZQ6yhUbUrfM8461OzViX5CRPE8lMa9PCAzgzp_MxWUAaH3vPRMMX8D99VoMbSc8gryW5qapKMnW3MEdIxiBD86VDziW61VnD7RC4sU" alt=""><figcaption></figcaption></figure>

7. The **REST API Keys** (i.e., **Key** and **Shared Secret**) have been generated successfully.

<figure><img src="https://lh5.googleusercontent.com/6ZHphttaddlCZDQCSxGjdugqeCFBZ6P_C37oFVYiOLowcpMvVurJGXAsj5d6Ve7JuGfhRKfD0mX_zvarO3HzkeTadg-QaohN8iv16EYcTMpMeykD6j-cdocaycDN08KZPrwTLx5nVGZRZCsaPo-vEXI" alt=""><figcaption></figcaption></figure>

### [**Providing the RESTful Keys to Ottu**](setup-cybersource.md#providing-the-restful-keys-to-ottu)

1. Log in to the **Ottu Dashboard**.
2. Navigate to **Administration Panel**.
3. Locate **Gateway** tab
4. Go to **Settings**.
5. Select the **Cybersource PG Setting** associated with Apple Pay services.
6. **Provide** the two generated [keys](setup-cybersource.md#generating-a-cybersource-restful-key) in the appropriate fields.

<figure><img src="https://lh5.googleusercontent.com/_9xpATTwn_rna7O4JeG1HCvFkGJ69tuQisicBM0iEXP3ICh9_F4BldZwdIib_bl8nbJMB4t82ukKvVEkZhvsCL4hq3KYQzClsk0fWViymQYnMX0r8SrscD-QFhN40iV6v6rNFf-CZ6ETazYLkpROoJI" alt=""><figcaption></figcaption></figure>

### [**Generating a Certificate Signing Request**](setup-cybersource.md#generating-a-certificate-signing-request)

1. Log in to the **Cybersource portal dashboard**.
2. Access **Digital Payment Solutions** under **Payment Configuration**.
3. Click on the **Configure** button.

<figure><img src="https://lh5.googleusercontent.com/47GpHtPOIx6sIuuV0I-vt-18EVBGhP20zElpS4fmx8LIM983lHXdjwjiniGcqXVZrI_zJdffOLwVY2piVyRlDvJH8OdIJf-kMBIg0BMnnD1XmWjhfDMv9ddQu_4eNCnhtfUTTdWakAZeovRMRLwCVhE" alt=""><figcaption></figcaption></figure>

4. Provide the [Apple Merchant ID](https://hazems-organization-new.gitbook.io/demo/user-guide/apple-pay#creating-merchant-id) that is associated with your Apple Pay and click on the **Generate new certificate signing request** button.

<figure><img src="https://lh6.googleusercontent.com/BSvhDyGVYB9WYYDKT9M4yqVLHst0B-zmV-4ouDMtwVT05aH7oQ5y_DbggHYOWPf0MA73xJ9M2eMV2ywzSRpCcQEv3IVdhNMjz41vq8PSi3SqKK_MjVcAvgWRL1tyHIDJqOtZRGFWXckwXmeqniMxOg8" alt=""><figcaption></figcaption></figure>

5. **Download** the generated certificate.

<figure><img src="https://lh4.googleusercontent.com/2Ka1RR0YxpnJ1fvNvO9-Yda1Ka72PNkKADODRDRHN6G8fokfLl8fCb6GJ7715M0cMjWYxuZQQmttuZoY_3noReHbVX4k5bwx9IO1gRNtxUIxWNIznXsTiTqPtXiTIJ1qfzMGgZVynr6NnU7uJ3K1Mzc" alt=""><figcaption></figcaption></figure>

### [**Download** the **Apple Pay Payment Processing Certificate**](setup-cybersource.md#download-the-apple-pay-payment-processing-certificate)

#### [**Uploading the Certificate Signing Request to Apple Pay**](setup-cybersource.md#uploading-the-certificate-signing-request-to-apple-pay)

1. Log in to your **Apple Pay developer account**.
2. Go to the [Merchant ID](../apple-pay.md#creating-merchant-id) that you created and **Create Certificate** under **Apple Pay Payment Processing Certificate**.

<figure><img src="https://lh4.googleusercontent.com/t4XvGJEL1USsMOctPDMRFwgdUgonEOXtNg14FBTVWJ3ywOJlBgdC3cp1716FB4DFNtnPCO70s4S4ehV1JDNYPjdNYELZvVnoNFxcNxS6B6asBNOTu86UkV4nar1N9x0-VmPb8nxLwd4XcqVQfdGc6Ng" alt=""><figcaption></figcaption></figure>

3. On the next page, select **No** and **Continue**.

<figure><img src="https://lh3.googleusercontent.com/Y8WKsp7kf_QPSNPozgGRCaxlVw_gLJIiOmIntAtKQHZ_vYumJQmEnq1P3uB-qHiKX_UKOjcip3U5cRXVNcQeA5KV4Bh4YBRN0CTbYppLcVHeMxDP4UXfRFLPrzFrrUzBEk3UqynPWYbz_XFrEo0v_0Q" alt=""><figcaption></figcaption></figure>

4. **Upload** the [Certificate Signing Request](setup-cybersource.md#generating-a-certificate-signing-request) that you generated from Cybersource in the previous step and click **Continue**.

<figure><img src="https://lh3.googleusercontent.com/YJ-gu3u4Q92K7H_aQlsygkLUmxJnetG55q7C8N91pPlKJof23yl65JUlWXac7Bw5fVavXRcuoKgBTrp0VrIGrta9j4Fvl9O1MLwMs9ltTKr8uIolvaNpU41lt1Rr6-9M_oRL3iq6UQ9tOzSnYRO8USI" alt=""><figcaption></figcaption></figure>

5. **Download** the **Apple Pay Payment Processing Certificate** file.

<figure><img src="https://lh4.googleusercontent.com/RgOCKmbNH_StKhHKUepR82xeSaQnjUpnxY8tRj7cbmK4kug_4Oa7FpxfSypdy8lDDutzPkI-8vmJe0B5Mf-gQz8fTTBVebqXZQVKQDylo1eNLeT4slK0o4JhRRtySk4BcyLBK5uCh0YLA5ui0T7xoBw" alt=""><figcaption></figcaption></figure>

\
