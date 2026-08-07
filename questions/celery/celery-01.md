<div dir="rtl">

# ۳۰ سؤال تخصصی `Celery` برای مصاحبه `Backend`

این فایل شامل ۳۰ سؤال سطح بالا در مصاحبه‌های شغلی `Backend` با تمرکز بر `Celery` و کاربرد آن در `Django`، `DRF`، `FastAPI`، `Docker` و `Docker Compose` است.

پاسخ‌ها کوتاه، دقیق و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

## مفاهیم پایه `Celery`

### ۱. `Celery` چیست و چه مشکلی را حل می‌کند؟

**پاسخ:**

`Celery` یک `Distributed Task Queue` است.

وظایف زمان‌بر را به‌صورت `Asynchronous` اجرا می‌کند.

از `Blocking` شدن `Request` در `Web Server` جلوگیری می‌کند.

مثلاً ارسال ایمیل، پردازش تصویر و تولید گزارش در پس‌زمینه انجام می‌شود.

---

### ۲. معماری `Celery` چگونه است؟

**پاسخ:**

`Producer` وظیفه را ایجاد و به `Broker` ارسال می‌کند.

`Broker` وظیفه را در صف ذخیره می‌کند.

`Worker` وظیفه را از `Broker` دریافت و اجرا می‌کند.

`Backend` نتیجه وظیفه را ذخیره می‌کند.

---

### ۳. `Broker` در `Celery` چیست؟

**پاسخ:**

`Broker` واسط بین `Producer` و `Worker` است.

وظایف را در صف نگه می‌دارد.

`RabbitMQ` و `Redis` رایج‌ترین `Broker`ها هستند.

`RabbitMQ` برای `Production` با `Message Acknowledgment` مناسب‌تر است.

---

### ۴. تفاوت `RabbitMQ` و `Redis` به‌عنوان `Broker` چیست؟

**پاسخ:**

`RabbitMQ` یک `Message Broker` کامل است.

از `Message Acknowledgment` و `Persistence` پشتیبانی می‌کند.

`Redis` سریع‌تر است اما `Message`ها در حافظه هستند.

در صورت کرش `Redis`، احتمال از دست رفتن `Message` وجود دارد.

برای `Production` حساس، `RabbitMQ` توصیه می‌شود.

---

### ۵. `Result Backend` در `Celery` چیست؟

**پاسخ:**

نتیجه اجرای `Task` را ذخیره می‌کند.

امکان بررسی وضعیت `Task` را فراهم می‌کند.

`Redis`، `PostgreSQL` و `RabbitMQ` قابل استفاده هستند.

اگر نیازی به نتیجه نیست، می‌توان غیرفعال کرد.

---

## `Task` و وضعیت‌ها

### ۶. وضعیت‌های `Task` در `Celery` کدامند؟

**پاسخ:**

`PENDING` وظیفه در صف منتظر است.

`STARTED` وظیفه شروع به اجرا شده است.

`SUCCESS` وظیفه با موفقیت انجام شده است.

`FAILURE` وظیفه با خطا مواجه شده است.

`RETRY` وظیفه برای تلاش مجدد در صف قرار گرفته است.

---

### ۷. چگونه یک `Task` ساده در `Celery` تعریف می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from celery import Celery

app = Celery('myapp', broker='redis://localhost:6379/0')

@app.task
def add(x, y):
    return x + y
```

</div>

---

### ۸. تفاوت `apply_async()` و `delay()` چیست؟

**پاسخ:**

`delay()` یک میانبر ساده است.

`apply_async()` کنترل بیشتری فراهم می‌کند.

با `apply_async()` می‌توان `countdown`، `eta` و `queue` را مشخص کرد.

<div dir="ltr">

```python
add.delay(4, 6)

add.apply_async(
    args=[4, 6],
    countdown=60,
    queue='high_priority'
)
```

</div>

---

### ۹. `countdown` و `eta` چه تفاوتی دارند؟

**پاسخ:**

`countdown` تأخیر به ثانیه از زمان ارسال است.

`eta` زمان دقیق اجرا را مشخص می‌کند.

`eta` باید یک `datetime` باشد.

`countdown` برای تأخیرهای نسبی مناسب‌تر است.

---

### ۱۰. `Task Expiration` چیست؟

**پاسخ:**

اگر `Task` در زمان مشخص‌شده اجرا نشود، منقضی می‌شود.

با `expires` در `apply_async()` تنظیم می‌شود.

با `task_expires` در تنظیمات سراسری تنظیم می‌شود.

از اجرای `Task`های قدیمی و بی‌معنا جلوگیری می‌کند.

---

## `Retry` و مدیریت خطا

### ۱۱. چگونه `Retry` را در `Celery` پیاده‌سازی می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
@app.task(bind=True, max_retries=3, default_retry_delay=60)
def send_email(self, user_id):
    try:
        result = external_api_call(user_id)
    except ConnectionError as exc:
        raise self.retry(exc=exc)
```

</div>

---

### ۱۲. `Exponential Backoff` در `Celery` چیست؟

**پاسخ:**

در هر بار `Retry`، زمان انتظار به‌صورت تصاعدی افزایش می‌یابد.

با `autoretry_for` و `retry_backoff` فعال می‌شود.

از فشار بر سرویس خارجی جلوگیری می‌کند.

<div dir="ltr">

```python
@app.task(
    autoretry_for=(ConnectionError,),
    retry_backoff=True,
    max_retries=5
)
def call_external_api(url):
    return requests.get(url)
```

</div>

---

### ۱۳. `acks_late` چیست و چرا مهم است؟

**پاسخ:**

به‌صورت پیش‌فرض، `Task` قبل از اجرا `Acknowledge` می‌شود.

با `acks_late=True`، پس از اجرای موفق `Acknowledge` می‌شود.

در صورت کرش `Worker`، `Task` دوباره اجرا می‌شود.

برای `Task`های حساس ضروری است.

---

### ۱۴. `Idempotency` در `Task`های `Celery` چیست؟

**پاسخ:**

اجرای مجدد یک `Task` نباید نتیجه متفاوتی ایجاد کند.

در صورت `Retry` یا `Redelivery`، داده‌ها تکراری نمی‌شوند.

با بررسی `unique constraint` یا `cache` پیاده‌سازی می‌شود.

---

## `Celery Canvas`

### ۱۵. `chain` در `Celery` چیست؟

**پاسخ:**

`Task`ها به‌صورت متوالی اجرا می‌شوند.

خروجی هر `Task` به `Task` بعدی ارسال می‌شود.

<div dir="ltr">

```python
from celery import chain

workflow = chain(step1.s(), step2.s(), step3.s())
workflow.apply_async()
```

</div>

---

### ۱۶. `group` در `Celery` چیست؟

**پاسخ:**

چند `Task` به‌صورت موازی اجرا می‌شوند.

نتایج به‌صورت یک لیست برگردانده می‌شود.

<div dir="ltr">

```python
from celery import group

workflow = group(process_item.s(i) for i in range(10))
workflow.apply_async()
```

</div>

---

### ۱۷. `chord` در `Celery` چیست؟

**پاسخ:**

ترکیب `group` و یک `Callback` است.

ابتدا تمام `Task`های `group` اجرا می‌شوند.

سپس `Callback` با نتایج `group` اجرا می‌شود.

<div dir="ltr">

```python
from celery import chord

workflow = chord(
    [fetch_data.s(i) for i in range(10)],
    aggregate_results.s()
)
workflow.apply_async()
```

</div>

---

## `Celery` در `Django` و `DRF`

### ۱۸. چگونه `Celery` را در `Django` پیکربندی می‌کنید؟

**پاسخ:**

فایل `celery.py` در کنار `settings.py` ایجاد می‌شود.

<div dir="ltr">

```python
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')

app = Celery('myproject')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```

</div>

---

### ۱۹. `autodiscover_tasks()` چه کاری انجام می‌دهد؟

**پاسخ:**

به‌صورت خودکار فایل `tasks.py` را در هر `App` پیدا می‌کند.

نیازی به `Import` دستی `Task`ها نیست.

ساختار پروژه تمیزتر باقی می‌ماند.

---

### ۲۰. در `DRF` چه زمانی از `Celery` استفاده می‌کنید؟

**پاسخ:**

ارسال ایمیل یا `SMS` پس از ثبت‌نام.

پردازش فایل‌های آپلودشده.

تولید گزارش‌های سنگین.

`Sync` با سرویس‌های خارجی.

هر عملیاتی که بیش از ۱-۲ ثانیه زمان می‌برد.

---

### ۲۱. چگونه وضعیت `Task` را به `Client` برمی‌گردانید؟

**پاسخ:**

`Task ID` در `Response` اولیه برگردانده می‌شود.

`Client` با `Task ID` وضعیت را بررسی می‌کند.

<div dir="ltr">

```python
@app.task
def generate_report(report_id):
    report = Report.objects.get(id=report_id)
    report.process()
    return report.id

@api_view(['POST'])
def create_report(request):
    task = generate_report.delay(report_id)
    return Response({'task_id': task.id}, status=202)

@api_view(['GET'])
def check_status(request, task_id):
    result = AsyncResult(task_id)
    return Response({'status': result.state})
```

</div>

---

## `Celery` در `FastAPI`

### ۲۲. چگونه `Celery` را در `FastAPI` استفاده می‌کنید؟

**پاسخ:**

`Celery App` به‌صورت جداگانه تعریف می‌شود.

`Task`ها در ماژول جداگانه قرار می‌گیرند.

در `Endpoint`ها با `delay()` فراخوانی می‌شوند.

<div dir="ltr">

```python
from fastapi import FastAPI, BackgroundTasks
from celery_app import send_email_task

app = FastAPI()

@app.post("/send-email")
async def send_email(email: str):
    task = send_email_task.delay(email)
    return {"task_id": task.id, "status": "queued"}
```

</div>

---

### ۲۳. تفاوت `Celery` و `BackgroundTasks` در `FastAPI` چیست؟

**پاسخ:**

`BackgroundTasks` در همان پروسه اجرا می‌شود.

در صورت کرش سرور، وظیفه از بین می‌رود.

`Celery` در پروسه جداگانه اجرا می‌شود.

`Celery` قابلیت `Retry` و `Monitoring` دارد.

برای وظایف حساس، `Celery` ضروری است.

---

## `Celery Worker`

### ۲۴. `Concurrency` در `Celery Worker` چیست؟

**پاسخ:**

تعداد `Task`هایی که همزمان اجرا می‌شوند.

`prefork` پیش‌فرض است و از پروسه‌ها استفاده می‌کند.

`eventlet` و `gevent` از `Coroutine`ها استفاده می‌کنند.

برای `I/O-Bound`، `gevent` بهینه‌تر است.

برای `CPU-Bound`، `prefork` مناسب‌تر است.

---

### ۲۵. `Prefork` و `Gevent` چه تفاوتی دارند؟

**پاسخ:**

`Prefork` چند پروسه مستقل ایجاد می‌کند.

هر پروسه `Memory` جداگانه دارد.

`Gevent` از `Green Threads` استفاده می‌کند.

`Gevent` مصرف `Memory` کمتری دارد.

`Gevent` برای هزاران `Task` همزمان `I/O-Bound` مناسب است.

---

### ۲۶. `Celery Beat` چیست؟

**پاسخ:**

زمان‌بند دوره‌ای برای اجرای `Task`ها است.

مشابه `Cron Job` عمل می‌کند.

تنظیمات در `CELERY_BEAT_SCHEDULE` تعریف می‌شود.

<div dir="ltr">

```python
CELERY_BEAT_SCHEDULE = {
    'cleanup-every-midnight': {
        'task': 'myapp.tasks.cleanup_old_data',
        'schedule': crontab(hour=0, minute=0),
    },
}
```

</div>

---

## `Docker` و `Docker Compose`

### ۲۷. چگونه `Celery Worker` را در `Docker` اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["celery", "-A", "myproject", "worker", "--loglevel=info", "--concurrency=4"]
```

</div>

---

### ۲۸. چگونه `Celery` را در `Docker Compose` پیکربندی می‌کنید؟

**پاسخ:**

<div dir="ltr">

```yaml
services:
  redis:
    image: redis:7-alpine

  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    depends_on:
      - redis
      - celery_worker

  celery_worker:
    build: .
    command: celery -A myproject worker --loglevel=info
    depends_on:
      - redis
    environment:
      - CELERY_BROKER_URL=redis://redis:6379/0

  celery_beat:
    build: .
    command: celery -A myproject beat --loglevel=info
    depends_on:
      - redis
    environment:
      - CELERY_BROKER_URL=redis://redis:6379/0
```

</div>

---

### ۲۹. چگونه `Celery Worker` را `Scale` می‌کنید؟

**پاسخ:**

در `Docker Compose` با `docker compose up --scale celery_worker=5` انجام می‌شود.

در `Kubernetes` با `ReplicaSet` یا `Deployment` انجام می‌شود.

`Broker` باید توانایی مدیریت `Connection`های متعدد را داشته باشد.

`Result Backend` باید `Concurrent Access` را پشتیبانی کند.

---

## `Monitoring` و `Production`

### ۳۰. چگونه `Celery` را در `Production` مانیتور می‌کنید؟

**پاسخ:**

`Flower` یک داشبورد `Web` برای `Celery` است.

`Prometheus` و `Grafana` برای متریک‌ها استفاده می‌شوند.

`task_success_total` و `task_failure_total` ردیابی می‌شوند.

`worker_up` و `worker_down` برای `Alerting` استفاده می‌شوند.

`Sentry` برای ردیابی خطاهای `Task` استفاده می‌شود.

---

## جمع‌بندی نکات کلیدی

<div dir="ltr">

| موضوع | نکته کلیدی |
|---|---|
| `Broker` | `RabbitMQ` برای `Production`، `Redis` برای سادگی |
| `Backend` | `Redis` برای سرعت، `PostgreSQL` برای `Persistence` |
| `Retry` | `Exponential Backoff` و `max_retries` تنظیم شود |
| `acks_late` | برای `Task`های حساس فعال شود |
| `Idempotency` | اجرای مجدد نباید تکراری ایجاد کند |
| `Worker` | `prefork` برای `CPU`، `gevent` برای `I/O` |
| `Beat` | فقط یک `Instance` در `Production` اجرا شود |
| `Docker` | `Worker` و `Beat` در `Container` جداگانه |
| `Scale` | با `--scale` یا `ReplicaSet` |
| `Monitor` | `Flower` + `Prometheus` + `Sentry` |

</div>

</div>
