<div dir="rtl">

۳۰ سؤال تخصصی و سطح بالا در مصاحبه Django

این فایل شامل ۳۰ سؤال پرتکرار و عمیق در مصاحبه‌های شغلی Backend با تمرکز بر فریم‌ورک Django است. پاسخ‌ها کوتاه، مستقیم و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

ORM و QuerySet

۱. تفاوت select_related() و prefetch_related() چیست؟

پاسخ: select_related() با استفاده از SQL JOIN برای روابط ForeignKey و OneToOne یک کوئری واحد تولید می‌کند. prefetch_related() برای روابط ManyToMany و Reverse ForeignKey استفاده می‌شود و دو کوئری جداگانه در Python به هم متصل می‌کند.

۲. کاربرد F() Expression چیست؟

پاسخ: F() امکان ارجاع به مقدار یک Field در Model را مستقیماً در سطح Database فراهم می‌کند. این کار از بارگذاری داده‌ها به Memory جلوگیری کرده و از Race Condition در عملیات‌های همزمان جلوگیری می‌کند.

<div dir="ltr">

```python
Product.objects.update(stock=F('stock') - 1)
```

</div>

۳. تفاوت annotate() و aggregate() چیست؟

پاسخ: aggregate() یک مقدار واحد روی کل QuerySet محاسبه می‌کند و یک Dictionary برمی‌گرداند. annotate() یک مقدار محاسبه‌شده را برای هر Object در QuerySet اضافه می‌کند و همچنان یک QuerySet برمی‌گرداند.

۴. کاربرد Q Object چیست؟

پاسخ: Q برای ترکیب شرط‌های پیچیده با عملگرهای OR (|)، AND (&) و NOT (~) در QuerySet استفاده می‌شود. این کار با kwargs معمولی امکان‌پذیر نیست.

۵. کاربرد Subquery و OuterRef چیست؟

پاسخ: Subquery برای ایجاد کوئری‌های تودرتو در سطح Database استفاده می‌شود. OuterRef به Fieldهای کوئری بیرونی در داخل Subquery ارجاع می‌دهد. این روش جایگزین بهینه‌تری برای حلقه‌های Python است.

۶. کاربرد only() و defer() چیست؟

پاسخ: هر دو برای Query Optimization استفاده می‌شوند. only() فقط Fieldهای مشخص‌شده را از Database بارگذاری می‌کند. defer() همه Fieldها به جز Fieldهای مشخص‌شده را بارگذاری می‌کند.

۷. QuerySet در Django چگونه Lazy عمل می‌کند؟

پاسخ: ایجاد یک QuerySet هیچ کوئری به Database ارسال نمی‌کند. کوئری فقط زمانی اجرا می‌شود که QuerySet ارزیابی شود؛ مانند Iteration، Slicing، تبدیل به list یا فراخوانی len().

---

Models و ارث‌بری

۸. تفاوت Abstract Base Class، Multi-Table Inheritance و Proxy Model چیست؟

پاسخ: Abstract Base Class یک Model والد است که جدول مخصوص خود را در Database ندارد. Multi-Table Inheritance برای هر Model یک جدول جداگانه در Database ایجاد می‌کند. Proxy Model ساختار جدول را تغییر نمی‌دهد و فقط برای اضافه کردن متدها یا تغییر Manager استفاده می‌شود.

۹. کاربرد پارامتر through در ManyToManyField چیست؟

پاسخ: این پارامتر برای تعریف یک Model میانی سفارشی استفاده می‌شود. زمانی کاربرد دارد که نیاز به ذخیره Fieldهای اضافی روی رابطه Many-to-Many باشد.

۱۰. معایب استفاده از Signals چیست؟

پاسخ: Signals جریان کد را پنهان می‌کنند و Debug را سخت می‌سازند. همچنین امکان ایجاد Circular Imports و Unexpected Side Effects وجود دارد. در صورت امکان استفاده از متدهای صریح مانند بازنویسی save() ترجیح داده می‌شود.

۱۱. تفاوت save() و create() چیست؟

پاسخ: create() یک Object جدید را در یک مرحله ساخته و در Database ذخیره می‌کند. save() روی یک Instance موجود فراخوانی می‌شود. save() متد pre_save و post_save را تریگر می‌کند، در حالی که QuerySet.bulk_create() این Signals را فراخوانی نمی‌کند.

۱۲. کاربرد Manager سفارشی چیست؟

پاسخ: Manager سفارشی برای تعریف QuerySetهای پرکاربرد و فیلترهای پیش‌فرض استفاده می‌شود. این کار باعث Clean Code و استفاده مجدد از Query Logic در کل پروژه می‌شود.

<div dir="ltr">

```python
class PublishedManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(status='published')
```

</div>

---

Migrations

۱۳. کاربرد RunPython در Migrations چیست؟

پاسخ: RunPython برای اجرای کدهای Python سفارشی در زمان Migration استفاده می‌شود. معمولاً برای Data Migration مانند پر کردن داده‌های اولیه یا تغییر فرمت داده‌های موجود کاربرد دارد.

۱۴. Squashing Migrations چیست و چه زمانی استفاده می‌شود؟

پاسخ: این فرایند چند Migration قدیمی را به یک Migration واحد تبدیل می‌کند. در پروژه‌های بالغ با تعداد زیاد Migration، برای کاهش زمان اعمال Migrations در محیط‌های جدید استفاده می‌شود.

۱۵. چگونه یک Migration را بدون اعمال در Database ایجاد می‌کنید؟

پاسخ: با استفاده از دستور makemigrations و سپس ویرایش دستی فایل Migration یا با استفاده از RunPython و RunSQL برای کنترل دقیق‌تر عملیات.

---

Middleware و Request Lifecycle

۱۶. ترتیب اجرای Middlewareها چگونه است؟

پاسخ: در مسیر Request به سمت View، Middlewareها به ترتیب تعریف‌شده در MIDDLEWARE اجرا می‌شوند. در مسیر Response به سمت کاربر، ترتیب اجرای آن‌ها برعکس می‌شود.

۱۷. تفاوت process_request و process_view چیست؟

پاسخ: process_request قبل از تعیین View مناسب فراخوانی می‌شود. process_view بعد از تعیین View اما قبل از اجرای واقعی آن فراخوانی می‌شود و به View Function دسترسی دارد.

---

Authentication و Authorization

۱۸. چرا باید از Custom User Model از ابتدای پروژه استفاده کرد؟

پاسخ: جایگزینی User Model پس از اعمال اولین Migration بسیار دشوار است و نیاز به Data Migration پیچیده دارد. Custom User Model از ابتدا انعطاف‌پذیری لازم برای تغییرات آینده مانند استفاده از Email به‌جای Username را فراهم می‌کند.

۱۹. تفاوت Permission و Group چیست؟

پاسخ: Permission یک مجوز خاص برای انجام یک عمل روی یک Model است. Group مجموعه‌ای از Permissionها است که می‌تواند به چندین کاربر به‌صورت همزمان اختصاص داده شود.

---

Admin

۲۰. کاربرد list_select_related در Django Admin چیست؟

پاسخ: این ویژگی باعث می‌شود Admin از select_related() برای بارگذاری روابط ForeignKey در لیست استفاده کند. این کار تعداد کوئری‌های Database را در صفحات Admin با داده‌های زیاد به‌شدت کاهش می‌دهد.

۲۱. چگونه یک Action سفارشی در Admin ایجاد می‌کنید؟

پاسخ: با تعریف یک متد در ModelAdmin و استفاده از @admin.action Decorator. این متد باید request و queryset را به‌عنوان ورودی دریافت کند.

---

Views و URLs

۲۲. چه زمانی Class-Based Views را به Function-Based Views ترجیح می‌دهید؟

پاسخ: Class-Based Views برای عملیات استاندارد CRUD، استفاده از Mixins و کدهای قابل استفاده مجدد مناسب هستند. Function-Based Views برای Logicهای خاص و پیچیده که در قالب کلاس نمی‌گنجند ترجیح داده می‌شوند.

۲۳. کاربرد Mixinها در Views چیست؟

پاسخ: Mixinها قابلیت‌های قابل استفاده مجدد را ارائه می‌دهند. ترکیب GenericAPIView با Mixinsهایی مانند ListModelMixin و CreateModelMixin امکان ساخت سریع Views استاندارد را بدون کدنویسی تکراری فراهم می‌کند.

---

Caching

۲۴. سطوح مختلف Caching در Django کدامند؟

پاسخ: Django چند سطح Caching ارائه می‌دهد: Per-Site Cache از طریق Middleware، Per-View Cache با @cache_page، Template Fragment Caching و Low-Level Cache API برای Cache کردن اشیای خاص در کد.

۲۵. کاربرد Cache Backendهای مختلف چیست؟

پاسخ: LocMemCache برای Development و تست مناسب است. Redis و Memcached برای محیط Production به دلیل Speed بالا و پشتیبانی از Distributed Cache توصیه می‌شوند. DatabaseCache برای داده‌های کمتر تغییرکننده استفاده می‌شود.

---

Testing

۲۶. تفاوت TestCase و TransactionTestCase چیست؟

پاسخ: TestCase هر تست را در یک Transaction می‌پیچد و در انتها آن را Rollback می‌کند که سریع‌تر است. TransactionTestCase تست را با Truncate کردن جداول پایان می‌دهد و برای تست Transactionها یا Signals خاص لازم است.

۲۷. جایگزین مدرن Fixtures چیست؟

پاسخ: Factory Boy جایگزین مدرن Fixtures است. Factories انعطاف‌پذیرتر، قابل استفاده مجدد و برای تست‌های پیچیده مناسب‌تر هستند.

---

Performance و Optimization

۲۸. کاربرد bulk_create() و bulk_update() چیست؟

پاسخ: این متدها برای ایجاد یا آپدیت تعداد زیادی Object در یک کوئری واحد استفاده می‌شوند. این روش بسیار سریع‌تر از فراخوانی save() در یک حلقه است، اما Signals را تریگر نمی‌کند.

۲۹. کاربرد db_index و indexes در Meta چیست؟

پاسخ: db_index=True یک Index روی یک Field واحد ایجاد می‌کند. indexes در کلاس Meta برای ایجاد Compound Indexes روی چند Field به‌صورت همزمان استفاده می‌شود که برای کوئری‌های پیچیده بسیار بهینه‌تر است.

<div dir="ltr">

```python
class Meta:
    indexes = [
        models.Index(fields=['last_name', 'first_name']),
    ]
```

</div>

---

Security و مباحث پیشرفته

۳۰. تفاوت CSRF و CORS چیست؟

پاسخ: CSRF از ارسال Requestهای ناخواسته از سمت مرورگر کاربر به Server شما جلوگیری می‌کند. CORS تعیین می‌کند که کدام Domainهای خارجی اجازه دارند به API شما از طریق مرورگر دسترسی داشته باشند. این دو مکانیزم امنیتی مستقل هستند و باید هر دو در Production به‌درستی تنظیم شوند.

</div>