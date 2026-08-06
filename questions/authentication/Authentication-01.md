<div dir="rtl">

# ۳۰ سؤال تخصصی `Authentication` و `JWT` در `Django REST Framework`

این فایل شامل ۳۰ سؤال سطح بالا در مصاحبه‌های شغلی `Backend` با تمرکز بر احراز هویت در `DRF` است.

پاسخ‌ها کوتاه، مستقیم و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

## مفاهیم پایه `Authentication`

### ۱. تفاوت `Authentication` و `Authorization` چیست؟

**پاسخ:**

`Authentication` هویت کاربر را تأیید می‌کند.

این فرایند مشخص می‌کند که کاربر کیست.

`Authorization` سطح دسترسی کاربر را تعیین می‌کند.

این فرایند مشخص می‌کند که کاربر چه عملیاتی مجاز است انجام دهد.

---

### ۲. `Authentication Class`های پیش‌فرض `DRF` کدامند؟

**پاسخ:**

`SessionAuthentication` از `Session`های `Django` استفاده می‌کند.

`TokenAuthentication` از `Token`های ساده استفاده می‌کند.

`BasicAuthentication` از `HTTP Basic Auth` استفاده می‌کند.

هر کدام برای سناریوهای خاصی مناسب هستند.

---

### ۳. `SessionAuthentication` چگونه کار می‌کند؟

**پاسخ:**

کاربر با `Username` و `Password` لاگین می‌کند.

`Django` یک `Session ID` در `Cookie` ذخیره می‌کند.

در درخواست‌های بعدی، `Session ID` ارسال می‌شود.

سرور هویت کاربر را از طریق `Session` شناسایی می‌کند.

---

### ۴. محدودیت `SessionAuthentication` برای `API` چیست؟

**پاسخ:**

`Session`ها `Stateful` هستند.

این روش برای `Distributed Systems` مناسب نیست.

`Scaling` در این روش پیچیده‌تر است.

برای `Mobile App`ها و `SPA`ها مناسب نیست.

---

### ۵. `TokenAuthentication` ساده چه محدودیت‌هایی دارد؟

**پاسخ:**

`Token` منقضی نمی‌شود.

امکان `Refresh` وجود ندارد.

در صورت لو رفتن `Token`، دسترسی برای همیشه باقی می‌ماند.

فقط یک `Token` برای هر کاربر وجود دارد.

---

## `JWT` و ساختار آن

### ۶. ساختار `JWT` چگونه است؟

**پاسخ:**

`JWT` از سه بخش تشکیل شده است.

`Header` شامل نوع و الگوریتم رمزنگاری است.

`Payload` شامل `Claims` و اطلاعات کاربر است.

`Signature` برای اعتبارسنجی `Token` استفاده می‌شود.

این سه بخش با نقطه از هم جدا می‌شوند.

---

### ۷. `Claims` در `JWT` چیست؟

**پاسخ:**

`Claims` اطلاعات ذخیره‌شده در `Payload` هستند.

`Registered Claims` شامل `iss`، `exp` و `sub` هستند.

`Public Claims` بین طرفین توافق می‌شوند.

`Private Claims` برای استفاده داخلی هستند.

---

### ۸. تفاوت `Access Token` و `Refresh Token` چیست؟

**پاسخ:**

`Access Token` عمر کوتاهی دارد.

این `Token` برای دسترسی به `API` استفاده می‌شود.

`Refresh Token` عمر طولانی‌تری دارد.

این `Token` برای دریافت `Access Token` جدید استفاده می‌شود.

---

### ۹. چرا `Access Token` باید عمر کوتاه داشته باشد؟

**پاسخ:**

در صورت لو رفتن، زمان سوءاستفاده محدود است.

نیاز به `Token Blacklisting` کاهش می‌یابد.

`Security` سیستم افزایش می‌یابد.

معمولاً بین ۵ تا ۱۵ دقیقه تنظیم می‌شود.

---

### ۱۰. `Refresh Token Rotation` چیست؟

**پاسخ:**

در هر بار استفاده از `Refresh Token`، یک `Refresh Token` جدید صادر می‌شود.

`Refresh Token` قدیمی باطل می‌شود.

این کار امنیت را افزایش می‌دهد.

در صورت سرقت `Refresh Token`، فقط یک بار قابل استفاده است.

---

## `Simple JWT` در `DRF`

### ۱۱. چگونه `Simple JWT` را پیکربندی می‌کنید؟

**پاسخ:**

تنظیمات در `settings.py` قرار می‌گیرد.

کلید `SIMPLE_JWT` برای تنظیمات استفاده می‌شود.

`ACCESS_TOKEN_LIFETIME` عمر `Access Token` را مشخص می‌کند.

`REFRESH_TOKEN_LIFETIME` عمر `Refresh Token` را مشخص می‌شود.

---

### ۱۲. چگونه `JWT` را در `DRF` فعال می‌کنید؟

**پاسخ:**

در `REST_FRAMEWORK` کلید `DEFAULT_AUTHENTICATION_CLASSES` تنظیم می‌شود.

کلاس `JWTAuthentication` اضافه می‌شود.

در `urls.py` مسیرهای `Token` تعریف می‌شود.

<div dir="ltr">

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}
```

</div>

---

### ۱۳. `TokenObtainPairView` چه کاری انجام می‌دهد؟

**پاسخ:**

این `View` برای لاگین کاربر استفاده می‌شود.

`Username` و `Password` را دریافت می‌کند.

در صورت موفقیت، `Access Token` و `Refresh Token` برمی‌گرداند.

---

### ۱۴. `TokenRefreshView` چه کاری انجام می‌دهد؟

**پاسخ:**

این `View` برای دریافت `Access Token` جدید استفاده می‌شود.

`Refresh Token` را به‌عنوان ورودی دریافت می‌کند.

یک `Access Token` جدید برمی‌گرداند.

---

### ۱۵. `TokenBlacklistView` چه کاربردی دارد؟

**پاسخ:**

این `View` برای `Logout` استفاده می‌شود.

`Refresh Token` را در لیست سیاه قرار می‌دهد.

پس از این عملیات، `Refresh Token` دیگر قابل استفاده نیست.

---

## `Cookie` و `Set-Cookie`

### ۱۶. چگونه `JWT` را در `Cookie` ذخیره می‌کنید؟

**پاسخ:**

در `Response` از متد `set_cookie()` استفاده می‌شود.

`Token` در `Cookie` ذخیره می‌شود.

این روش برای `SPA`ها مناسب است.

از `LocalStorage` امن‌تر است.

---

### ۱۷. `HttpOnly Cookie` چیست و چرا مهم است؟

**پاسخ:**

`HttpOnly` مانع دسترسی `JavaScript` به `Cookie` می‌شود.

این ویژگی از حملات `XSS` جلوگیری می‌کند.

در `set_cookie()` پارامتر `httponly=True` تنظیم می‌شود.

---

### ۱۸. `Secure Cookie` چیست؟

**پاسخ:**

`Secure` تضمین می‌کند `Cookie` فقط از طریق `HTTPS` ارسال شود.

از شنود `Cookie` در شبکه جلوگیری می‌کند.

در `Production` حتماً باید فعال باشد.

---

### ۱۹. `SameSite Cookie` چیست؟

**پاسخ:**

`SameSite` کنترل می‌کند `Cookie` در درخواست‌های `Cross-Site` ارسال شود یا خیر.

مقدار `Strict` اجازه ارسال در هیچ درخواست خارجی را نمی‌دهد.

مقدار `Lax` اجازه ارسال در `GET`های خارجی را می‌دهد.

مقدار `None` اجازه ارسال در همه درخواست‌ها را می‌دهد.

---

### ۲۰. چگونه `Cookie` را در `DRF` تنظیم می‌کنید؟

**پاسخ:**

در `View` از `response.set_cookie()` استفاده می‌شود.

پارامترهای امنیتی تنظیم می‌شوند.

<div dir="ltr">

```python
response.set_cookie(
    key='access_token',
    value=str(token),
    httponly=True,
    secure=True,
    samesite='Lax',
    max_age=300
)
```

</div>

---

## `CSRF` و امنیت

### ۲۱. `CSRF Token` چیست و چرا لازم است؟

**پاسخ:**

`CSRF` مخفف `Cross-Site Request Forgery` است.

این حمله از `Session` کاربر برای ارسال درخواست ناخواسته استفاده می‌کند.

`CSRF Token` یک مقدار تصادفی است که در هر فرم قرار می‌گیرد.

سرور این `Token` را در هر درخواست اعتبارسنجی می‌کند.

---

### ۲۲. آیا `JWT` به `CSRF` نیاز دارد؟

**پاسخ:**

اگر `JWT` در `Authorization Header` ارسال شود، نیازی به `CSRF` نیست.

اگر `JWT` در `Cookie` ذخیره شود، `CSRF` لازم است.

در `DRF` کلاس `SessionAuthentication` به `CSRF` نیاز دارد.

---

### ۲۳. چگونه `CSRF` را برای `API` غیرفعال می‌کنید؟

**پاسخ:**

در `View` از `@csrf_exempt` `Decorator` استفاده می‌شود.

یا از `CsrfExemptSessionAuthentication` استفاده می‌شود.

این کار فقط زمانی مجاز است که `JWT` در `Header` ارسال شود.

---

## `Permission` و دسترسی

### ۲۴. `Permission Class`های پیش‌فرض `DRF` کدامند؟

**پاسخ:**

`AllowAny` به همه کاربران دسترسی می‌دهد.

`IsAuthenticated` فقط به کاربران لاگین‌شده دسترسی می‌دهد.

`IsAdminUser` فقط به کاربران `Admin` دسترسی می‌دهد.

`IsAuthenticatedOrReadOnly` برای کاربران ناشناس فقط `GET` مجاز است.

---

### ۲۵. چگونه `Permission` سفارشی ایجاد می‌کنید؟

**پاسخ:**

از `BasePermission` ارث‌بری می‌شود.

متد `has_permission()` بازنویسی می‌شود.

متد `has_object_permission()` برای دسترسی به `Object` تکی بازنویسی می‌شود.

<div dir="ltr">

```python
class IsOwner(BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.owner == request.user
```

</div>

---

### ۲۶. `Object-Level Permission` چیست؟

**پاسخ:**

این نوع `Permission` دسترسی به یک `Object` خاص را بررسی می‌کند.

مثلاً فقط مالک یک پست می‌تواند آن را ویرایش کند.

متد `has_object_permission()` این بررسی را انجام می‌دهد.

---

## `Throttling` و محدودیت

### ۲۷. `Throttling` چیست و چرا استفاده می‌شود؟

**پاسخ:**

`Throttling` تعداد درخواست‌های یک کاربر را محدود می‌کند.

از حملات `Brute Force` جلوگیری می‌کند.

از `Abuse` و مصرف بیش از حد منابع جلوگیری می‌کند.

---

### ۲۸. چگونه `Throttling` سفارشی برای لاگین ایجاد می‌کنید؟

**پاسخ:**

از `AnonRateThrottle` ارث‌بری می‌شود.

متد `get_cache_key()` بازنویسی می‌شود.

`rate` محدودیت را مشخص می‌کند.

<div dir="ltr">

```python
class LoginThrottle(AnonRateThrottle):
    rate = '5/minute'

    def get_cache_key(self, request, view):
        return self.cache_format % {
            'scope': 'login',
            'ident': request.META.get('REMOTE_ADDR')
        }
```

</div>

---

## مباحث پیشرفته

### ۲۹. `Token Blacklisting` چگونه پیاده‌سازی می‌شود؟

**پاسخ:**

در `Simple JWT` اپ `token_blacklist` فعال می‌شود.

در `SIMPLE_JWT` کلید `BLACKLIST_AFTER_ROTATION` فعال می‌شود.

در زمان `Logout`، `Refresh Token` در لیست سیاه قرار می‌گیرد.

---

### ۳۰. `Multi-Factor Authentication` را چگونه پیاده‌سازی می‌کنید؟

**پاسخ:**

پس از تأیید `Password`، یک کد تأیید ارسال می‌شود.

این کد از طریق `SMS` یا `Email` یا `TOTP` ارسال می‌شود.

پس از تأیید کد، `JWT` صادر می‌شود.

این فرایند امنیت را به‌شدت افزایش می‌دهد.

---

## جمع‌بندی نکات کلیدی

| موضوع | نکته کلیدی |
|---|---|
| `Access Token` | عمر کوتاه، در `Header` ارسال شود |
| `Refresh Token` | عمر طولانی، در `HttpOnly Cookie` ذخیره شود |
| `HttpOnly` | از دسترسی `JavaScript` جلوگیری می‌کند |
| `Secure` | فقط در `HTTPS` ارسال شود |
| `SameSite` | از حملات `CSRF` جلوگیری می‌کند |
| `Token Rotation` | `Refresh Token` قدیمی باطل شود |
| `Blacklisting` | در `Logout` حتماً انجام شود |

</div>