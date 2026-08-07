<div dir="rtl">

# ۳۰ سؤال تخصصی `Authorization` در `Django REST Framework`

این فایل شامل ۳۰ سؤال سطح بالا در مصاحبه‌های شغلی `Backend` با تمرکز بر کنترل دسترسی در `DRF` است.

پاسخ‌ها کوتاه، مستقیم و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

## مفاهیم پایه `Authorization`

### ۱. تفاوت `Authentication` و `Authorization` را با مثال توضیح دهید.

**پاسخ:**

`Authentication` تأیید می‌کند که کاربر کیست.

مثلاً کاربر با `Email` و `Password` لاگین می‌کند.

`Authorization` تعیین می‌کند که کاربر چه دسترسی‌هایی دارد.

مثلاً کاربر عادی نمی‌تواند به پنل `Admin` دسترسی داشته باشد.

---

### ۲. `Authorization` در `DRF` در چه مرحله‌ای اجرا می‌شود؟

**پاسخ:**

ابتدا `Authentication` انجام می‌شود.

سپس `Permission`ها بررسی می‌شوند.

در نهایت `Throttling` اعمال می‌شود.

این ترتیب در `initial()` متد `APIView` اجرا می‌شود.

---

### ۳. `Permission Class`های پیش‌فرض `DRF` کدامند؟

**پاسخ:**

`AllowAny` به همه کاربران دسترسی می‌دهد.

`IsAuthenticated` فقط به کاربران لاگین‌شده دسترسی می‌دهد.

`IsAdminUser` فقط به کاربران `is_staff=True` دسترسی می‌دهد.

`IsAuthenticatedOrReadOnly` برای کاربران ناشناس فقط `Safe Methods` مجاز است.

---

### ۴. `Safe Methods` در `DRF` چیست؟

**پاسخ:**

`Safe Methods` شامل `GET`، `HEAD` و `OPTIONS` هستند.

این متدها داده‌ای را تغییر نمی‌دهند.

`IsAuthenticatedOrReadOnly` فقط این متدها را برای کاربران ناشناس مجاز می‌داند.

---

### ۵. چگونه `Permission` پیش‌فرض را برای کل پروژه تنظیم می‌کنید؟

**پاسخ:**

در `settings.py` کلید `DEFAULT_PERMISSION_CLASSES` تنظیم می‌شود.

این تنظیم برای تمام `View`ها اعمال می‌شود.

<div dir="ltr">

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
}
```

</div>

---

## `Permission` در سطح `View`

### ۶. چگونه `Permission` را فقط برای یک `View` خاص تنظیم می‌کنید؟

**پاسخ:**

از `permission_classes` `Attribute` در `View` استفاده می‌شود.

این تنظیم، تنظیم پیش‌فرض را بازنویسی می‌کند.

<div dir="ltr">

```python
class AdminOnlyView(APIView):
    permission_classes = [IsAdminUser]
```

</div>

---

### ۷. چگونه `Permission` را در `ViewSet` تنظیم می‌کنید؟

**پاسخ:**

در `ViewSet` نیز از `permission_classes` استفاده می‌شود.

برای `Action`های مختلف می‌توان `Permission`های متفاوت تعریف کرد.

متد `get_permissions()` این امکان را فراهم می‌کند.

---

### ۸. چگونه برای `Action`های مختلف `ViewSet`، `Permission` متفاوت تعریف می‌کنید؟

**پاسخ:**

متد `get_permissions()` بازنویسی می‌شود.

بر اساس `self.action`، `Permission` مناسب برگردانده می‌شود.

<div dir="ltr">

```python
def get_permissions(self):
    if self.action == 'list':
        return [AllowAny()]
    return [IsAuthenticated()]
```

</div>

---

### ۹. `permission_classes` و `get_permissions()` چه تفاوتی دارند؟

**پاسخ:**

`permission_classes` یک لیست ثابت است.

`get_permissions()` یک متد پویا است.

`get_permissions()` امکان تعیین `Permission` بر اساس `Request` را می‌دهد.

---

## `Custom Permission`

### ۱۰. چگونه یک `Permission` سفارشی ایجاد می‌کنید؟

**پاسخ:**

از `BasePermission` ارث‌بری می‌شود.

متد `has_permission()` بازنویسی می‌شود.

این متد `True` یا `False` برمی‌گرداند.

<div dir="ltr">

```python
class IsVerifiedUser(BasePermission):
    def has_permission(self, request, view):
        return request.user.is_authenticated and request.user.is_verified
```

</div>

---

### ۱۱. متد `has_permission()` چه ورودی‌هایی دارد؟

**پاسخ:**

`request` شامل اطلاعات درخواست است.

`view` به `View` مربوطه اشاره دارد.

در صورت برگشت `True`، دسترسی مجاز است.

در صورت برگشت `False`، خطای `403` برگردانده می‌شود.

---

### ۱۲. `Object-Level Permission` چیست؟

**پاسخ:**

این نوع `Permission` دسترسی به یک `Object` خاص را بررسی می‌کند.

مثلاً فقط مالک یک پست می‌تواند آن را ویرایش کند.

متد `has_object_permission()` این بررسی را انجام می‌دهد.

---

### ۱۳. متد `has_object_permission()` چه ورودی‌هایی دارد؟

**پاسخ:**

`request` شامل اطلاعات درخواست است.

`view` به `View` مربوطه اشاره دارد.

`obj` به `Object` مورد نظر اشاره دارد.

<div dir="ltr">

```python
def has_object_permission(self, request, view, obj):
    return obj.owner == request.user
```

</div>

---

### ۱۴. `Object-Level Permission` چه زمانی فراخوانی می‌شود؟

**پاسخ:**

فقط زمانی فراخوانی می‌شود که `View` یک `Object` تکی را بازیابی کند.

این اتفاق در `Detail View`ها رخ می‌دهد.

در `List View`ها فراخوانی نمی‌شود.

باید از `check_object_permissions()` استفاده شود.

---

### ۱۵. چگونه `Object-Level Permission` را در `APIView` فراخوانی می‌کنید؟

**پاسخ:**

ابتدا `Object` بازیابی می‌شود.

سپس `self.check_object_permissions()` فراخوانی می‌شود.

<div dir="ltr">

```python
def get_object(self):
    obj = get_object_or_404(Post, pk=self.kwargs['pk'])
    self.check_object_permissions(self.request, obj)
    return obj
```

</div>

---

## `Role-Based Access Control`

### ۱۶. `RBAC` چیست؟

**پاسخ:**

`RBAC` مخفف `Role-Based Access Control` است.

دسترسی‌ها بر اساس نقش کاربر تعیین می‌شوند.

مثلاً نقش `Admin`، `Editor` و `Viewer` تعریف می‌شود.

هر نقش مجموعه‌ای از `Permission`ها دارد.

---

### ۱۷. چگونه `RBAC` را در `Django` پیاده‌سازی می‌کنید؟

**پاسخ:**

از `Group`های `Django` استفاده می‌شود.

هر `Group` یک نقش را نشان می‌دهد.

`Permission`ها به `Group` اختصاص داده می‌شوند.

کاربران به `Group`ها اضافه می‌شوند.

---

### ۱۸. چگونه یک `Permission` سفارشی بر اساس نقش ایجاد می‌کنید؟

**پاسخ:**

یک `Custom Permission` تعریف می‌شود.

نقش‌های مجاز به‌صورت پارامتر دریافت می‌شوند.

<div dir="ltr">

```python
class HasRole(BasePermission):
    allowed_roles = []

    def has_permission(self, request, view):
        return request.user.groups.filter(
            name__in=self.allowed_roles
        ).exists()
```

</div>

---

### ۱۹. چگونه `Permission`های `Django` را در `DRF` استفاده می‌کنید؟

**پاسخ:**

از `DjangoModelPermissions` استفاده می‌شود.

این کلاس از `Permission`های `Model` در `Django` استفاده می‌کند.

`DjangoModelPermissionsOrAnonReadOnly` برای کاربران ناشناس فقط `GET` مجاز است.

---

### ۲۰. `DjangoModelPermissions` چگونه کار می‌کند؟

**پاسخ:**

برای `POST` به `Permission` با نام `add_modelname` نیاز دارد.

برای `PUT` و `PATCH` به `Permission` با نام `change_modelname` نیاز دارد.

برای `DELETE` به `Permission` با نام `delete_modelname` نیاز دارد.

برای `GET` نیازی به `Permission` ندارد.

---

## `Field-Level Permission`

### ۲۱. `Field-Level Permission` چیست؟

**پاسخ:**

کنترل دسترسی به `Field`های خاص یک `Model`.

مثلاً فقط `Admin` می‌تواند `Field` حساس را ببیند.

این کار معمولاً در `Serializer` انجام می‌شود.

---

### ۲۲. چگونه `Field-Level Permission` را در `Serializer` پیاده‌سازی می‌کنید؟

**پاسخ:**

متد `to_representation()` بازنویسی می‌شود.

بر اساس نقش کاربر، `Field`های حساس حذف می‌شوند.

<div dir="ltr">

```python
def to_representation(self, instance):
    data = super().to_representation(instance)
    request = self.context.get('request')
    if not request.user.is_staff:
        data.pop('salary', None)
    return data
```

</div>

---

### ۲۳. چگونه `Field` را در `Serializer` فقط برای خواندن محدود می‌کنید؟

**پاسخ:**

از `read_only_fields` در `Meta` استفاده می‌شود.

یا `Field` با پارامتر `read_only=True` تعریف می‌شود.

کاربر نمی‌تواند این `Field`ها را ارسال کند.

---

## `Group` و `Permission` در `Django`

### ۲۴. چگونه `Permission` سفارشی برای `Model` تعریف می‌کنید؟

**پاسخ:**

در کلاس `Meta` از `permissions` استفاده می‌شود.

<div dir="ltr">

```python
class Meta:
    permissions = [
        ('can_publish', 'Can publish post'),
    ]
```

</div>

---

### ۲۵. چگونه `Permission` را به `Group` اختصاص می‌دهید؟

**پاسخ:**

از `Django Admin` استفاده می‌شود.

یا به‌صورت برنامه‌نویسی انجام می‌شود.

<div dir="ltr">

```python
group.permissions.add(permission)
```

</div>

---

### ۲۶. چگونه بررسی می‌کنید کاربر یک `Permission` خاص دارد؟

**پاسخ:**

از متد `has_perm()` استفاده می‌شود.

<div dir="ltr">

```python
request.user.has_perm('app.can_publish')
```

</div>

این متد `True` یا `False` برمی‌گرداند.

---

## `Middleware` و `Authorization`

### ۲۷. آیا می‌توان `Authorization` را در `Middleware` پیاده‌سازی کرد؟

**پاسخ:**

بله، امکان‌پذیر است.

`Middleware` قبل از `View` اجرا می‌شود.

برای `Authorization` سراسری مناسب است.

اما `Permission`های `DRF` انعطاف‌پذیرتر هستند.

---

### ۲۸. مزایای استفاده از `Permission`های `DRF` نسبت به `Middleware` چیست؟

**پاسخ:**

`Permission`های `DRF` در سطح `View` اعمال می‌شوند.

قابل استفاده مجدد هستند.

با `ViewSet` و `GenericAPIView` سازگار هستند.

`Object-Level Permission` را پشتیبانی می‌کنند.

---

## `Testing` و `Security`

### ۲۹. چگونه `Permission`ها را تست می‌کنید؟

**پاسخ:**

از `APITestCase` استفاده می‌شود.

کاربران مختلف با نقش‌های مختلف ایجاد می‌شوند.

درخواست‌های مختلف ارسال می‌شوند.

`Status Code` بررسی می‌شود.

<div dir="ltr">

```python
def test_admin_can_delete(self):
    self.client.force_authenticate(user=self.admin_user)
    response = self.client.delete(f'/api/posts/{self.post.id}/')
    self.assertEqual(response.status_code, 204)
```

</div>

---

### ۳۰. بهترین شیوه‌های `Authorization` در `DRF` چیست؟

**پاسخ:**

از `Deny by Default` استفاده شود.

`Permission`ها در سطح `View` تعریف شوند.

برای `Object-Level` حتماً `has_object_permission()` پیاده‌سازی شود.

از `RBAC` برای مدیریت دسترسی‌ها استفاده شود.

`Permission`ها تست شوند.

از `Logging` برای ردیابی دسترسی‌های غیرمجاز استفاده شود.

---

## جمع‌بندی نکات کلیدی

| موضوع | نکته کلیدی |
|---|---|
| `IsAuthenticated` | فقط کاربران لاگین‌شده |
| `IsAdminUser` | فقط `is_staff=True` |
| `AllowAny` | همه دسترسی دارند |
| `Object-Level` | فقط در `Detail View` |
| `RBAC` | بر اساس نقش کاربر |
| `DjangoModelPermissions` | بر اساس `Permission`های `Model` |
| `Field-Level` | در `Serializer` پیاده‌سازی شود |
| `Testing` | حتماً `Permission`ها تست شوند |

</div>
