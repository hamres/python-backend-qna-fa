<div dir="rtl">

# ۳۰ سؤال تخصصی `Docker` و `Docker Compose` برای مصاحبه `Backend`

این فایل شامل ۳۰ سؤال سطح بالا در مصاحبه‌های شغلی `Backend` با تمرکز بر `Docker` و `Docker Compose` است.

پاسخ‌ها کوتاه، مستقیم و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

## مفاهیم پایه `Docker`

### ۱. تفاوت `Docker Image` و `Docker Container` چیست؟

**پاسخ:**

`Docker Image` یک قالب فقط‌خواندنی است.

شامل کد، کتابخانه‌ها و تنظیمات است.

`Docker Container` یک `Instance` در حال اجرا از `Image` است.

`Container` قابل تغییر است و `Image` قابل تغییر نیست.

---

### ۲. `Docker` چگونه با `Virtual Machine` تفاوت دارد؟

**پاسخ:**

`Virtual Machine` یک `OS` کامل را شبیه‌سازی می‌کند.

`Docker Container` فقط پروسه‌ها را ایزوله می‌کند.

`Container`ها سبک‌تر و سریع‌تر هستند.

`Container`ها `Kernel` میزبان را به اشتراک می‌گذارند.

---

### ۳. `Docker Layer` چیست؟

**پاسخ:**

هر دستور در `Dockerfile` یک `Layer` ایجاد می‌کند.

`Layer`ها به‌صورت `Read-Only` هستند.

`Layer`های مشترک بین `Image`ها به اشتراک گذاشته می‌شوند.

این مکانیزم باعث کاهش حجم و افزایش سرعت می‌شود.

---

### ۴. `Docker Registry` چیست؟

**پاسخ:**

`Docker Registry` محل ذخیره‌سازی `Image`ها است.

`Docker Hub` یک `Registry` عمومی است.

`Private Registry` برای سازمان‌ها استفاده می‌شود.

`Image`ها با `Push` و `Pull` مدیریت می‌شوند.

---

### ۵. `Dockerfile` چیست و چه دستوراتی دارد؟

**پاسخ:**

`Dockerfile` فایل متنی برای ساخت `Image` است.

`FROM` تصویر پایه را مشخص می‌کند.

`RUN` دستورات را اجرا می‌کند.

`COPY` و `ADD` فایل‌ها را کپی می‌کنند.

`CMD` دستور پیش‌فرض `Container` را مشخص می‌کند.

`EXPOSE` پورت را مستند می‌کند.

---

## `Dockerfile` بهینه‌سازی

### ۶. `Multi-Stage Build` چیست و چرا استفاده می‌شود؟

**پاسخ:**

`Multi-Stage Build` چند مرحله ساخت دارد.

مرحله `Build` شامل ابزارهای توسعه است.

مرحله `Production` فقط فایل‌های نهایی را شامل می‌شود.

حجم `Image` نهایی به‌شدت کاهش می‌یابد.

<div dir="ltr">

```dockerfile
FROM python:3.12 AS builder
COPY requirements.txt .
RUN pip install --target=/app/packages -r requirements.txt

FROM python:3.12-slim
COPY --from=builder /app/packages /usr/local/lib/python3.12/site-packages
COPY . .
CMD ["python", "manage.py", "runserver"]
```

</div>

---

### ۷. چگونه `Docker Image` را بهینه می‌کنید؟

**پاسخ:**

از `Image`های `slim` یا `alpine` استفاده می‌شود.

دستورات `RUN` ترکیب می‌شوند تا `Layer` کمتری ایجاد شود.

از `Multi-Stage Build` استفاده می‌شود.

فایل‌های غیرضروری در `.dockerignore` قرار می‌گیرند.

---

### ۸. `.dockerignore` چیست و چرا مهم است؟

**پاسخ:**

مشابه `.gitignore` عمل می‌کند.

فایل‌های غیرضروری را از `Build Context` حذف می‌کند.

سرعت `Build` را افزایش می‌دهد.

از ورود فایل‌های حساس به `Image` جلوگیری می‌کند.

---

### ۹. تفاوت `CMD` و `ENTRYPOINT` چیست؟

**پاسخ:**

`ENTRYPOINT` دستور اصلی `Container` را مشخص می‌کند.

`CMD` پارامترهای پیش‌فرض برای `ENTRYPOINT` را مشخص می‌کند.

`CMD` با `docker run` قابل بازنویسی است.

`ENTRYPOINT` بدون `--entrypoint` قابل بازنویسی نیست.

---

### ۱۰. تفاوت `COPY` و `ADD` چیست؟

**پاسخ:**

`COPY` فایل‌ها را از مبدا به مقصد کپی می‌کند.

`ADD` علاوه بر کپی، قابلیت استخراج `Archive` را دارد.

`ADD` می‌تواند از `URL` دانلود کند.

برای سادگی و شفافیت، `COPY` ترجیح داده می‌شود.

---

## `Docker` Networking

### ۱۱. `Docker Network`های پیش‌فرض کدامند؟

**پاسخ:**

`bridge` شبکه پیش‌فرض است.

`host` شبکه میزبان را مستقیماً استفاده می‌کند.

`none` هیچ شبکه‌ای ندارد.

`overlay` برای `Swarm` و `Multi-Host` استفاده می‌شود.

---

### ۱۲. `Container`ها چگونه با هم ارتباط برقرار می‌کنند؟

**پاسخ:**

در یک `Network` مشترک، با نام `Service` قابل دسترسی هستند.

`Docker DNS` نام `Container`ها را به `IP` تبدیل می‌کند.

در `Docker Compose`، نام `Service` به‌عنوان `Hostname` استفاده می‌شود.

---

### ۱۳. تفاوت `Publish Port` و `Expose Port` چیست؟

**پاسخ:**

`EXPOSE` فقط مستندسازی است.

`Publish Port` با `-p` پورت را به میزبان متصل می‌کند.

بدون `Publish`، پورت از بیرون قابل دسترسی نیست.

---

## `Docker Volumes`

### ۱۴. `Docker Volume` چیست؟

**پاسخ:**

`Volume` برای ذخیره‌سازی دائمی داده‌ها استفاده می‌شود.

داده‌ها پس از حذف `Container` باقی می‌مانند.

`Volume`ها توسط `Docker` مدیریت می‌شوند.

برای `Database` و فایل‌های `Media` ضروری هستند.

---

### ۱۵. تفاوت `Volume` و `Bind Mount` چیست؟

**پاسخ:**

`Volume` توسط `Docker` مدیریت می‌شود.

`Bind Mount` یک مسیر مشخص در میزبان را متصل می‌کند.

`Bind Mount` برای `Development` مناسب است.

`Volume` برای `Production` مناسب‌تر است.

---

### ۱۶. چگونه `Volume` را در `Docker Compose` تعریف می‌کنید؟

**پاسخ:**

در بخش `volumes` سطح بالا تعریف می‌شود.

در هر `Service` به آن ارجاع داده می‌شود.

<div dir="ltr">

```yaml
services:
  db:
    image: postgres:16
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

</div>

---

## `Docker Compose` پایه

### ۱۷. `Docker Compose` چیست؟

**پاسخ:**

ابزاری برای تعریف و اجرای اپلیکیشن‌های `Multi-Container` است.

تنظیمات در فایل `docker-compose.yml` نوشته می‌شود.

با یک دستور تمام `Service`ها اجرا می‌شوند.

برای `Development` و `Testing` بسیار مناسب است.

---

### ۱۸. ساختار فایل `docker-compose.yml` چگونه است؟

**پاسخ:**

`version` نسخه فرمت را مشخص می‌کند.

`services` لیست `Container`ها را تعریف می‌کند.

`networks` شبکه‌های سفارشی را تعریف می‌کند.

`volumes` حجم‌های مشترک را تعریف می‌کند.

---

### ۱۹. چگونه یک `Service` در `Docker Compose` تعریف می‌کنید؟

**پاسخ:**

<div dir="ltr">

```yaml
services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DEBUG=True
    depends_on:
      - db
```

</div>

---

### ۲۰. `depends_on` چه کاری انجام می‌دهد؟

**پاسخ:**

ترتیب شروع `Service`ها را مشخص می‌کند.

`Service` وابسته پس از `Service` مورد نظر شروع می‌شود.

اما تضمین نمی‌کند `Service` مورد نظر آماده باشد.

برای بررسی آمادگی از `healthcheck` استفاده می‌شود.

---

## `Docker Compose` پیشرفته

### ۲۱. `healthcheck` در `Docker Compose` چیست؟

**پاسخ:**

بررسی می‌کند `Service` آماده است یا خیر.

`depends_on` با `condition: service_healthy` از این بررسی استفاده می‌کند.

<div dir="ltr">

```yaml
services:
  db:
    image: postgres:16
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
```

</div>

---

### ۲۲. `restart policy` در `Docker Compose` چیست؟

**پاسخ:**

مشخص می‌کند `Container` پس از کرش چه رفتاری داشته باشد.

`no` پیش‌فرض است و `Restart` نمی‌شود.

`always` همیشه `Restart` می‌شود.

`on-failure` فقط در صورت خطا `Restart` می‌شود.

`unless-stopped` همیشه `Restart` می‌شود مگر دستی متوقف شود.

---

### ۲۳. `Docker Compose Profiles` چیست؟

**پاسخ:**

امکان گروه‌بندی `Service`ها را فراهم می‌کند.

فقط `Service`های `Profile` فعال اجرا می‌شوند.

برای محیط‌های مختلف مانند `Development` و `Testing` مناسب است.

<div dir="ltr">

```yaml
services:
  debug-tools:
    image: busybox
    profiles: ["debug"]
```

</div>

---

### ۲۴. `docker-compose.override.yml` چیست؟

**پاسخ:**

تنظیمات اضافی برای محیط `Development` است.

به‌صورت خودکار با `docker-compose.yml` ادغام می‌شود.

برای `Debug Port` و `Volume`های محلی استفاده می‌شود.

در `Git` معمولاً `Commit` نمی‌شود.

---

### ۲۵. چگونه `Environment Variables` را در `Docker Compose` مدیریت می‌کنید؟

**پاسخ:**

مستقیماً در `environment` تعریف می‌شوند.

از فایل `.env` خوانده می‌شوند.

از `env_file` برای خواندن فایل جداگانه استفاده می‌شود.

<div dir="ltr">

```yaml
services:
  web:
    env_file:
      - .env
    environment:
      - DATABASE_URL=postgres://db:5432/app
```

</div>

---

## `Docker` برای `Django` و `PostgreSQL`

### ۲۶. چگونه `Django` را در `Docker` اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

</div>

---

### ۲۷. چگونه `PostgreSQL` را در `Docker Compose` پیکربندی می‌کنید؟

**پاسخ:**

<div dir="ltr">

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

</div>

---

### ۲۸. چگونه `Redis` را در `Docker Compose` پیکربندی می‌کنید؟

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

volumes:
  redis_data:
```

</div>

---

## `Security` و `Best Practices`

### ۲۹. چگونه `Docker Image` را امن می‌کنید؟

**پاسخ:**

از `Image`های رسمی و به‌روز استفاده می‌شود.

`Secret`ها در `Image` ذخیره نمی‌شوند.

از `Environment Variables` یا `Secret Manager` استفاده می‌شود.

`Container` به‌عنوان `root` اجرا نمی‌شود.

`Image`ها با ابزارهایی مانند `Trivy` اسکن می‌شوند.

---

### ۳۰. بهترین شیوه‌های `Docker` برای `Production` چیست؟

**پاسخ:**

از `Multi-Stage Build` استفاده شود.

`Image`ها با `Tag` مشخص نسخه‌گذاری شوند.

`Health Check` تعریف شود.

`Restart Policy` مناسب تنظیم شود.

`Logs` به `Centralized Logging` ارسال شوند.

`Resource Limits` با `--memory` و `--cpus` تنظیم شوند.

از `Read-Only Filesystem` استفاده شود.

---

## جمع‌بندی نکات کلیدی

| موضوع | نکته کلیدی |
|---|---|
| `Image` | قالب فقط‌خواندنی |
| `Container` | `Instance` در حال اجرا |
| `Layer` | هر دستور یک `Layer` |
| `Multi-Stage Build` | کاهش حجم `Image` |
| `Volume` | ذخیره‌سازی دائمی |
| `depends_on` | ترتیب شروع، نه آمادگی |
| `healthcheck` | بررسی آمادگی `Service` |
| `restart` | `always` یا `unless-stopped` |
| `.env` | مدیریت `Secret`ها |
| `Security` | بدون `root`، بدون `Secret` در `Image` |

</div>
