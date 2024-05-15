# Laravel Webhook Receiver Guide

This section is designed for developers who are using `Laravel`, a popular `PHP` framework, to integrate webhook notifications from Ottu. We aim to provide clear and practical examples to help you effectively set up and handle webhook events within your `Laravel` applications.&#x20;

## [Database Setup](laravel-webhook-receiver-guide.md#database-setup)

In this section, we focus on setting up the necessary database models and migrations required to handle and store webhook notifications, both [Payment Webhooks](../payment-notification.md) and [Operation Notification](../operation-notification.md),  and related data in a `Laravel` application. Our primary goal is to capture data associated with both the webhook notifications and the [Checkout API responses](../../checkout-api.md#response-parameters) efficiently.

## [**Models and Migrations**](laravel-webhook-receiver-guide.md#models-and-migrations)

We need two models: [Checkout](laravel-webhook-receiver-guide.md#the-checkout-model) and [Webhook](laravel-webhook-receiver-guide.md#the-webhook-model).&#x20;

*   #### The Checkout model:&#x20;

    It will store the details of the checkout process, including the [session ID](../../checkout-api.md#session\_id-string-mandatory) which uniquely identifies each payment transaction.&#x20;
*   #### The Webhook model:&#x20;

    It will be used to capture and store the data from webhook notifications.

### [**Creating the Checkout Model**](laravel-webhook-receiver-guide.md#creating-the-checkout-model)

The [Checkout model](laravel-webhook-receiver-guide.md#the-checkout-model) represents the data structure for storing information received from the [Checkout API response](../../checkout-api.md#response-parameters). Here is how you can set up this model and its corresponding migration:

#### Model Definition:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Factories\HasFactory;

class Checkout extends Model
{
    use HasFactory;

    protected $primaryKey = 'session_id';
    public $incrementing = false;
    protected $keyType = 'string';

    protected $fillable = [
        'session_id', 'type', 'payment_type', 'amount', 'currency_code',
        'state', 'customer_id', 'token', 'agreement', 'extra_params'
    ];

    protected $casts = [
        'agreement' => 'array',
        'extra_params' => 'array',
        'created_at' => 'datetime',
        'updated_at' => 'datetime',
    ];

    public function webhooks()
    {
        return $this->hasMany(Webhook::class);
    }
}
```

#### Migration for Checkout Table:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateCheckoutsTable extends Migration
{
    public function up()
    {
        Schema::create('checkouts', function (Blueprint $table) {
            $table->string('session_id', 100)->primary();
            $table->string('type', 250);
            $table->string('payment_type', 250);
            $table->string('amount', 20);
            $table->string('currency_code', 10);
            $table->string('state', 250);
            $table->string('customer_id', 250)->nullable();
            $table->string('token', 250)->nullable();
            $table->json('agreement')->nullable();
            $table->json('extra_params')->nullable();
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('checkouts');
    }
}
```

### [**Creating the Webhook Model**](laravel-webhook-receiver-guide.md#creating-the-webhook-model)

The [Webhook model](laravel-webhook-receiver-guide.md#the-webhook-model) is used to store each webhook notification, including the payload and the timestamp when it was received.

#### Model Definition:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Factories\HasFactory;

class Webhook extends Model
{
    use HasFactory;

    protected $fillable = [
        'session_id', 
        'checkout_id', 
        'payload'
    ];

    protected $casts = [
        'payload' => 'array',
        'timestamp' => 'datetime',
    ];

    public function checkout()
    {
        return $this->belongsTo(Checkout::class);
    }
}
```

#### Migration for Webhooks Table:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateWebhooksTable extends Migration
{
    public function up()
    {
        Schema::create('webhooks', function (Blueprint $table) {
            $table->id();
            $table->string('session_id', 250);
            $table->foreignId('checkout_id')
                  ->nullable()
                  ->constrained('checkouts')
                  ->onDelete('cascade');
            $table->json('payload')->nullable();
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('webhooks');
    }
}
```

## [Setting Up Webhook Controller in `Laravel`](laravel-webhook-receiver-guide.md#setting-up-webhook-controller-in-laravel)

A `Controller` will need to be created. Below can be generated using the `artisan command`:

`php artisan make:controller WebhookController`

Below is how the `WebhookController` could be structured, translating the functionality from the `Django` view:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Webhook;
use Illuminate\Support\Facades\Log;

class WebhookController extends Controller
{
    public function handle(Request $request)
    {
        Log::info('Webhook received: ' . $request->getContent());

        if (!$this->verifySignature($request)) {
            return response()->json(['detail' => 'Unable to verify signature'], 401);
        }

        try {
            $processedData = $this->processData($request);
            $this->saveData($processedData);
            return response()->json(['detail' => 'Success'], 200);
        } catch (\Exception $e) {
            Log::error('Failed to process webhook: ' . $e->getMessage());
            return response()->json(['detail' => 'Failed to process webhook'], 400);
        }
    }

    protected function verifySignature(Request $request)
    {
        // Assume `verify_signature` is a helper function you've set up as per the signing mechanism section:
        // https://docs.ottu.com/developer/webhooks/signing-mechanism#php
        return verify_signature($request->getContent(), $request->header('Signature'));
    }

    protected function processData(Request $request)
    {
        // Extract data from $request and perform any necessary transformations to process the data sent by Ottu
        return json_decode($request->getContent(), true);
    }

    protected function saveData(array $data)
    {
        // Use the data array to create or update your models
        Webhook::create([
            'session_id' => $data['session_id'],
            'checkout_id' => $data['checkout_id'] ?? null,
            'payload' => $data
        ]);
    }
}
```

#### **Key Points:**

1. **Signature Verification:**
   * Implement the `verify_signature` function to confirm the authenticity of incoming webhooks. This involves comparing a signature calculated from the request payload with the signature provided in the webhook headers. For detailed instructions on how to perform this verification, refer to our [Signing Mechanism](laravel-webhook-receiver-guide.md#signature-verification) section.
2. **Integrating with Laravel Routes:**
   * Define a route in `Laravel` that directs to this controller method by adding the following line to your routes file (typically located in `routes/web.php`):\
     `Route::post('/webhook', [WebhookController::class, 'handle']);`
3. **Logging:**
   * Log all incoming data from Ottu before processing. This is crucial for maintaining detailed records useful for troubleshooting and auditing purposes.
4. **Response Status Codes:**
   * Carefully manage the response status codes sent back to Ottu to ensure clear communication about the success or failure of the webhook handling.

#### Conclusion:

The example code provided above offers a foundational framework for developing your own webhook receiver in a Laravel environment. To enhance integration, it's important to adhere to the [Key Points](laravel-webhook-receiver-guide.md#key-points) outlined above and incorporate the following best practices:

* **Response Status Codes:** Exercise meticulous care in managing the response status codes returned to Ottu. This is critical for ensuring transparent communication regarding the success or failure of the webhook handling.
* **Endpoint Requirements:** Gain a thorough understanding of all endpoint requirements to ensure complete compliance with Ottu's operational standards. Detailed information on these requirements is available on our [Endpoint Requirements](../#endpoint-requirements) page.

These enhancements and best practices will optimize your webhook integration and ensure effective communication and compliance within the `Laravel` framework.
