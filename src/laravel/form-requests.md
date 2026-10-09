> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Form Requests & Validation

Form Requests are dedicated HTTP request classes that encapsulate authorization and validation logic. They keep controllers lean and ensure input data is authorized and validated before controller methods execute.

## Generating a Form Request

```bash
php artisan make:request StorePostRequest
```

Files are stored in `app/Http/Requests/`.

---

## Form Request Anatomy

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Support\Str;

class StorePostRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     */
    public function authorize(): bool
    {
        // Return true to allow, or check policy / user permissions
        return $this->user() !== null;
    }

    /**
     * Modify input data before validation rules run.
     */
    protected function prepareForValidation(): void
    {
        $this->merge([
            'slug' => Str::slug($this->input('title')),
        ]);
    }

    /**
     * Get the validation rules that apply to the request.
     */
    public function rules(): array
    {
        return [
            'title'       => ['required', 'string', 'max:255'],
            'slug'        => ['required', 'string', 'unique:posts,slug'],
            'category_id' => ['required', 'integer', 'exists:categories,id'],
            'content'     => ['required', 'string', 'min:20'],
            'tags'        => ['nullable', 'array'],
            'tags.*'      => ['string', 'max:30'],
            'cover_image' => ['nullable', 'image', 'mimes:jpeg,png,webp', 'max:2048'],
        ];
    }

    /**
     * Custom validation error messages.
     */
    public function messages(): array
    {
        return [
            'category_id.exists' => 'The selected category does not exist.',
            'cover_image.max'    => 'Cover image size must not exceed 2MB.',
        ];
    }

    /**
     * Custom attribute names for validation errors.
     */
    public function attributes(): array
    {
        return [
            'category_id' => 'category',
        ];
    }
}
```

> [!NOTE]
> If `authorize()` returns `false`, Laravel automatically halts execution and returns an HTTP `403 Forbidden` response.

---

## Controller Usage

Type-hint the Form Request in your controller action. Validation runs automatically before entering the method body:

```php
namespace App\Http\Controllers;

use App\Http\Requests\StorePostRequest;
use App\Models\Post;
use Illuminate\Http\RedirectResponse;

class PostController extends Controller
{
    public function store(StorePostRequest $request): RedirectResponse
    {
        // Retrieve all validated data (excludes unvalidated input fields)
        $validated = $request->validated();

        // Retrieve a specific subset of validated data
        $data = $request->safe()->only(['title', 'slug', 'content']);

        $post = Post::create($validated);

        return redirect()->route('posts.show', $post)
            ->with('success', 'Post created successfully!');
    }
}
```

---

## Common Validation Rules

| Rule | Description | Example |
| --- | --- | --- |
| `required` | Field must be present and not empty | `'title' => 'required'` |
| `nullable` | Field can be null; validation rules only run if value exists | `'bio' => 'nullable\|string'` |
| `string` / `integer` / `boolean` / `array` | Type checks | `'age' => 'integer'` |
| `email` | Valid email address format | `'email' => 'required\|email'` |
| `unique:table,column` | Value must not exist in specified table/column | `'email' => 'unique:users,email'` |
| `exists:table,column` | Value must exist in database table | `'role_id' => 'exists:roles,id'` |
| `min:value` / `max:value` | Minimum / maximum length (strings) or numeric value | `'password' => 'min:8'` |
| `confirmed` | Requires matching field with `_confirmation` suffix | `'password' => 'confirmed'` (checks `password_confirmation`) |
| `date` / `after:date` / `before:date` | Valid date comparisons | `'end_date' => 'date\|after:start_date'` |
| `image` / `file` | Valid uploaded file | `'avatar' => 'file\|image\|max:1024'` |
| `mimes:foo,bar` | Valid MIME file extensions | `'resume' => 'mimes:pdf,docx\|max:5120'` |
| `in:foo,bar` | Value must match one of the listed values | `'status' => 'in:draft,published,archived'` |
| `regex:/pattern/` | Custom regex validation | `'code' => 'regex:/^[A-Z]{3}-\d{4}$/'` |

---

## Validation Behavior & Error Handling

- **Web Requests**: On failure, Laravel redirects back to the previous form with input preserved (`old('field')`) and error messages stored in `$errors`.
- **API / JSON Requests**: On failure, Laravel returns an HTTP `422 Unprocessable Content` response containing a JSON payload with validation messages:

```json
{
  "message": "The title field is required.",
  "errors": {
    "title": ["The title field is required."],
    "category_id": ["The selected category does not exist."]
  }
}
```
