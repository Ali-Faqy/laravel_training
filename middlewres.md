# Laravel Middleware Cheat Sheet

Middleware هي طبقات **تصفية** أو **فحص** للـ HTTP Requests قبل الوصول لتطبيقك. يمكن أن تستخدم للتحقق من صلاحيات المستخدم، تسجيل الطلبات، حماية ضد CSRF، وغيرها.

---

## 1️⃣ مقدمة

* Middleware هو **طبقة وسيطة** تمر بها جميع الطلبات قبل الوصول للتطبيق.
* مثال: Middleware يتحقق إذا كان المستخدم مسجل دخول، إذا لم يكن كذلك يعيده لشاشة تسجيل الدخول.
* يمكن كتابة Middleware لأي غرض: تسجيل، حماية، تعديل الطلبات، إلخ.
* جميع Middleware موجودة في `app/Http/Middleware`.

---

## 2️⃣ إنشاء Middleware جديد

```bash
php artisan make:middleware EnsureTokenIsValid
```

* سينشئ ملفًا جديدًا: `app/Http/Middleware/EnsureTokenIsValid.php`

### مثال عملي:

```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

class EnsureTokenIsValid
{
    public function handle(Request $request, Closure $next): Response
    {
        if ($request->input('token') !== 'my-secret-token') {
            return redirect('/home'); // إعادة التوجيه إذا التوكن غير صالح
        }

        return $next($request); // السماح للطلب بالمرور
    }
}
```

* `$next($request)` يمرر الطلب للطبقة التالية أو للتطبيق.
* Middleware يشبه **سلسلة من الطبقات** تمر عبرها الطلبات.

---

## 3️⃣ Middleware قبل وبعد الطلب

### قبل معالجة الطلب:

```php
class BeforeMiddleware
{
    public function handle(Request $request, Closure $next): Response
    {
        // تنفيذ كود قبل الطلب
        return $next($request);
    }
}
```

### بعد معالجة الطلب:

```php
class AfterMiddleware
{
    public function handle(Request $request, Closure $next): Response
    {
        $response = $next($request);
        // تنفيذ كود بعد معالجة الطلب
        return $response;
    }
}
```

---

## 4️⃣ تسجيل Middleware

### Middleware عام (Global Middleware)

```php
use App\Http\Middleware\EnsureTokenIsValid;

->withMiddleware(function ($middleware) {
    $middleware->append(EnsureTokenIsValid::class);
});
```

* `append()` تضيف Middleware في نهاية السلسلة.
* `prepend()` تضيفه في البداية.

### Middleware على Route محدد

```php
Route::get('/profile', function () {
    // محتوى الصفحة
})->middleware(EnsureTokenIsValid::class);
```

* يمكن إضافة أكثر من Middleware:

```php
Route::get('/', function () {
    // ...
})->middleware([First::class, Second::class]);
```

### استثناء Middleware

```php
Route::middleware([EnsureTokenIsValid::class])->group(function () {
    Route::get('/', function () {});

    Route::get('/profile', function () {})->withoutMiddleware([EnsureTokenIsValid::class]);
});
```

---

## 5️⃣ Middleware Groups

```php
->withMiddleware(function ($middleware) {
    $middleware->appendToGroup('group-name', [
        First::class,
        Second::class,
    ]);
});

Route::middleware('group-name')->group(function () {
    Route::get('/', function () {});
});
```

### Middleware Groups الافتراضية:

* **web**: التشفير، الجلسات، CSRF، وغيرها.
* **api**: SubstituteBindings، throttle، إلخ.

---

## 6️⃣ Aliases لـ Middleware

```php
$middleware->alias([
    'subscribed' => EnsureUserIsSubscribed::class
]);

Route::get('/profile', function () {})->middleware('subscribed');
```

* مثال Alias افتراضي: `auth`, `guest`, `throttle`.

---

## 7️⃣ Middleware Parameters

```php
Route::put('/post/{id}', function (string $id) {})->middleware(EnsureUserHasRole::class.':editor,publisher');
```

```php
class EnsureUserHasRole
{
    public function handle(Request $request, Closure $next, string $role): Response
    {
        if (! $request->user()->hasRole($role)) {
            // إعادة توجيه
        }

        return $next($request);
    }
}
```

---

## 8️⃣ Terminable Middleware

```php
class TerminatingMiddleware
{
    public function handle(Request $request, Closure $next): Response
    {
        return $next($request);
    }

    public function terminate(Request $request, Response $response): void
    {
        // كود بعد إرسال Response
    }
}
```

* يجب تسجيله في `bootstrap/app.php`.
* إذا أردت استخدام نفس الكائن في handle و terminate، سجل Middleware كـ **singleton**:

```php
$this->app->singleton(TerminatingMiddleware::class);
```
