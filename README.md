# Laravel Training

هذا الملف يشرح **هيكل مشروع Laravel** بطريقة مبسطة وواضحة، مع شرح **باللغة العربية والإنجليزية**.

---

## Root Directory (المجلد الرئيسي)

### `/app`

**Arabic:** يحتوي على الكود الأساسي للتطبيق (منطق العمل).
**English:** Contains the core application logic.

#### `app/Http`

**Arabic:** مسؤول عن استقبال طلبات الويب.
**English:** Handles incoming HTTP requests.

* **Controllers**
  Arabic: تستقبل الطلب وتتعامل معه وتعيد response.
  English: Handle requests and return responses.

* **Middleware**
  Arabic: فحص الطلب قبل أو بعد تنفيذه (مثل التحقق من تسجيل الدخول).
  English: Filters requests before/after they reach controllers.

* **Requests**
  Arabic: التحقق من صحة البيانات (Validation).
  English: Validate incoming request data.

---

#### `app/Models`

Arabic: يحتوي على موديلات قاعدة البيانات والعلاقات بينها.
English: Contains Eloquent models representing database tables.

---

#### `app/Services`

Arabic: يحتوي على منطق العمل (Business Logic).
English: Holds reusable business logic.

---

#### `app/Jobs`

Arabic: مهام تعمل في الخلفية (Queues).
English: Background jobs (queued or sync).

---

#### `app/Events`

Arabic: أحداث تحصل داخل التطبيق.
English: Events triggered by actions in the app.

---

#### `app/Listeners`

Arabic: تستمع للأحداث وتنفذ كود معين.
English: Listen to events and handle them.

---

#### `app/Policies`

Arabic: تحديد صلاحيات المستخدمين.
English: Authorization logic.

---

#### `app/Providers`

Arabic: تهيئة وربط الخدمات داخل التطبيق.
English: Bootstrap and bind services.

---

## `/routes`

Arabic: تعريف مسارات التطبيق.
English: Define application routes.

* **web.php**
  Arabic: مسارات الويب مع sessions و CSRF.
  English: Web routes with session & CSRF.

* **api.php**
  Arabic: مسارات API بدون session.
  English: Stateless API routes.

---

## `/resources`

Arabic: يحتوي على Blade Views و CSS و JS.
English: Views and uncompiled frontend assets.

---

## `/public`

Arabic: نقطة الدخول للتطبيق والملفات العامة.
English: Public entry point and assets.

---

## `/config`

Arabic: ملفات الإعدادات.
English: Configuration files.

---

## `/database`

Arabic: migrations و seeders و factories.
English: Database structure and seeding.

---

## `/storage`

Arabic: التخزين، الكاش، اللوجات.
English: Logs, cache, and file storage.

---

## `/tests`

Arabic: اختبارات التطبيق.
English: Application tests.

---

## Request Lifecycle (رحلة الطلب)

Arabic:

1. Route
2. Middleware
3. Controller
4. Service / Model
5. Response

English:

1. Route
2. Middleware
3. Controller
4. Service / Model
5. Response

---

## Best Practices (أفضل الممارسات)

* Arabic: لا تضع منطق العمل داخل Controller.
  English: Keep controllers thin.

* Arabic: استخدم Services للمنطق المعقد.
  English: Use services for business logic.

* Arabic: استخدم Requests للتحقق من البيانات.
  English: Use Form Requests for validation.

---

## Code Examples (أمثلة كود)

---

### Route Example (routes/web.php)

```php
Route::get('/', [HomeController::class, 'index']);
```

Arabic: تعريف مسار يوجّه إلى Controller.
English: Define a route pointing to a controller.

---

### Controller Example (app/Http/Controllers/HomeController.php)

```php
class HomeController extends Controller
{
    public function index()
    {
        return view('home');
    }
}
```

Arabic: يستقبل الطلب ويرجع View.
English: Handles request and returns a view.

---

### Middleware Example (app/Http/Middleware/CheckUser.php)

```php
class CheckUser
{
    public function handle($request, Closure $next)
    {
        if (!auth()->check()) {
            return redirect('/login');
        }
        return $next($request);
    }
}
```

Arabic: فحص المستخدم قبل تنفيذ الطلب.
English: Check user before request continues.

---

### Request Validation Example (app/Http/Requests/StoreUserRequest.php)

```php
class StoreUserRequest extends FormRequest
{
    public function rules()
    {
        return [
            'name' => 'required',
            'email' => 'required|email'
        ];
    }
}
```

Arabic: التحقق من صحة البيانات.
English: Validate incoming data.

---

### Model Example (app/Models/User.php)

```php
class User extends Model
{
    protected $fillable = ['name', 'email'];
}
```

Arabic: يمثل جدول في قاعدة البيانات.
English: Represents a database table.

---

### Service Example (app/Services/UserService.php)

```php
class UserService
{
    public function create(array $data)
    {
        return User::create($data);
    }
}
```

Arabic: منطق العمل بعيد عن Controller.
English: Business logic separated from controller.

---

### Job Example (app/Jobs/SendEmailJob.php)

```php
class SendEmailJob implements ShouldQueue
{
    public function handle()
    {
        // send email
    }
}
```

Arabic: مهمة تعمل في الخلفية.
English: Background job.

---

### Event Example (app/Events/UserRegistered.php)

```php
class UserRegistered
{
    public $user;

    public function __construct($user)
    {
        $this->user = $user;
    }
}
```

Arabic: حدث عند تسجيل مستخدم.
English: Event triggered on user registration.

---

### Listener Example (app/Listeners/SendWelcomeEmail.php)

```php
class SendWelcomeEmail
{
    public function handle(UserRegistered $event)
    {
        // send welcome email
    }
}
```

Arabic: يستمع للحدث وينفذ كود.
English: Listens to event and reacts.

---

### Blade View Example (resources/views/home.blade.php)

```blade
<x-layout>
    <h1>Welcome</h1>
</x-layout>
```

Arabic: عرض واجهة المستخدم.
English: UI view.

---

### Blade Component Example (resources/views/components/nav-link.blade.php)

```blade
<a {{ $attributes }}>
    {{ $slot }}
</a>
```

Arabic: مكون قابل لإعادة الاستخدام.
English: Reusable Blade component.

---

✨ **This README is suitable for learning, training, and GitHub projects.**
