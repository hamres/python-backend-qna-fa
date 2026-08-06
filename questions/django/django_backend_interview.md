

# سوالات مصاحبه Backend Developer با Django

این فایل پاسخ تشریحی و قابل استفاده برای آمادگی مصاحبه است. تمرکز آن روی Django، Django ORM، PostgreSQL و Python است.

---

## ۱. تو رزومه نوشتی Queryهای PostgreSQL رو ۴۰ درصد بهینه کردی. دقیقاً چه کارهایی کردی؟

بهینه‌سازی را با حدس شروع نمی‌کنم؛ ابتدا Queryهای کند و Bottleneck را اندازه‌گیری می‌کنم. سپس Execution Plan، Indexها، تعداد Queryها، N+1، Joinها، Sortها و حجم داده را بررسی می‌کنم.

کارهایی که معمولاً انجام می‌دهم:

- پیدا کردن Queryهای کند با Django Debug Toolbar و لاگ‌ها
- بررسی Query با `EXPLAIN` و `EXPLAIN ANALYZE`
- ایجاد Index مناسب برای ستون‌های پرتکرار در Filter، Join و بعضی Order Byها
- رفع N+1 با `select_related` و `prefetch_related`
- محدود کردن فیلدهای خروجی با `only`، `values` یا `values_list` در صورت نیاز
- Pagination برای لیست‌های بزرگ
- کاهش Join و Queryهای غیرضروری
- Cache کردن داده‌های مناسب
- اندازه‌گیری دوباره بعد از هر تغییر

مثلاً:

```python
products = Product.objects.select_related("category")
```

> **نکته مهم:** Index را هم بی‌دلیل اضافه نمی‌کنم، چون Index هزینه نگهداری دارد و می‌تواند عملیات Write را سنگین‌تر کند.

### پاسخ مناسب در مصاحبه:

> ابتدا Queryهای کند را با ابزارهای Django و PostgreSQL پیدا کردم. سپس با EXPLAIN ANALYZE Execution Plan را بررسی کردم و مشکلاتی مثل N+1، Index نامناسب، Join اضافی و دریافت داده بیش از نیاز را اصلاح کردم. بعد از هر تغییر دوباره Benchmark گرفتم تا مطمئن شوم بهبود واقعی اتفاق افتاده است.

---

## ۲. چطوری فهمیدی Bottleneck سیستم کجاست؟

اصل مهم این است:

> **بدون اندازه‌گیری، Bottleneck را حدس نمی‌زنم.**

Bottleneck ممکن است در Database، Python، CPU، RAM، Network، Disk، Cache یا External API باشد.

ابتدا زمان کل Request را بررسی می‌کنم و بعد مشخص می‌کنم این زمان کجا مصرف شده است.

مثلاً اگر:

```
Database:      100ms
Python:        200ms
External API:  1600ms
```

باشد، مشکل اصلی Database نیست؛ External API Bottleneck است.

برای Database از Query logging و `EXPLAIN ANALYZE` استفاده می‌کنم. برای Application هم در صورت نیاز Profiling و Monitoring انجام می‌دهم.

---

## ۳. چه چیزهایی را Cache کردی؟ Search و Detail را Cache کردی؟

Cache را بر اساس رفتار داده انتخاب می‌کنم.

### Search

Search می‌تواند پارامترهای زیادی داشته باشد:

```
/search?q=django&page=2&category=backend
```

بنابراین Cache Key باید وابسته به پارامترهای مؤثر روی نتیجه باشد.

مثلاً:

```
search:django:backend:2
```

اگر Search بسیار Dynamic باشد، Cache کردن همه حالت‌ها ممکن است سود کمی داشته باشد.

### Detail

صفحه Detail معمولاً گزینه خوبی برای Cache است، مخصوصاً وقتی Read زیاد و تغییرات کم باشد.

مثلاً:

```
product:123
```

را می‌توان Cache کرد.

اما داده‌های حساس، شخصی یا بسیار متغیر را بدون دلیل Cache نمی‌کنم.

---

## ۴. اگر Object Edit یا Delete بشه Cache چی میشه؟ چطور Cache Invalidation رو هندل کردی؟

اگر `product:123` در Cache باشد و Product تغییر کند، Cache قدیمی باید حذف یا بی‌اعتبار شود.

روش ساده:

```python
cache.delete("product:123")
```

اما ممکن است همین Product در Cacheهای دیگری هم وجود داشته باشد:

```
product:123
category:10:products
search:laptop:page:1
homepage:popular-products
```

پس فقط حذف Detail همیشه کافی نیست.

راهکارها شامل:

- TTL
- Cache Versioning
- Namespace
- حذف Cacheهای مرتبط
- Tag-based invalidation در سیستم‌های مناسب
- Event-driven invalidation

است.

> **اصل مهم:** Cache Invalidation یکی از سخت‌ترین بخش‌های طراحی Cache است.

---

## ۵. Python Async کار کردی؟ کجا استفاده کردی و چرا؟

Async برای عملیات I/O-bound مناسب است؛ یعنی زمانی که برنامه بخش زیادی از زمان را منتظر I/O می‌ماند.

مثلاً:

- HTTP Request
- External API
- Socket
- File I/O
- Database در Stack مناسب

نمونه:

```python
import asyncio


async def task_one():
    await asyncio.sleep(1)
    return "one"


async def task_two():
    await asyncio.sleep(1)
    return "two"


async def main():
    return await asyncio.gather(
        task_one(),
        task_two(),
    )
```

اما Async به معنی سریع‌تر شدن همه برنامه‌ها نیست.

برای CPU-bound مثل پردازش سنگین تصویر، Async به‌تنهایی راه‌حل مناسبی نیست و معمولاً باید Worker، Process یا Task Queue در نظر گرفت.

---

## ۶. Product و Category داریم و Product به Category ForeignKey دارد. اگر ۱۰۰ Product برگردانیم چند Query اجرا می‌شود؟

اگر بنویسیم:

```python
products = Product.objects.all()

for product in products:
    print(product.category.name)
```

ممکن است با N+1 مواجه شویم.

در سناریوی ساده:

```
1 Query برای Product
+
100 Query برای Category
=
101 Query
```

راه‌حل:

```python
products = Product.objects.select_related("category")
```

در این حالت Django می‌تواند Product و Category را با JOIN دریافت کند و در سناریوی معمول تعداد Queryها به یک Query کاهش پیدا می‌کند.

---

## ۷. اگر داخل View روی Category هر Product شرط بگذاری، Queryها چطور اجرا می‌شوند؟

بهتر است تا جای ممکن شرط را به Database منتقل کنیم.

به جای:

```python
products = Product.objects.all()

for product in products:
    if product.category.name == "Laptop":
        ...
```

می‌توان نوشت:

```python
products = Product.objects.filter(
    category__name="Laptop"
)
```

در این حالت Database خودش Filter و Join را انجام می‌دهد.

> **اصل مهم:** تا جایی که منطقی است Filtering را به Database بسپار، نه اینکه تمام داده را وارد Python کنی و بعد Loop بزنی.

---

## ۸. select_related و prefetch_related دقیقاً چه فرقی دارند؟

### select_related

برای Relationهایی مثل:

- ForeignKey
- OneToOneField

مناسب است و معمولاً از SQL JOIN استفاده می‌کند.

```python
Product.objects.select_related("category")
```

### prefetch_related

برای Relationهایی مثل:

- ManyToMany
- Reverse ForeignKey

مناسب است و معمولاً چند Query اجرا می‌کند و نتایج را در Python به هم مرتبط می‌کند.

```python
Category.objects.prefetch_related("products")
```

خلاصه:

```
select_related
→ JOIN
→ ForeignKey / OneToOne

prefetch_related
→ چند Query + اتصال نتایج
→ ManyToMany / Reverse Relations
```

---

## ۹. بیشتر با ORM کار کردی یا SQL خام و PostgreSQL؟

ORM انتخاب اول من در Django است چون خوانایی، Maintainability و یکپارچگی خوبی با Django دارد.

اما ORM جایگزین کامل SQL نیست.

اگر Query پیچیده باشد یا قابلیت خاص PostgreSQL نیاز داشته باشم، می‌توانم از مواردی مثل:

- Raw SQL
- Django Expressions
- Database Functions
- Annotation
- قابلیت‌های PostgreSQL

استفاده کنم.

### پاسخ حرفه‌ای:

> بیشتر با ORM کار می‌کنم، ولی SQL و PostgreSQL را هم در حدی می‌شناسم که بتوانم Queryهای ORM را تحلیل کنم، Execution Plan را بخوانم و در صورت نیاز SQL خام بنویسم. ترجیح می‌دهم تا زمانی که ORM نیاز پروژه را پوشش می‌دهد از آن استفاده کنم.

---

## ۱۰. Queryهای کند را چطور پیدا کردی؟ EXPLAIN و EXPLAIN ANALYZE چیست؟

`EXPLAIN` به PostgreSQL نشان می‌دهد Query را با چه Execution Planای اجرا خواهد کرد.

```sql
EXPLAIN
SELECT *
FROM products
WHERE category_id = 10;
```

`EXPLAIN ANALYZE` Query را واقعاً اجرا می‌کند و اطلاعات واقعی اجرای آن را نشان می‌دهد.

```sql
EXPLAIN ANALYZE
SELECT *
FROM products
WHERE category_id = 10;
```

مواردی که بررسی می‌کنم:

- Execution Time
- Planning Time
- Actual Rows
- Estimated Rows
- Scan Type
- Join Strategy
- تعداد Rows خوانده‌شده

### Sequential Scan

ممکن است PostgreSQL کل جدول را Scan کند.

این همیشه بد نیست؛ اگر بخش بزرگی از جدول باید خوانده شود، Sequential Scan می‌تواند بهتر از Index Scan باشد.

> **نکته مهم:** Index داشتن به معنی سریع بودن Query نیست؛ Execution Plan تعیین می‌کند PostgreSQL واقعاً چگونه Query را اجرا کرده است.

---

## ۱۱. فرق Normal Method، Class Method و Static Method در Python چیست؟

### Normal Method

با `self` کار می‌کند و به Instance دسترسی دارد.

```python
class User:
    def hello(self):
        return self.name
```

### Class Method

با `@classmethod` تعریف می‌شود و `cls` دریافت می‌کند.

```python
class User:
    count = 0

    @classmethod
    def get_count(cls):
        return cls.count
```

### Static Method

به شکل خودکار `self` یا `cls` دریافت نمی‌کند.

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b
```

خلاصه:

```
Normal Method → self → Instance
Class Method  → cls  → Class
Static Method → بدون self/cls → رفتار مستقل از State
```

---

## ۱۲. با Django Signal کار کردی؟ چه استفاده‌ای ازش داشتی؟

Signal برای واکنش به Eventهای مشخص Django استفاده می‌شود.

مثلاً:

- `pre_save`
- `post_save`
- `pre_delete`
- `post_delete`

نمونه:

```python
from django.db.models.signals import post_save
from django.dispatch import receiver

from .models import User


@receiver(post_save, sender=User)
def user_created(sender, instance, created, **kwargs):
    if created:
        print(instance.pk)
```

استفاده مناسب می‌تواند برای کارهای جانبی مانند ایجاد Profile یا ثبت Event باشد.

اما Business Logic اصلی پروژه را بیش از حد داخل Signal قرار نمی‌دهم، چون جریان اجرای سیستم را غیرشفاف و Debug را سخت می‌کند.

---

## ۱۳. Signal دقیقاً چطور کار می‌کند و چه زمانی اجرا می‌شود؟

Signal به یک Event متصل می‌شود.

مثلاً:

```
pre_save  → قبل از Save
post_save → بعد از Save
pre_delete → قبل از Delete
post_delete → بعد از Delete
```

اما یک نکته مهم وجود دارد:

> `post_save` به معنی Commit شدن Transaction نیست.

ممکن است `post_save` اجرا شود ولی Transaction بعداً Rollback شود.

بنابراین برای سیستم‌های حساس باید Signal را از مفهوم Database Commit جدا کنیم.

---

## ۱۴. Django Signal از چه Design Pattern پیروی می‌کند؟

Signal از نظر رفتاری بسیار شبیه **Observer Pattern** است.

در Observer:

```
Event / Subject
      ↓
Subscribers
      ↓
Notification
```

در Django:

```
Signal
   ↓
Receiver
   ↓
اجرای منطق
```

پس پاسخ دقیق این است که Signal از نظر معماری به Observer Pattern شباهت زیادی دارد، نه اینکه الزاماً یک پیاده‌سازی کتابی و کامل از آن باشد.

---

## ۱۵. Singleton، Factory، Strategy و Observer را کجا استفاده می‌کنی؟

### Singleton

وقتی واقعاً لازم باشد در یک Context مشخص فقط یک Instance داشته باشیم.

نباید صرفاً برای استفاده از Design Pattern آن را وارد پروژه کرد.

### Factory

برای ایجاد Object بر اساس شرایط.

مثلاً:

```
PaymentFactory
├── ZarinpalPayment
├── StripePayment
└── WalletPayment
```

### Strategy

وقتی چند روش قابل تعویض برای انجام یک کار داریم.

مثلاً:

```
DiscountStrategy
├── PercentageDiscount
├── FixedDiscount
└── CustomerDiscount
```

### Observer

وقتی یک Event باید چند Subscriber را مطلع کند.

Django Signal نمونه‌ای نزدیک به این مفهوم است.

---

## ۱۶. Signalها دقیقاً کجای Django صدا زده می‌شوند؟

Signalها در نقاط مشخصی از Django و ORM Dispatch می‌شوند.

مثلاً هنگام Save شدن Model، Signalهای مربوط به Save ارسال می‌شوند.

برای Register کردن Signal معمولاً ساختاری مانند زیر استفاده می‌شود:

```
app/
├── apps.py
├── models.py
├── signals.py
└── ...
```

و در `AppConfig.ready()`:

```python
from django.apps import AppConfig


class AccountsConfig(AppConfig):
    name = "accounts"

    def ready(self):
        from . import signals
```

> **نکته مهم:** نباید تصور کنیم Signal فقط وقتی از یک View مشخص Model را تغییر می‌دهیم اجرا می‌شود. هر مسیر کدی که Event مربوطه را ایجاد کند می‌تواند Signal را Trigger کند.

---

## ۱۷. اگر بخوای قبل از اجرای Signal حتماً Log ذخیره بشه، چطور این کار را انجام می‌دهی؟

ابتدا باید مشخص کنیم Log دقیقاً چه معنایی دارد.

اگر هدف ثبت Log در همان جریان Database باشد، می‌توان از Transaction استفاده کرد:

```python
from django.db import transaction


with transaction.atomic():
    # Database changes
    ...
```

اگر Log و تغییرات اصلی داخل یک Transaction باشند و Transaction Rollback شود، Log هم می‌تواند Rollback شود.

اگر می‌خواهیم یک Log یا Event فقط بعد از Commit موفق ارسال شود، از `transaction.on_commit()` استفاده می‌کنیم:

```python
from django.db import transaction


transaction.on_commit(
    lambda: send_event()
)
```

اگر لازم باشد Log حتی هنگام Rollback هم باقی بماند، بهتر است Logging مستقل از Transaction اصلی طراحی شود.

> **نکته کلیدی:** Signal، Transaction و Commit سه مفهوم متفاوت هستند و نباید آن‌ها را یکی در نظر گرفت.

---

## ۱۸. خودت را ۲ یا ۵ سال آینده کجا می‌بینی؟

این سؤال بیشتر برای بررسی نگرش، هدف و واقع‌بینی فرد است.

پاسخ خوب نباید بیش از حد کلی یا غیرواقع‌بینانه باشد.

### پاسخ حرفه‌ای:

> در دو تا پنج سال آینده هدفم این است که از نظر فنی به یک Backend Developer قوی‌تر تبدیل شوم؛ کسی که فقط کدنویسی نمی‌کند و بتواند درباره Database، Architecture، Performance، Security و Scalability تصمیم درست بگیرد. دوست دارم در پروژه‌های واقعی مسئولیت بیشتری داشته باشم و به مرور در طراحی و تصمیم‌های فنی پروژه هم نقش داشته باشم. طبیعتاً نمی‌توانم دقیقاً پیش‌بینی کنم پنج سال آینده در چه شرکت یا چه جایگاهی خواهم بود، اما هدفم این است که در این مسیر رشد کنم و ارزش بیشتری برای تیم و پروژه ایجاد کنم.

این پاسخ:

- هدف دارد
- واقع‌بینانه است
- انعطاف‌پذیر است
- رشد فنی را نشان می‌دهد
- فقط روی عنوان شغلی تمرکز نمی‌کند

---

# نکات مهم برای پاسخ دادن در مصاحبه

## فقط اسم تکنولوژی را نگو

اگر گفتی:

> PostgreSQL بلدم.

باید بتوانی درباره Index، Execution Plan، Join، Transaction و Query Optimization صحبت کنی.

اگر گفتی:

> Django ORM بلدم.

باید N+1، `select_related`، `prefetch_related` و زمان اجرای Query را بفهمی.

اگر گفتی:

> Cache کار کردم.

باید درباره Cache Key، TTL و Invalidation توضیح بدهی.

---

## جواب را با تجربه واقعی قوی کن

به جای:

> N+1 را می‌شناسم.

بهتر است بگویی:

> در یک API لیست متوجه شدم برای هر Product یک Query جدا برای Category اجرا می‌شود. Queryها را بررسی کردم و متوجه N+1 شدم. سپس با `select_related` Relation را همراه Query اصلی دریافت کردم و تعداد Queryها را کاهش دادم.

---

## Performance را با عدد ثابت تعریف نکن

نگو:

> `select_related` همیشه Query را به یک Query تبدیل می‌کند.

بهتر است بگویی:

> در سناریوی مناسب می‌تواند Queryهای اضافی مربوط به Relationهای قابل Join را حذف کند و اطلاعات را با JOIN دریافت کند.

---

## Cache را فقط برای سریع شدن استفاده نکن

قبل از Cache این موارد را بررسی کن:

```
چه چیزی کند است؟
چرا کند است؟
چقدر خوانده می‌شود؟
چقدر تغییر می‌کند؟
Cache چه هزینه‌ای دارد؟
Invalidation چگونه انجام می‌شود؟
```

---

## Signal را بیش از حد استفاده نکن

Signal ابزار خوبی است، اما اگر Business Logic زیادی داخل آن قرار بگیرد، Debug و Maintenance سخت می‌شود.

منطق اصلی Business بهتر است در ساختار مشخصی مثل Service Layer قرار بگیرد و Signal برای Eventهای جانبی و مشخص استفاده شود.

---

# جمع‌بندی

یک Backend Developer حرفه‌ای Django فقط Syntax جنگو را نمی‌داند.

باید بتواند ارتباط بین این بخش‌ها را درک کند:

```
Django
  ↓
ORM
  ↓
SQL
  ↓
PostgreSQL
  ↓
Index
  ↓
EXPLAIN ANALYZE
  ↓
Query Optimization
  ↓
Caching
  ↓
Transactions
  ↓
Signals
  ↓
Architecture
  ↓
Performance
  ↓
Scalability
```

هدف مصاحبه‌گر معمولاً این نیست که فقط ببیند چند دستور را حفظ کرده‌ای.

می‌خواهد بفهمد وقتی سیستم کند شد، Query زیاد شد، Cache قدیمی شد، Transaction شکست خورد یا Architecture پیچیده شد، آیا می‌توانی مسئله را تحلیل کنی و راه‌حل درست ارائه بدهی یا نه.

> **مهم‌ترین اصل:** در مصاحبه فقط نگو «بلدم». توضیح بده «چرا»، «چه زمانی»، «چطور» و «چه Trade-offهایی» دارد.

---

<div align="left">

**سازنده:** معین رضایی  
**ایمیل:** moeinrezaie516@gmail.com

</div>

