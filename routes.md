# Laravel Routing Cheat Sheet

هذا الدليل يشرح **Routing** في Laravel خطوة بخطوة مع أمثلة عملية لفهم كل نوع من الروتس وكيفية استخدامها.

---

## 1️⃣ Basic Routing

أبسط نوع من الروتس. تستقبل URI و Closure لتحديد السلوك.

```php
use Illuminate\Support\Facades\Route;

Route::get('/greeting', function () {
    return 'Hello World';
});
```

**شرح:**
عند زيارة `/greeting` في المتصفح، سيعرض "Hello World". لا تحتاج لأي Controller.

---

## 2️⃣ Default Route Files

* `routes/web.php` → للواجهات العادية (HTML + Forms + Views)
* `routes/api.php` → للـ API (JSON فقط)

**مثال ويب:**

```php
use App\Http\Controllers\UserController;

Route::get('/user', [UserController::class, 'index']);
```

**شرح:**
عند زيارة `/user`، Laravel يشغل `index` داخل `UserController`.

---

## 3️⃣ API Routes

* يستخدم للـ API فقط
* Stateless → لا يعتمد على الجلسات أو CSRF

```php
use Illuminate\Http\Request;

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');
```

**شرح:**
يعرض بيانات المستخدم الموثق. الـ URI يبدأ تلقائيًا بـ `/api`.

---

## 4️⃣ Router Methods

يمكنك تعريف الروتس لكل HTTP Verb:

```php
use Illuminate\Support\Facades\Route;

Route::match(['get', 'post'], '/', function () {
    return "GET or POST request";
});

Route::any('/', function () {
    return "Any HTTP method";
});
```

---

## 5️⃣ Dependency Injection

يمكن حقن الـ Request أو أي خدمة تلقائيًا في الـ Route:

```php
use Illuminate\Http\Request;

Route::get('/users', function (Request $request) {
    return $request->ip();
});
```

---

## 6️⃣ CSRF Protection

```blade
<form method="POST" action="/profile">
    @csrf
</form>
```

**شرح:**
مطلوب للـ POST, PUT, PATCH, DELETE في web.php لمنع هجمات CSRF.

---

## 7️⃣ Redirect Routes

```php
Route::redirect('/here', '/there', 301);
```

**شرح:**
تحويل من `/here` إلى `/there` مع رمز الحالة 301.

---

## 8️⃣ View Routes

```php
Route::view('/welcome', 'welcome', ['name' => 'Ali']);
```

**شرح:**
يعرض view مباشرة بدون الحاجة Controller.

---

## 9️⃣ Route Parameters

### Required

```php
Route::get('/user/{id}', function ($id) {
    return "User $id";
});
```

### Optional

```php
Route::get('/user/{name?}', function ($name = 'John') {
    return $name;
});
```

### Constraints

```php
Route::get('/user/{id}', function ($id) {
    return $id;
})->whereNumber('id');
```

---

## 🔟 Named Routes

```php
use App\Http\Controllers\UserController;

Route::get('/user/profile', [UserController::class, 'show'])->name('profile');
```

```php
$url = route('profile'); // URL
return redirect()->route('profile'); // Redirect
```

---

## 1️⃣1️⃣ Route Groups

### Middleware

```php
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', function () {});
});
```

### Prefix

```php
Route::prefix('admin')->group(function () {
    Route::get('/users', function () {}); // /admin/users
});
```

---

## 1️⃣2️⃣ Route Model Binding

### Implicit

```php
use App\Models\User;

Route::get('/users/{user}', function (User $user) {
    return $user->email;
});
```

### Custom Key

```php
Route::get('/posts/{post:slug}', function (Post $post) {
    return $post->title;
});
```

---

## 1️⃣3️⃣ Fallback Route

```php
Route::fallback(function () {
    return "404 - Page not found";
});
```

**شرح:**
يتم تنفيذه إذا لم تطابق أي Route.

---

## 1️⃣4️⃣ Rate Limiting

```php
use Illuminate\Support\Facades\RateLimiter;
use Illuminate\Cache\RateLimiting\Limit;
use Illuminate\Http\Request;

RateLimiter::for('uploads', function (Request $request) {
    return Limit::perMinute(10)->by($request->ip());
});

Route::middleware(['throttle:uploads'])->group(function () {
    Route::post('/audio', function () {});
});
```

**شرح:**
تحديد الحد الأقصى لعدد الطلبات للـ Route لكل IP أو User.

---

## 📌 نصائح إضافية

* استخدم `php artisan route:list` لعرض كل الروتس.
* استخدم `route:cache` لتسريع الروتس في الإنتاج.
* لكل Route يمكن إضافة Middleware, Prefix, Name، Model Binding وكل شيء حسب الحاجة.

> هذا الملخص يغطي أهم 14 نقطة في Laravel Routing مع أمثلة عملية لكل نوع Route.
