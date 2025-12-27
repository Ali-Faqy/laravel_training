# Laravel Starter Kits (شرح تفصيلي)

هذا الملف يشرح **Starter Kits** التي يوفرها Laravel لبناء الـ Frontend مع Backend بسهولة.

---

## ما هو Starter Kit؟

**Starter Kit** هو قالب جاهز (Scaffold) ينشئ لك:

* نظام تسجيل دخول (Auth)
* Layout أساسي
* Frontend + Backend مربوطين
* إعدادات Vite و Tailwind

يعني بدل ما تبدأ من صفر 🚀

---

# 1️⃣ Livewire Starter Kit

## الفكرة العامة

* Backend: Laravel
* Frontend: Blade + Livewire
* لغة واحدة تقريبًا: **PHP**

> مناسب جدًا للمبتدئين أو لمحبي Laravel بدون React/Vue

---

## ماذا يوفر؟

* Authentication (Login / Register)
* Livewire Components
* Blade Layouts
* Tailwind CSS
* Vite

---

## مثال Component (Livewire)

```php
class Counter extends Component
{
    public $count = 0;

    public function increment()
    {
        $this->count++;
    }

    public function render()
    {
        return view('livewire.counter');
    }
}
```

```blade
<button wire:click="increment">+</button>
<h1>{{ $count }}</h1>
```

---

## متى أستخدم Livewire Starter Kit؟

✔ مبتدئ
✔ تحب PHP
✔ مشروع Dashboard / Admin Panel

---

# 2️⃣ React + Inertia Starter Kit

## الفكرة العامة

* Backend: Laravel
* Frontend: React
* Bridge: Inertia.js

> Laravel Routes + React UI بدون API منفصل

---

## ماذا يوفر؟

* Auth كامل (Login / Register)
* React Pages
* Inertia Routing
* Tailwind CSS
* Vite

---

## مثال Controller

```php
return Inertia::render('Dashboard', [
    'user' => auth()->user()
]);
```

## مثال React Page

```jsx
export default function Dashboard({ user }) {
  return <h1>Welcome {user.name}</h1>
}
```

---

## مميزات React + Inertia

* SPA Experience
* SEO أفضل من API
* Repo واحد
* Auth سهل

---

## متى أستخدمه؟

✔ بدك React
✔ مشروع حديث
✔ Performance عالي

---

# 3️⃣ Vue + Inertia Starter Kit

## الفكرة العامة

* Backend: Laravel
* Frontend: Vue.js
* Bridge: Inertia.js

> نفس فكرة React لكن باستخدام Vue

---

## مثال Vue Page

```vue
<script setup>
defineProps({ user: Object })
</script>

<template>
  <h1>Hello {{ user.name }}</h1>
</template>
```

---

## متى أستخدم Vue + Inertia؟

✔ تحب Vue
✔ متعود على Ecosystem Vue

---

# مقارنة سريعة

| Starter Kit     | Frontend | لغة | مناسب لمن؟            |
| --------------- | -------- | --- | --------------------- |
| Livewire        | Blade    | PHP | مبتدئ / Laravel Lover |
| React + Inertia | React    | JS  | Modern Apps           |
| Vue + Inertia   | Vue      | JS  | Vue Developers        |

---

# كيف تختار؟

* إذا لسه بتتعلم Laravel → **Livewire**
* إذا بدك شغل سوق → **React + Inertia**
* إذا تحب Vue → **Vue + Inertia**

---

# أوامر التثبيت (مثال)

```bash
laravel new app-name
php artisan starter:install livewire
php artisan starter:install react
php artisan starter:install vue
```

---

📌 نصيحة: لا تتعلم كلهم مع بعض. اختار واحد وامشي فيه صح.
