> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Laravel

Laravel is an expressive, elegant PHP web application framework utilizing the MVC (Model-View-Controller) architectural pattern.

## Topic Index

| Topic | Description |
| --- | --- |
| [Artisan Commands](artisan-commands.md) | Daily CLI reference for generators, database, routing, and cache optimization |
| [Migrations & Seeding](migrations.md) | Database schema version control, column modifiers, seeders, and factories |
| [Form Requests & Validation](form-requests.md) | Dedicated request validation classes, common rules, and controller integration |
| [Mailer](mailer.md) | Mailables, envelopes, content, attachments, and email queueing |
| [Task Scheduling](scheduler.md) | Managing scheduled cron tasks in code, execution frequencies, and local workers |
| [Authorization & Policies](policies.md) | Resource authorization, Gates vs Policies, and Blade/Controller checks |

## Standard Directory Structure

| Path | Purpose |
| --- | --- |
| `app/Models/` | Eloquent ORM models |
| `app/Http/Controllers/` | HTTP controllers handling incoming requests |
| `app/Http/Requests/` | Form Request validation and authorization classes |
| `app/Policies/` | Authorization policies mapped to models |
| `app/Mail/` | Mailable classes for email generation |
| `database/migrations/` | Schema definition files executed in order |
| `database/seeders/` | Database seeders for test and initial data |
| `database/factories/` | Model factories for generating fake test data |
| `routes/web.php` | Stateful web routes (cookies, session, CSRF) |
| `routes/api.php` | Stateless API routes with token/sanctum authentication |
| `routes/console.php` | Artisan console commands and scheduled tasks (Laravel 11+) |
| `resources/views/` | Blade templates |
