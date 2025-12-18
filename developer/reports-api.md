# Reports API

The Reports-API lets merchants access their completed transaction reports programmatically.\
It’s designed for reconciliation, accounting, analytics, and compliance — without exposing any public storage links.

Two secure APIs are provided:

1. [**List Reports**](reports-api.md#id-1-list-reports) : Returns a filtered, paginated list of finished reports (auto or manual), plus a secure `download_action`.
2. [**Download Report** ](reports-api.md#id-2-download-report): Downloads the binary file using authenticated access.

**Why this matters:**\
The dashboard already shows reports, but this API enables automated systems (ERP, BI tools, finance scripts) to fetch and download reports safely.

## [Quick Start](reports-api.md#quick-start)

This quick start shows how to authenticate, list available reports, and securely download a report using the Reports API in just a few steps.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% stepper %}
{% step %}
#### Authenticate

Choose one authentication method:

*   **API Key (recommended)**\
    Add your private API key to the request header:

    ```
    Api-Key: your_private_api_key
    ```
* **Basic Auth (user-based)**\
  Use Basic Auth credentials. The user must have the `report.can_view_report` permission (or be a superuser).
{% endstep %}

{% step %}
#### List Available Reports

Call the List Reports API to retrieve finished reports.

**Request**

```bash
curl -X GET "https://sandbox.ottu.net/api/v1/reports?limit=10" \
  -H "Api-Key: your_private_api_key"
```

**What you get**

* A list of completed reports only
* Each report includes metadata and a secure `download_action.url`
{% endstep %}

{% step %}
#### Select a Report

From the response, choose the report you want to download and copy its `encrypted_id` or `download_action.url`.

Example:

```json
"download_action": {
  "method": "GET",
  "url": "https://<ottu-url>/b/api/v1/reports/files/{report_id}/download/"
}
```
{% endstep %}

{% step %}
#### Download the Report

Use the provided `download_action.url` to download the file.

**Request**

```bash
curl -X GET "https://<ottu-url>/b/api/v1/reports/files/{report_id}/download/" \
  -H "Api-Key: your_private_api_key"
```

**Result**

* The report file is returned as binary (CSV, XLSX, etc.)
* The download action is securely authenticated and audit-logged
{% endstep %}

{% step %}
#### Handle Common Scenarios

* **No reports found**:\
  The API returns `200 OK` with an empty `results` array.
* **Missing permission (Basic Auth)**:\
  The API returns `403 Forbidden`.
* **Invalid credentials**:\
  The API returns `401 Unauthorized`.
{% endstep %}
{% endstepper %}

## [Setup](reports-api.md#setup)

#### Base URL

Use your Ottu domain:

```
https://<ottu-url>
```

#### Endpoints

#### **1) List Reports**

```
GET b/api/v1/reports
```

#### **2) Download Report**

&#x20;From the response of [List Reports API](reports-api.md#id-1-list-reports), extract `download_action.url` for the desired report

make GET request to `download_action.url` to download the file

{% hint style="info" %}
If you don’t pass date filters, the List Reports API returns **the last 30 days** of finished reports, sorted newest first.
{% endhint %}

## [Authentication](reports-api.md#authentication)

**Supported Methods**

* [Private API Key](authentication.md#private-key-api-key)
* [Basic Authentication](authentication.md#basic-authentication)

For detailed information on authentication procedures, please refer to the [Authentication documentation](reports-api.md#authentication).

## [Permissions](reports-api.md#permissions)

Permissions apply only when using [Basic Auth](authentication.md#basic-authentication).

#### Required permission

* `report.can_view_report`

#### Rules

* If the user has the permission or is a superuser, the [List](reports-api.md#id-1-list-reports) and [Download ](reports-api.md#id-2-download-report)APIs work normally.
* If the user **does not have the permission**, the API responds with:
  * **403 Forbidden**
  * Message like: `"No permission assigned"`

{% hint style="success" %}
API Key authentication does **not** require user permissions, since it is merchant-scoped.
{% endhint %}

## [How it works](reports-api.md#how-it-works)

#### Reports sources

Reports can be generated in two ways:

* **Auto reports**: daily / weekly / monthly / yearly schedules.
* **Manual reports**: created via dashboard.

#### Visibility and security rules

* A merchant can only see **their own reports**.
* Only **finished reports** are returned.
* **No public or raw storage URLs are ever returned.**
* Every download attempt is **audit-logged**.

#### Download workflow (high-level)

1. Call **List Reports** to get available reports.
2. Pick a report from `results`.
3. Use its secure `download_action.url`.
4. Call that URL with the same authentication headers.
5. The report file is returned as binary.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## [API Schema Reference](reports-api.md#api-schema-reference)

{% openapi-operation spec="reports-api" path="/b/api/v1/reports/files/" method="get" %}
[OpenAPI reports-api](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/69a580417f576cfa51abdc5f795e1d93ccfc60fa4cd1b7d874968684449d1628.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20251218%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20251218T084905Z&X-Amz-Expires=172800&X-Amz-Signature=b9a142a081abfacc24886503b27f9fbb0e3bad6ee4073bef4ae1dfc5e67d3bb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="reports-api" path="/b/api/v1/reports/files/{token}/download/" method="get" %}
[OpenAPI reports-api](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/69a580417f576cfa51abdc5f795e1d93ccfc60fa4cd1b7d874968684449d1628.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20251218%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20251218T084905Z&X-Amz-Expires=172800&X-Amz-Signature=b9a142a081abfacc24886503b27f9fbb0e3bad6ee4073bef4ae1dfc5e67d3bb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

## [Guide](reports-api.md#guide)

#### 1) List Reports

**Purpose:**\
Retrieve a paginated list of completed reports.

**Request**

```
GET b/api/v1/reports/files/
```

**Query parameters**

|       Name       | Type | Required |                     Notes                       |
| :--------------: | :--: | :------: | ----------------------------------------------- |
|  `created_after` | date |    No    | Lower bound (inclusive)                         |
| `created_before` | date |    No    | Upper bound (inclusive)                         |
|    `interval`    | enum |    No    | `daily`, `weekly`, `monthly`, `yearly`          |
|     `source`     | enum |    No    | `auto` or `manual`                              |
|      `limit`     |  int |    No    | Default 50, max 200                             |
|     `offset`     |  int |    No    | Pagination mode (cursor recommended if enabled) |

**Response**&#x20;

* `download_action.url` – pre-signed download URL with embedded token.
* `download_action.method` – always `GET`.

#### 2) Download Report

**Purpose**\
Download the report file as binary.

**Request**

```
GET b/api/v1/reports/files/a75cd20e-d5b7-4019-972e-c0fe45c1bb96/download/
```

{% hint style="info" %}
`:report_id` is file download token&#x20;
{% endhint %}

**Response**

* Returns the file directly (CSV, XLSX, etc.) as binary.
* No JSON body unless there’s an error.

## [Best Practices](reports-api.md#best-practices)

1. **Use API Key for automation**
   * More stable and permission-free for system integrations.
2. **Always filter by date**
   * Avoid pulling large histories unintentionally.
   * Example: fetch only last month’s reports.
3. **Respect pagination**
   * Use `limit` and `cursor/offset` until `next` is null.
4. **Handle empty results**
   *   A valid response can be:

       ```json
       {"count":0, "next":null, "previous":null, "results":[]}
       ```
5. **Log your own download tracking**
   * Even though Ottu logs downloads, your system should store:
     * report id
     * download time
     * success state
6. **Retry safely**
   * On `429 rate_limited`, backoff before retrying.

## [FAQ](reports-api.md#faq)

#### :digit\_one:[**Can I download reports without listing them first?**](reports-api.md#can-i-download-reports-without-listing-them-first)

You technically can if you already stored the `encrypted_id`.\
But listing first is the safest way to discover available reports.

#### :digit\_two:[**Why don’t I see reports that are still generating?**](reports-api.md#why-dont-i-see-reports-that-are-still-generating)

The List Reports API returns **finished reports only** to keep results correct and fast.

#### :digit\_three:[**What happens if I don’t pass date filters?**](reports-api.md#what-happens-if-i-dont-pass-date-filters)

The API returns reports from the **last 30 days**, newest first.

#### :digit\_four:[**Can I share download links publicly?**](reports-api.md#can-i-share-download-links-publicly)

No. Download URLs are secure, tokenized, and require authentication.\
They are not permanent or public.

#### :digit\_five:[**What errors should I expect?**](reports-api.md#what-errors-should-i-expect)

| HTTP | code                 | message                  | When                               |
| ---- | -------------------- | ------------------------ | ---------------------------------- |
| 400  | `invalid_parameters` | Bad filters              | `created_before` < `created_after` |
| 401  | `unauthorized`       | Invalid/missing auth     | Header missing or wrong            |
| 403  | `forbidden`          | Insufficient permissions | Basic user lacks report permission |
| 404  | `not_found`          | Report not found         | Wrong or inaccessible ID           |
| 429  | `rate_limited`       | Too many requests        | Quota exceeded                     |

#### :digit\_six: [**I’m using Basic Auth but still getting 403 — why?**](reports-api.md#im-using-basic-auth-but-still-getting-403-why)

Your user is missing `report.can_view_report`.\
Ask your admin to enable report API access.

#### :digit\_seven: [**Are downloads logged even if they fail?**](reports-api.md#are-downloads-logged-even-if-they-fail)

Yes. Every attempt is logged with outcome status for audit compliance.
