<div dir="rtl">

۳۰ سؤال تخصصی و سطح بالا در مصاحبه Django

این فایل شامل ۳۰ سؤال پرتکرار و عمیق در مصاحبه‌های شغلی Backend با تمرکز بر فریم‌ورک Django است.

پاسخ‌ها کوتاه، مستقیم و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

ORM و QuerySet

۱. تفاوت select_related() و prefetch_related() چیست؟

پاسخ:

select_related() برای روابط ForeignKey و OneToOne استفاده می‌شود.

این متد از SQL JOIN استفاده می‌کند و یک کوئری واحد تولید می‌کند.

prefetch_related() برای روابط ManyToMany و Reverse ForeignKey استفاده می‌شود.

این متد دو کوئری جداگانه اجرا می‌کند و نتایج را در Python به هم متصل می‌کند.

---

۲. کاربرد F() Expression چیست؟

پاسخ:

F() امکان ارجاع به مقدار یک Field را مستقیماً در سطح Database فراهم می‌کند.

این کار از بارگذاری داده‌ها به Memory جلوگیری می‌کند.

همچنین از Race Condition در عملیات‌های همزمان جلوگیری می‌شود.

<div dir="ltr">

```python
Product.objects.update(stock=F('stock') - 1)
```

</div>

---

۳. تفاوت annotate() و aggregate() چیست؟

پاسخ:

aggregate() یک مقدار واحد روی کل QuerySet محاسبه می‌کند.

خروجی این متد یک Dictionary است.

annotate() یک مقدار محاسبه‌شده را برای هر Object اضافه می‌کند.

خروجی این متد همچنان یک QuerySet است.

---

۴. کاربرد Q Object چیست؟

پاسخ:

Q برای ترکیب شرط‌های پیچیده در QuerySet استفاده می‌شود.

با این کلاس می‌توان از عملگرهای OR، AND و NOT استفاده کرد.

این کار با kwargs معمولی امکان‌پذیر نیست.

<div dir="ltr">

```python
from django.db.models import Q

Post.objects.filter(Q(status='published') | Q(status='draft'))
```

</div>

---

۵. کاربرد Subquery و OuterRef چیست؟

پاسخ:

Subquery برای ایجاد کوئری‌های تودرتو در سطح Database استفاده می‌شود.

OuterRef به Fieldهای کوئری بیرونی در داخل Subquery ارجاع می‌دهد.

این روش جایگزین بهینه‌تری برای حلقه‌های Python است.

---

۶. کاربرد only() و defer() چیست؟

پاسخ:

هر دو متد برای Query Optimization استفاده می‌شوند.

only() فقط Fieldهای مشخص‌شده را از Database بارگذاری می‌کند.

defer() همه Fieldها به جز Fieldهای مشخص‌شده را بارگذاری می‌کند.

---

۷. QuerySet در Django چگونه Lazy عمل می‌کند؟

پاسخ:

ایجاد یک QuerySet هیچ کوئری به Database ارسال نمی‌کند.

کوئری فقط زمانی اجرا می‌شود که QuerySet ارزیابی شود.

این ارزیابی شامل Iteration، Slicing، تبدیل به list یا فراخوانی len() است.

---

Models و ارث‌بری

۸. تفاوت Abstract Base Class، Multi-Table Inheritance و Proxy Model چیست؟

پاسخ:

Abstract Base Class یک Model والد است که جدول مخصوص خود را در Database ندارد.

Multi-Table Inheritance برای هر Model یک جدول جداگانه در Database ایجاد می‌کند.

Proxy Model ساختار جدول را تغییر نمی‌دهد.

Proxy Model فقط برای اضافه کردن متدها یا تغییر Manager استفاده می‌شود.

---

۹. کاربرد پارامتر through در ManyToManyField چیست؟

پاسخ:

این پارامتر برای تعریف یک Model میانی سفارشی استفاده می‌شود.

زمانی کاربرد دارد که نیاز به ذخیره Fieldهای اضافی روی رابطه Many-to-Many باشد.

---

۱۰. معایب استفاده از Signals چیست؟

پاسخ:

Signals جریان کد را پنهان می‌کنند و Debug را سخت می‌سازند.

همچنین احتمال ایجاد Circular Imports وجود دارد.

احتمال Unexpected Side Effects نیز وجود دارد.

در صورت امکان استفاده از متدهای صریح مانند بازنویسی save() ترجیح داده می‌شود.

---

۱۱. تفاوت save() و create() چیست؟

پاسخ:

create() یک Object جدید را در یک مرحله ساخته و در Database ذخیره می‌کند.

save() روی یک Instance موجود فراخوانی می‌شود.

save() متدهای pre_save و post_save را تریگر می‌کند.

bulk_create() این Signals را فراخوانی نمی‌کند.

---

۱۲. کاربرد Manager سفارشی چیست؟

پاسخ:

Manager سفارشی برای تعریف QuerySetهای پرکاربرد استفاده می‌شود.

این کلاس برای فیلترهای پیش‌فرض نیز کاربرد دارد.

این کار باعث Clean Code و استفاده مجدد از Query Logic می‌شود.

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

پاسخ:

RunPython برای اجرای کدهای Python سفارشی در زمان Migration استفاده می‌شود.

معمولاً برای Data Migration کاربرد دارد.

مثال‌هایی شامل پر کردن داده‌های اولیه یا تغییر فرمت داده‌های موجود است.

---

۱۴. Squashing Migrations چیست و چه زمانی استفاده می‌شود؟

پاسخ:

این فرایند چند Migration قدیمی را به یک Migration واحد تبدیل می‌کند.

در پروژه‌های بالغ با تعداد زیاد Migration استفاده می‌شود.

هدف کاهش زمان اعمال Migrations در محیط‌های جدید است.

---

۱۵. چگونه یک Migration را بدون اعمال در Database ایجاد می‌کنید؟

پاسخ:

با استفاده از دستور makemigrations فایل Migration ایجاد می‌شود.

سپس می‌توان فایل را به‌صورت دستی ویرایش کرد.

همچنین می‌توان از RunPython و RunSQL برای کنترل دقیق‌تر عملیات استفاده کرد.

---

Middleware و Request Lifecycle

۱۶. ترتیب اجرای Middlewareها چگونه است؟

پاسخ:

در مسیر Request به سمت View، Middlewareها به ترتیب تعریف‌شده اجرا می‌شوند.

این ترتیب در لیست MIDDLEWARE در settings.py مشخص شده است.

در مسیر Response به سمت کاربر، ترتیب اجرای آن‌ها برعکس می‌شود.

---

۱۷. تفاوت process_request و process_view چیست؟

پاسخ:

process_request قبل از تعیین View مناسب فراخوانی می‌شود.

process_view بعد از تعیین View اما قبل از اجرای واقعی آن فراخوانی می‌شود.

process_view به View Function دسترسی دارد.

---

Authentication و Authorization

۱۸. چرا باید از Custom User Model از ابتدای پروژه استفاده کرد؟

پاسخ:

جایگزینی User Model پس از اعمال اولین Migration بسیار دشوار است.

این کار نیاز به Data Migration پیچیده دارد.

Custom User Model از ابتدا انعطاف‌پذیری لازم را فراهم می‌کند.

مثلاً می‌توان از Email به‌جای Username استفاده کرد.

---

۱۹. تفاوت Permission و Group چیست؟

پاسخ:

Permission یک مجوز خاص برای انجام یک عمل روی یک Model است.

Group مجموعه‌ای از Permissionها است.

Group می‌تواند به چندین کاربر به‌صورت همزمان اختصاص داده شود.

---

Admin

۲۰. کاربرد list_select_related در Django Admin چیست؟

پاسخ:

این ویژگی باعث می‌شود Admin از select_related() استفاده کند.

این کار برای بارگذاری روابط ForeignKey در لیست استفاده می‌شود.

تعداد کوئری‌های Database در صفحات Admin به‌شدت کاهش می‌یابد.

---

۲۱. چگونه یک Action سفارشی در Admin ایجاد می‌کنید؟

پاسخ:

با تعریف یک متد در ModelAdmin این کار انجام می‌شود.

سپس از @admin.action Decorator استفاده می‌شود.

این متد باید request و queryset را به‌عنوان ورودی دریافت کند.

<div dir="ltr">

```python
@admin.action(description='Mark selected posts as published')
def make_published(modeladmin, request, queryset):
    queryset.update(status='published')
```

</div>

---

Views و URLs

۲۲. چه زمانی Class-Based Views را به Function-Based Views ترجیح می‌دهید؟

پاسخ:

Class-Based Views برای عملیات استاندارد CRUD مناسب هستند.

این Views برای استفاده از Mixins و کدهای قابل استفاده مجدد نیز مناسب‌اند.

Function-Based Views برای Logicهای خاص و پیچیده ترجیح داده می‌شوند.

---

۲۳. کاربرد Mixinها در Views چیست؟

پاسخ:

Mixinها قابلیت‌های قابل استفاده مجدد را ارائه می‌دهند.

ترکیب GenericAPIView با Mixinsهایی مانند ListModelMixin امکان ساخت سریع Views را فراهم می‌کند.

این کار از کدنویسی تکراری جلوگیری می‌کند.

---

Caching

۲۴. سطوح مختلف Caching در Django کدامند؟

پاسخ:

Per-Site Cache از طریق Middleware اعمال می‌شود.

Per-View Cache با @cache_page اعمال می‌شود.

Template Fragment Caching برای بخش‌هایی از Template استفاده می‌شود.

Low-Level Cache API برای Cache کردن اشیای خاص در کد استفاده می‌شود.

---

۲۵. کاربرد Cache Backendهای مختلف چیست؟

پاسخ:

LocMemCache برای Development و تست مناسب است.

Redis و Memcached برای محیط Production توصیه می‌شوند.

دلیل این توصیه Speed بالا و پشتیبانی از Distributed Cache است.

DatabaseCache برای داده‌های کمتر تغییرکننده استفاده می‌شود.

---

Testing

۲۶. تفاوت TestCase و TransactionTestCase چیست؟

پاسخ:

TestCase هر تست را در یک Transaction می‌پیچد.

در انتها Transaction به‌صورت Rollback می‌شود.

این روش سریع‌تر است.

TransactionTestCase تست را با Truncate کردن جداول پایان می‌دهد.

این کلاس برای تست Transactionها یا Signals خاص لازم است.

---

۲۷. جایگزین مدرن Fixtures چیست؟

پاسخ:

Factory Boy جایگزین مدرن Fixtures است.

Factories انعطاف‌پذیرتر هستند.

این Factories قابل استفاده مجدد هستند.

برای تست‌های پیچیده مناسب‌تر هستند.

---

Performance و Optimization

۲۸. کاربرد bulk_create() و bulk_update() چیست؟

پاسخ:

این متدها برای ایجاد یا آپدیت تعداد زیادی Object استفاده می‌شوند.

عملیات در یک کوئری واحد انجام می‌شود.

این روش بسیار سریع‌تر از فراخوانی save() در یک حلقه است.

اما Signals را تریگر نمی‌کند.

---

۲۹. کاربرد db_index و indexes در Meta چیست؟

پاسخ:

db_index=True یک Index روی یک Field واحد ایجاد می‌کند.

indexes در کلاس Meta برای ایجاد Compound Indexes استفاده می‌شود.

Compound Indexes روی چند Field به‌صورت همزمان اعمال می‌شوند.

این روش برای کوئری‌های پیچیده بسیار بهینه‌تر است.

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

پاسخ:

CSRF از ارسال Requestهای ناخواسته جلوگیری می‌کند.

این Requestها از سمت مرورگر کاربر به Server شما ارسال می‌شوند.

CORS تعیین می‌کند که کدام Domainهای خارجی اجازه دسترسی دارند.

این دسترسی از طریق مرورگر به API شما انجام می‌شود.

این دو مکانیزم امنیتی مستقل هستند.

هر دو باید در Production به‌درستی تنظیم شوند.

</div>