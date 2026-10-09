> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Mailer

Laravel provides a clean email API powered by Symfony Mailer. It supports drivers for SMTP, Amazon SES, Postmark, Resend, sendmail, and local development options like Mailpit and the application log.

## Generating Mailables

```bash
# Standard view-based mailable
php artisan make:mail WelcomeUser

# Mailable with an auto-generated Markdown Blade template
php artisan make:mail OrderShipped --markdown=emails.orders.shipped
```

Mailable classes are placed in `app/Mail/`.

---

## Mailable Structure

Modern Laravel mailables use `Envelope`, `Content`, and `Attachment` objects:

```php
namespace App\Mail;

use App\Models\Order;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Mail\Mailables\Attachment;
use Illuminate\Mail\Mailables\Content;
use Illuminate\Mail\Mailables\Envelope;
use Illuminate\Queue\SerializesModels;

class OrderShipped extends Mailable implements ShouldQueue
{
    use Queueable, SerializesModels;

    public function __construct(
        public Order $order
    ) {}

    /**
     * Define the email envelope (subject, sender, tags).
     */
    public function envelope(): Envelope
    {
        return new Envelope(
            subject: 'Your Order #' . $this->order->id . ' Has Shipped',
        );
    }

    /**
     * Define the email message content / template.
     */
    public function content(): Content
    {
        return new Content(
            markdown: 'emails.orders.shipped',
            with: [
                'trackingUrl' => 'https://tracking.example.com/' . $this->order->tracking_code,
            ],
        );
    }

    /**
     * Attach files to the message.
     */
    public function attachments(): array
    {
        return [
            Attachment::fromStorage('invoices/' . $this->order->id . '.pdf')
                ->as('invoice.pdf')
                ->withMime('application/pdf'),
        ];
    }
}
```

---

## Markdown Email Templates

Markdown mailables use pre-styled Blade components (`resources/views/emails/orders/shipped.blade.php`):

```blade
<x-mail::message>
# Order Shipped

Hi {{ $order->user->name }},

Good news! Your order **#{{ $order->id }}** has been handed over to the carrier.

<x-mail::button :url="$trackingUrl">
Track Your Shipment
</x-mail::button>

<x-mail::panel>
Estimated delivery: 2-4 business days.
</x-mail::panel>

Thanks,<br>
{{ config('app.name') }}
</x-mail::message>
```

---

## Sending Mail

Use the `Mail` facade to send or queue messages:

```php
use App\Mail\OrderShipped;
use App\Models\Order;
use Illuminate\Support\Facades\Mail;

$order = Order::find(1);

// Send synchronously
Mail::to($order->user->email)->send(new OrderShipped($order));

// Queue for asynchronous delivery (recommended for fast HTTP responses)
Mail::to($order->user->email)->queue(new OrderShipped($order));

// Add CC, BCC, and multiple recipients
Mail::to($order->user)
    ->cc('billing@example.com')
    ->bcc('audit@example.com')
    ->queue(new OrderShipped($order));
```

> [!TIP]
> Implementing `ShouldQueue` on the `Mailable` class automatically queues the email when `Mail::to()->send()` is called, removing the need to remember `->queue()`.

---

## Previewing Mailables in the Browser

You can return a `Mailable` directly from a route to preview its rendered HTML in the browser without sending test emails:

```php
use App\Mail\OrderShipped;
use App\Models\Order;
use Illuminate\Support\Facades\Route;

Route::get('/mailable/preview', function () {
    $order = Order::first();

    return new OrderShipped($order);
});
```

---

## Configuration & Local Testing

Configured in `.env` and `config/mail.php`:

```dotenv
# Production (SMTP example)
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailgun.org
MAIL_PORT=587
MAIL_USERNAME=your-username
MAIL_PASSWORD=your-password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

# Local development: Write all emails to storage/logs/laravel.log
# MAIL_MAILER=log

# Local development: Mailpit (localhost:1025 for SMTP, localhost:8025 for web UI)
# MAIL_MAILER=smtp
# MAIL_HOST=127.0.0.1
# MAIL_PORT=1025
```
