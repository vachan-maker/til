> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Common Artisan Commands

Artisan is the command-line interface included with Laravel. It provides commands for generating boilerplate, running database migrations, managing caches, and inspecting application state.

## Make / Generator Commands

### Creating Models

| Command | Description |
| --- | --- |
| `php artisan make:model Post` | Creates a basic Eloquent model |
| `php artisan make:model Post -m` | Model + migration |
| `php artisan make:model Post -c` | Model + controller |
| `php artisan make:model Post -cr` | Model + resource controller |
| `php artisan make:model Post -mcr` | Model + migration + resource controller |
| `php artisan make:model Post -s` | Model + seeder |
| `php artisan make:model Post -f` | Model + factory |
| `php artisan make:model Post -a` | Model + migration + factory + seeder + policy + resource controller + form requests (`--all`) |
| `php artisan make:model RoleUser --pivot` | Creates a custom pivot model |

### Creating Controllers

| Command | Description |
| --- | --- |
| `php artisan make:controller PostController` | Basic empty controller |
| `php artisan make:controller PostController --resource` | Controller with CRUD actions (`index`, `create`, `store`, `show`, `edit`, `update`, `destroy`) |
| `php artisan make:controller PostController --resource --model=Post` | Resource controller with typed model route-binding |
| `php artisan make:controller Api/PostController --api` | API resource controller (omits HTML `create` and `edit` methods) |
| `php artisan make:controller ProvisionServer --invokable` | Single action controller with `__invoke()` |

### Creating Other Core Classes

| Command | Description |
| --- | --- |
| `php artisan make:migration create_posts_table` | Generates a new migration file |
| `php artisan make:request StorePostRequest` | Generates a Form Request validation class |
| `php artisan make:policy PostPolicy --model=Post` | Generates a Policy class mapped to a model |
| `php artisan make:mail WelcomeUser` | Generates a Mailable class |
| `php artisan make:mail OrderShipped --markdown=emails.orders.shipped` | Mailable with a Markdown Blade template |
| `php artisan make:seeder PostSeeder` | Generates a database seeder |
| `php artisan make:factory PostFactory --model=Post` | Generates a model factory |
| `php artisan make:middleware EnsureTokenIsValid` | Generates custom HTTP middleware |
| `php artisan make:job ProcessPodcast` | Generates a queueable job |

---

## Database Migrations & Seeding

| Command | Description |
| --- | --- |
| `php artisan migrate` | Run pending migrations |
| `php artisan migrate:status` | Show migration execution status and batches |
| `php artisan migrate:rollback` | Rollback the last migration batch |
| `php artisan migrate:rollback --step=2` | Rollback the last 2 migrations |
| `php artisan migrate:reset` | Rollback all database migrations |
| `php artisan migrate:refresh` | Rollback all migrations and run them again |
| `php artisan migrate:refresh --seed` | Rollback all, re-run, and seed database |
| `php artisan migrate:fresh` | Drop all tables and execute all migrations from scratch |
| `php artisan migrate:fresh --seed` | Drop all tables, run all migrations, and run seeders |
| `php artisan db:seed` | Run the root `DatabaseSeeder` |
| `php artisan db:seed --class=UserSeeder` | Run a specific seeder class |

> [!CAUTION]
> Never run `migrate:fresh` in production. It drops all tables and destroys data permanently.

---

## Optimization & Caching

Use caching in production to boost performance by eliminating runtime file scanning and route parsing.

| Command | Description |
| --- | --- |
| `php artisan optimize` | Caches bootstrap files, configuration, and routes |
| `php artisan optimize:clear` | Clears all cached bootstrap files, config, routes, views, and events |
| `php artisan route:cache` | Compiles all routes into a single cached file |
| `php artisan route:clear` | Deletes the compiled route cache |
| `php artisan config:cache` | Merges all config files into a single cached file |
| `php artisan config:clear` | Deletes the compiled configuration cache |
| `php artisan view:cache` | Pre-compiles all Blade templates |
| `php artisan view:clear` | Clears compiled Blade view files |
| `php artisan cache:clear` | Flushes the application data cache (Redis/Memcached/file) |
| `php artisan event:cache` | Caches discovered events and listeners |
| `php artisan event:clear` | Clears the event cache |

> [!TIP]
> During local development, run `php artisan optimize:clear` if route or configuration changes fail to reflect.

---

## Routing & Inspection

| Command | Description |
| --- | --- |
| `php artisan route:list` | Lists all registered application routes |
| `php artisan route:list --path=api` | Filters routes by URI path |
| `php artisan route:list --except-vendor` | Excludes vendor-registered routes |
| `php artisan about` | Displays environment, PHP version, cache drivers, and framework details |

---

## Development & Maintenance

| Command | Description |
| --- | --- |
| `php artisan serve` | Starts local PHP development server (`http://127.0.0.1:8000`) |
| `php artisan serve --port=8080` | Starts server on a specified port |
| `php artisan tinker` | Interactive REPL shell powered by PsySH |
| `php artisan down` | Puts the application into maintenance mode |
| `php artisan down --secret="bypass-token"` | Enables maintenance mode with a bypass cookie |
| `php artisan up` | Brings the application out of maintenance mode |
| `php artisan queue:work` | Starts processing queued jobs |
| `php artisan schedule:work` | Runs the task scheduler locally (daemon mode for local dev) |
