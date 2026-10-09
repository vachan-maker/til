> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Authorization & Policies

Laravel provides two primary mechanisms for authorizing user actions: **Gates** (closure-based) and **Policies** (class-based, organized around Eloquent models).

| Feature | Best For | Location |
| --- | --- | --- |
| **Gates** | General actions not tied to a specific model (e.g. view admin panel) | Registered in `AppServiceProvider` or `AuthServiceProvider` |
| **Policies** | Resource actions tied to a model (e.g. view, update, delete a `Post`) | `app/Policies/` |

---

## Generating Policies

```bash
# Generate policy mapped to a specific model
php artisan make:policy PostPolicy --model=Post

# Generate a blank policy
php artisan make:policy AdminPolicy
```

Policies are saved to `app/Policies/`.

---

## Policy Structure

```php
namespace App\Policies;

use App\Models\Post;
use App\Models\User;
use Illuminate\Auth\Access\Response;

class PostPolicy
{
    /**
     * Perform pre-authorization checks (e.g. grant all permissions to Super Admin).
     */
    public function before(User $user, string $ability): ?bool
    {
        if ($user->is_super_admin) {
            return true;
        }

        return null; // Fall through to individual policy methods
    }

    /**
     * Determine if user can view the list of posts.
     */
    public function viewAny(User $user): bool
    {
        return true;
    }

    /**
     * Determine if user can view a specific post.
     */
    public function view(?User $user, Post $post): bool
    {
        // Type-hinting nullable ?User allows unauthenticated guests
        return $post->is_published || ($user && $user->id === $post->user_id);
    }

    /**
     * Determine if user can create posts.
     */
    public function create(User $user): bool
    {
        return $user->hasVerifiedEmail();
    }

    /**
     * Determine if user can update the post.
     */
    public function update(User $user, Post $post): bool
    {
        return $user->id === $post->user_id;
    }

    /**
     * Determine if user can delete the post.
     */
    public function delete(User $user, Post $post): Response
    {
        return $user->id === $post->user_id
            ? Response::allow()
            : Response::deny('You do not own this post.');
    }
}
```

> [!NOTE]
> Laravel automatically discovers policies if the policy class is in `app/Policies/` and matches the model name (e.g. `App\Models\Post` -> `App\Policies\PostPolicy`).

---

## Authorizing Actions

### 1. In Controllers

Use `$this->authorize()` (or `Gate::authorize()`). If unauthorized, Laravel automatically throws an `AuthorizationException` resulting in an HTTP `403 Forbidden` response:

```php
namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    public function update(Request $request, Post $post)
    {
        $this->authorize('update', $post);

        // Update post...
    }

    public function destroy(Post $post)
    {
        $this->authorize('delete', $post);

        $post->delete();
    }
}
```

### 2. In Form Requests

Authorize directly inside the `authorize()` method of a Form Request:

```php
public function authorize(): bool
{
    $post = $this->route('post');

    return $this->user()->can('update', $post);
}
```

### 3. In Blade Templates

Control UI visibility using `@can` and `@cannot` directives:

```blade
@can('update', $post)
    <a href="{{ route('posts.edit', $post) }}">Edit Post</a>
@endcan

@can('delete', $post)
    <button type="submit">Delete Post</button>
@endcan

@cannot('update', $post)
    <p class="text-muted">You cannot edit this post.</p>
@endcannot
```

### 4. In Route Middleware

Protect routes directly at the routing level:

```php
use App\Http\Controllers\PostController;
use Illuminate\Support\Facades\Route;

Route::put('/posts/{post}', [PostController::class, 'update'])
    ->middleware('can:update,post');

Route::delete('/posts/{post}', [PostController::class, 'destroy'])
    ->middleware('can:delete,post');
```

---

## Defining & Using Gates

Gates are ideal for standalone actions not tied to a single model:

```php
// Defined in AppServiceProvider.php (boot method)
use App\Models\User;
use Illuminate\Support\Facades\Gate;

Gate::define('access-admin-panel', function (User $user) {
    return $user->is_admin;
});
```

Using Gates in code:

```php
// Check permission boolean
if (Gate::allows('access-admin-panel')) {
    // Proceed
}

// Check denial
if (Gate::denies('access-admin-panel')) {
    abort(403);
}

// Authorize or abort with 403
Gate::authorize('access-admin-panel');
```
