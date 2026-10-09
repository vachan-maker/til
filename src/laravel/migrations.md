> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Database Migrations & Seeding

Migrations are version control for database schemas. They allow a team to define, modify, and share database structure definitions across environments using PHP.

## Generating Migrations

```bash
# Create a new table
php artisan make:migration create_posts_table

# Modify an existing table
php artisan make:migration add_status_to_posts_table --table=posts
```

## Migration Structure

Migrations live in `database/migrations/` and contain `up()` and `down()` methods:

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id')->constrained()->cascadeOnDelete();
            $table->string('title');
            $table->string('slug')->unique();
            $table->text('body');
            $table->boolean('is_published')->default(false);
            $table->timestamp('published_at')->nullable();
            $table->timestamps();
            $table->softDeletes();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('posts');
    }
};
```

---

## Common Column Types

| Column Definition | Database Equivalent |
| --- | --- |
| `$table->id();` | `BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY` |
| `$table->string('name', 255);` | `VARCHAR(255)` |
| `$table->text('content');` | `TEXT` |
| `$table->longText('payload');` | `LONGTEXT` |
| `$table->integer('votes');` | `INT` |
| `$table->unsignedBigInteger('account_id');` | `BIGINT UNSIGNED` |
| `$table->decimal('price', 8, 2);` | `DECIMAL(8, 2)` |
| `$table->boolean('is_active');` | `TINYINT(1)` / `BOOLEAN` |
| `$table->json('metadata');` | `JSON` |
| `$table->timestamp('activated_at');` | `TIMESTAMP` |
| `$table->timestamps();` | Adds `created_at` and `updated_at` |
| `$table->softDeletes();` | Adds `deleted_at` for soft delete support |

## Common Column Modifiers

```php
$table->string('email')->nullable();           // Allows NULL values
$table->string('role')->default('subscriber'); // Default value
$table->string('username')->unique();          // Unique index
$table->string('category')->index();           // Standard index
$table->string('subtitle')->after('title');    // Place column after existing one (MySQL)
```

## Foreign Key Constraints

Modern Laravel syntax uses concise foreign ID helpers:

```php
// Shorthand for user_id referencing id on users table with cascade delete
$table->foreignId('user_id')->constrained()->cascadeOnDelete();

// Nullable foreign key that sets null on delete
$table->foreignId('category_id')->nullable()->constrained()->nullOnDelete();

// Manual foreign key definition
$table->unsignedBigInteger('author_id');
$table->foreign('author_id')->references('id')->on('users')->onDelete('cascade');
```

---

## Modifying Existing Tables

When modifying an existing table, specify changes in `up()` and reverse them in `down()`:

```php
public function up(): void
{
    Schema::table('posts', function (Blueprint $table) {
        $table->string('subtitle')->nullable()->after('title');
        $table->renameColumn('body', 'content');
    });
}

public function down(): void
{
    Schema::table('posts', function (Blueprint $table) {
        $table->dropColumn('subtitle');
        $table->renameColumn('content', 'body');
    });
}
```

---

## Seeders & Model Factories

Seeders populate database tables with dummy or initial data (e.g. admin accounts, lookups).

### 1. Generating Seeders & Factories

```bash
php artisan make:seeder PostSeeder
php artisan make:factory PostFactory --model=Post
```

### 2. Defining a Factory (`database/factories/PostFactory.php`)

```php
namespace Database\Factories;

use App\Models\User;
use Illuminate\Database\Eloquent\Factories\Factory;

class PostFactory extends Factory
{
    public function definition(): array
    {
        return [
            'user_id' => User::factory(),
            'title' => fake()->sentence(),
            'slug' => fake()->unique()->slug(),
            'body' => fake()->paragraphs(3, true),
            'is_published' => fake()->boolean(80),
        ];
    }
}
```

### 3. Calling Seeders in `database/seeders/DatabaseSeeder.php`

```php
namespace Database\Seeders;

use App\Models\User;
use App\Models\Post;
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run(): void
    {
        // Create an explicit admin user
        User::factory()->create([
            'name' => 'Admin User',
            'email' => 'admin@example.com',
        ]);

        // Generate 20 dummy posts with related users
        Post::factory(20)->create();

        // Or invoke specific seeder classes
        $this->call([
            RoleSeeder::class,
        ]);
    }
}
```

---

## Running Migrations & Seeders

```bash
# Run pending migrations
php artisan migrate

# Run database seeders
php artisan db:seed

# Run a specific seeder class
php artisan db:seed --class=PostSeeder

# Roll back the last batch of migrations
php artisan migrate:rollback

# Drop all tables, re-run all migrations, and run seeders
php artisan migrate:fresh --seed
```

> [!TIP]
> `php artisan migrate:fresh --seed` is the most common command during local development to reset the database to a clean, populated state.

> [!CAUTION]
> Avoid `migrate:fresh` in production environments as it completely wipes existing tables and records.
