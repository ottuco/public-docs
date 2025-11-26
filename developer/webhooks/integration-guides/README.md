# Integration Guides

Ottu uses [webhooks](../) to send real-time notifications to your application about important events such as payment updates, subscription changes, and more. To handle these notifications, you'll need to implement a webhook receiver – an endpoint within your application that Ottu can send data to.

## [Code Examples (Illustrative)](./#code-examples-illustrative)

The following examples provide a basic overview of how to structure webhook receivers in different languages. For production use, we strongly recommend using our official `SDKs`.

*   #### Laravel Example :&#x20;

    An illustrative guide for building webhook receivers in `Laravel`. View detailed [example](https://app.gitbook.com/s/XdPwy0yrnZJ8gfKCUk9E/developer/webhooks/integration-guides/laravel-webhook-receiver-guide).
*   #### .NET Example:&#x20;

    A demonstration of Ottu webhook handling within a `.NET` environment. View detailed [example](https://app.gitbook.com/s/XdPwy0yrnZJ8gfKCUk9E/developer/webhooks/integration-guides/laravel-webhook-receiver-guide).

## [Production-Ready SDKs](./#production-ready-sdks)

For streamlined development and optimal reliability, we highly recommend using our official Ottu `SDKs`:

* **Python (ottu-py):** This is our primary `SDK`, providing comprehensive functionality for Ottu integration. Access it on [GitHub](https://github.com/ottuco/ottu-py), where you can find detailed instructions in the README file.

{% hint style="warning" %}
Remember to implement proper signature verification mechanisms in your webhook receivers to ensure the authenticity of notifications from Ottu.<br>
{% endhint %}

**We're Here to Help!**

If you have questions or need further guidance, please refer to our comprehensive [Ottu documentation](../../../) or reach out to our [support team](mailto:support@ottu.com).
