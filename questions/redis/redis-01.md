<div dir="rtl">

# ۳۰ سؤال تخصصی Redis و Caching برای مصاحبه Backend

این فایل شامل ۳۰ سؤال سطح بالا در مصاحبه‌های شغلی Backend با تمرکز بر Redis، استراتژی‌های Caching و کاربرد آن در Django، DRF، FastAPI و Docker است.

پاسخ‌ها کوتاه، دقیق و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

## مفاهیم پایه Redis

### ۱. Redis چیست و چه تفاوتی با Database رابطه‌ای دارد؟

**پاسخ:**

Redis یک In-Memory Data Store است.

داده‌ها در Memory ذخیره می‌شوند و دسترسی بسیار سریع است.

Database رابطه‌ای داده‌ها را روی Disk ذخیره می‌کند.

Redis برای Cache، Session و Message Queue استفاده می‌شود.

Redis جایگزین Database اصلی نیست.

---

### ۲. ساختارهای داده Redis کدامند؟

**پاسخ:**

String برای مقادیر ساده و Cache استفاده می‌شود.

Hash برای ذخیره Object با چند Field استفاده می‌شود.

List برای صف‌ها و لیست‌های ترتیبی استفاده می‌شود.

Set برای مقادیر یکتا بدون ترتیب استفاده می‌شود.

Sorted Set برای مقادیر یکتا با امتیاز و ترتیب استفاده می‌شود.

Stream برای Message Queue و Event Log استفاده می‌شود.

---

### ۳. Redis چگونه به Performance کمک می‌کند؟

**پاسخ:**

داده‌های پرتکرار در Memory ذخیره می‌شوند.

تعداد کوئری‌ها به Database کاهش می‌یابد.

زمان پاسخ از میلی‌ثانیه به میکروثانیه کاهش می‌یابد.

فشار بر Database اصلی کم می‌شود.

---

### ۴. Redis Persistence چگونه کار می‌کند؟

**پاسخ:**

RDB یک Snapshot از داده‌ها در زمان مشخص ایجاد می‌کند.

AOF هر عملیات نوشتن را در یک فایل لاگ ثبت می‌کند.

RDB سریع‌تر است اما داده‌های اخیر ممکن است از بین بروند.

AOF امن‌تر است اما فایل بزرگ‌تری تولید می‌کند.

در Production معمولاً هر دو فعال هستند.

---

### ۵. Redis Eviction Policy چیست؟

**پاسخ:**

وقتی Memory پر می‌شود، Redis باید داده‌های قدیمی را حذف کند.

allkeys-lru کمترین استفاده اخیر را حذف می‌کند.

volatile-lru فقط کلیدهایی با TTL را حذف می‌کند.

volatile-ttl کلیدهایی با کمترین TTL باقی‌مانده را حذف می‌کند.

noeviction خطا برمی‌گرداند و داده‌ای حذف نمی‌شود.

---

## استراتژی‌های Caching

### ۶. Cache-Aside Pattern چیست؟

**پاسخ:**

Application ابتدا Cache را بررسی می‌کند.

در صورت وجود، داده از Cache خوانده می‌شود.

در صورت عدم وجود، داده از Database خوانده می‌شود.

سپس داده در Cache ذخیره می‌شود.

رایج‌ترین الگوی Caching است.

---

### ۷. Write-Through Pattern چیست؟

**پاسخ:**

داده همزمان در Cache و Database نوشته می‌شود.

Cache همیشه به‌روز است.

عملیات نوشتن کندتر است.

خواندن همیشه سریع است.

---

### ۸. Write-Behind Pattern چیست؟

**پاسخ:**

داده ابتدا در Cache نوشته می‌شود.

سپس به‌صورت Asynchronous در Database نوشته می‌شود.

عملیات نوشتن بسیار سریع است.

در صورت کرش Cache، احتمال از دست رفتن داده وجود دارد.

---

### ۹. Cache Invalidation چیست و چرا سخت است؟

**پاسخ:**

Cache Invalidation حذف یا به‌روزرسانی داده‌های قدیمی در Cache است.

وقتی داده در Database تغییر می‌کند، Cache باید به‌روز شود.

در سیستم‌های Distributed هماهنگی سخت است.

Race Condition ممکن است رخ دهد.

---

### ۱۰. TTL در Redis چیست و چرا مهم است؟

**پاسخ:**

TTL مخفف Time To Live است.

مدت زمان باقی‌ماندن کلید در Redis را مشخص می‌کند.

پس از اتمام TTL، کلید به‌صورت خودکار حذف می‌شود.

از انباشته شدن داده‌های قدیمی جلوگیری می‌کند.

<div dir="ltr">

```python
import redis

r = redis.Redis()
r.setex('user:1:profile', 3600, profile_data)
```

</div>

---

### ۱۱. Cache Stampede یا Thundering Herd چیست؟

**پاسخ:**

وقتی یک کلید پرکاربرد منقضی می‌شود.

هزاران درخواست همزمان به Database مراجعه می‌کنند.

فشار ناگهانی بر Database وارد می‌شود.

با Lock یا Early Expiration جلوگیری می‌شود.

---

### ۱۲. چگونه از Cache Stampede جلوگیری می‌کنید؟

**پاسخ:**

از Distributed Lock استفاده می‌شود.

فقط یک درخواست داده را از Database می‌خواند.

سایر درخواست‌ها منتظر می‌مانند.

TTL با Random Jitter تنظیم می‌شود تا انقضای همزمان رخ ندهد.

---

## Redis در Django و DRF

### ۱۳. چگونه Redis را به‌عنوان Cache Backend در Django تنظیم می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
        'OPTIONS': {
            'CLIENT_CLASS': 'django_redis.client.DefaultClient',
        }
    }
}
```

</div>

---

### ۱۴. چگونه از Cache در Viewهای Django استفاده می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from django.core.cache import cache
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)
def product_list(request):
    products = Product.objects.all()
    return render(request, 'products.html', {'products': products})
```

</div>

---

### ۱۵. چگونه از Low-Level Cache API در Django استفاده می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from django.core.cache import cache

def get_user_profile(user_id):
    cache_key = f'user:{user_id}:profile'
    profile = cache.get(cache_key)

    if profile is None:
        profile = Profile.objects.get(user_id=user_id)
        cache.set(cache_key, profile, timeout=3600)

    return profile
```

</div>

---

### ۱۶. چگونه DRF Response را Cache می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from django.utils.decorators import method_decorator
from django.views.decorators.cache import cache_page

class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer

    @method_decorator(cache_page(60 * 5))
    def dispatch(self, *args, **kwargs):
        return super().dispatch(*args, **kwargs)
```

</div>

---

### ۱۷. چگونه Cache را در Serializer استفاده می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from django.core.cache import cache

class ProductSerializer(serializers.ModelSerializer):
    category_name = serializers.SerializerMethodField()

    def get_category_name(self, obj):
        cache_key = f'category:{obj.category_id}:name'
        name = cache.get(cache_key)

        if name is None:
            name = obj.category.name
            cache.set(cache_key, name, timeout=300)

        return name
```

</div>

---

### ۱۸. چگونه Session را در Redis ذخیره می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.cache'
SESSION_CACHE_ALIAS = 'default'
```

</div>

Sessionها در Redis ذخیره می‌شوند.

در Distributed Systems، Session بین سرورها به اشتراک گذاشته می‌شود.

---

## Redis در FastAPI

### ۱۹. چگونه Redis را در FastAPI استفاده می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
import redis.asyncio as redis
from fastapi import FastAPI

app = FastAPI()
redis_client = redis.from_url("redis://localhost:6379")

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    cache_key = f"user:{user_id}"
    cached = await redis_client.get(cache_key)

    if cached:
        return {"data": cached, "source": "cache"}

    user = await fetch_user_from_db(user_id)
    await redis_client.setex(cache_key, 3600, user.json())
    return {"data": user, "source": "database"}
```

</div>

---

### ۲۰. چگونه Redis Connection Pool را در FastAPI مدیریت می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.redis = redis.from_url(
        "redis://localhost:6379",
        max_connections=50
    )
    yield
    await app.state.redis.close()

app = FastAPI(lifespan=lifespan)
```

</div>

---

## Redis و Celery

### ۲۱. چگونه Redis را به‌عنوان Broker برای Celery استفاده می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
CELERY_BROKER_URL = 'redis://localhost:6379/0'
CELERY_RESULT_BACKEND = 'redis://localhost:6379/1'
```

</div>

Redis هم Broker و هم Result Backend است.

برای Production حساس، RabbitMQ به‌عنوان Broker توصیه می‌شود.

---

### ۲۲. محدودیت Redis به‌عنوان Celery Broker چیست؟

**پاسخ:**

Redis در Memory ذخیره می‌کند.

در صورت کرش، Taskهای در صف ممکن است از بین بروند.

Message Acknowledgment به‌قدرت RabbitMQ نیست.

برای Taskهای بسیار حساس، RabbitMQ مناسب‌تر است.

---

## Docker و Docker Compose

### ۲۳. چگونه Redis را در Docker اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```bash
docker run -d \
  --name redis \
  -p 6379:6379 \
  -v redis_data:/data \
  redis:7-alpine \
  redis-server --appendonly yes
```

</div>

---

### ۲۴. چگونه Redis را در Docker Compose پیکربندی می‌کنید؟

**پاسخ:**

<div dir="ltr">

```yaml
services:
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    command: redis-server --appendonly yes --maxmemory 256mb --maxmemory-policy allkeys-lru
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  redis_data:
```

</div>

---

### ۲۵. چگونه Redis را با Password در Docker اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```yaml
services:
  redis:
    image: redis:7-alpine
    command: redis-server --requirepass mysecretpassword
```

</div>

در Application:

<div dir="ltr">

```python
REDIS_URL = 'redis://:mysecretpassword@redis:6379/0'
```

</div>

---

## مباحث پیشرفته

### ۲۶. Redis Pipeline چیست؟

**پاسخ:**

چند دستور را در یک درخواست به سرور ارسال می‌کند.

تعداد Round Tripها کاهش می‌یابد.

Performance به‌شدت افزایش می‌یابد.

<div dir="ltr">

```python
pipe = r.pipeline()
pipe.set('key1', 'value1')
pipe.set('key2', 'value2')
pipe.get('key1')
results = pipe.execute()
```

</div>

---

### ۲۷. Redis Pub/Sub چیست؟

**پاسخ:**

یک سیستم Publish/Subscribe است.

Publisher پیام را به یک Channel ارسال می‌کند.

Subscriberها پیام را دریافت می‌کنند.

برای Real-Time Notifications و Chat استفاده می‌شود.

پیام‌ها ذخیره نمی‌شوند و در صورت عدم حضور Subscriber از بین می‌روند.

---

### ۲۸. Redis Streams چیست و چه تفاوتی با Pub/Sub دارد؟

**پاسخ:**

Streams پیام‌ها را ذخیره می‌کنند.

Consumer Group امکان پردازش گروهی را فراهم می‌کند.

پیام‌های پردازش‌نشده باقی می‌مانند.

برای Event Sourcing و Message Queue مناسب است.

Pub/Sub پیام‌ها را ذخیره نمی‌کند.

---

### ۲۹. Distributed Lock با Redis چگونه پیاده‌سازی می‌شود؟

**پاسخ:**

از دستور SET با NX و PX استفاده می‌شود.

NX تضمین می‌کند فقط یک Client قفل را بگیرد.

PX زمان انقضای قفل را مشخص می‌کند.

<div dir="ltr">

```python
lock_acquired = r.set('lock:order:123', 'worker-1', nx=True, px=30000)

if lock_acquired:
    try:
        process_order(123)
    finally:
        r.delete('lock:order:123')
```

</div>

---

### ۳۰. Redis Sentinel و Redis Cluster چیست؟

**پاسخ:**

Redis Sentinel برای High Availability استفاده می‌شود.

در صورت کرش Master، یک Replica به‌صورت خودکار Master می‌شود.

Redis Cluster برای Horizontal Scaling استفاده می‌شود.

داده‌ها بین چند Node تقسیم می‌شوند.

Cluster برای داده‌های بسیار بزرگ استفاده می‌شود.

---

## جمع‌بندی نکات کلیدی

<div dir="ltr">

| موضوع | نکته کلیدی |
|---|---|
| Cache-Aside | رایج‌ترین الگو، ابتدا Cache سپس Database |
| TTL | همیشه تنظیم شود تا داده قدیمی نماند |
| Cache Stampede | با Lock یا Jitter جلوگیری شود |
| Eviction Policy | allkeys-lru برای Cache مناسب است |
| Persistence | AOF برای امنیت داده، RDB برای سرعت |
| Pipeline | برای عملیات دسته‌ای ضروری است |
| Connection Pool | در FastAPI و Django حتماً استفاده شود |
| Docker | appendonly yes و maxmemory تنظیم شود |
| Security | requirepass و bind محدود شود |
| Monitoring | redis-cli info و Redis Exporter استفاده شود |

</div>

</div>dir="ltr"` آماده شده است.
