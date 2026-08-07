<div dir="rtl">

# ۳۰ سؤال تخصصی `Testing` و ساختار پروژه برای مصاحبه `Backend`

این فایل شامل ۳۰ سؤال سطح بالا در مصاحبه‌های شغلی `Backend` با تمرکز بر `Testing`، ساختار پوشه‌بندی و کاربرد آن در `Django`، `DRF`، `FastAPI` و `Docker` است.

پاسخ‌ها کوتاه، دقیق و مناسب مرور سریع قبل از مصاحبه طراحی شده‌اند.

---

## مفاهیم پایه `Testing` در `Django`

### ۱. `Testing Framework`های رایج در `Django` کدامند؟

**پاسخ:**

`unittest` فریم‌ورک پیش‌فرض `Python` است.

`pytest` فریم‌ورک محبوب‌تر با `Syntax` ساده‌تر است.

`pytest-django` امکان استفاده از `pytest` در `Django` را فراهم می‌کند.

`Factory Boy` برای ایجاد داده‌های تست استفاده می‌شود.

---

### ۲. تفاوت `TestCase` و `TransactionTestCase` و `SimpleTestCase` چیست؟

**پاسخ:**

`TestCase` هر تست را در یک `Transaction` می‌پیچد و `Rollback` می‌کند.

`TestCase` برای تست‌هایی که با `Database` کار می‌کنند مناسب است.

`TransactionTestCase` جداول را `Truncate` می‌کند.

`TransactionTestCase` برای تست `Signals` و `Middleware` لازم است.

`SimpleTestCase` برای تست‌هایی بدون نیاز به `Database` است.

`SimpleTestCase` سریع‌تر است.

---

### ۳. چگونه یک `Model` را در `Django` تست می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from django.test import TestCase
from myapp.models import Product

class ProductModelTest(TestCase):
    def setUp(self):
        self.product = Product.objects.create(
            name='Laptop',
            price=1000
        )

    def test_product_creation(self):
        self.assertEqual(self.product.name, 'Laptop')
        self.assertEqual(Product.objects.count(), 1)

    def test_product_str(self):
        self.assertEqual(str(self.product), 'Laptop')
```

</div>

---

### ۴. چگونه یک `View` را در `Django` تست می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from django.test import TestCase, Client

class ProductViewTest(TestCase):
    def setUp(self):
        self.client = Client()

    def test_product_list_view(self):
        response = self.client.get('/products/')
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, 'products/list.html')

    def test_product_detail_view(self):
        product = Product.objects.create(name='Laptop', price=1000)
        response = self.client.get(f'/products/{product.id}/')
        self.assertEqual(response.status_code, 200)
```

</div>

---

### ۵. `Fixtures` چیست و چه معایبی دارد؟

**پاسخ:**

`Fixtures` فایل‌های `JSON` یا `YAML` با داده‌های اولیه هستند.

با `loaddata` بارگذاری می‌شوند.

نگهداری آن‌ها سخت است.

با تغییر `Model`، `Fixture`ها ممکن است خراب شوند.

`Factory Boy` جایگزین مدرن و انعطاف‌پذیرتر است.

---

### ۶. `Factory Boy` چیست و چگونه استفاده می‌شود؟

**پاسخ:**

<div dir="ltr">

```python
import factory
from myapp.models import Product

class ProductFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Product

    name = factory.Sequence(lambda n: f'Product {n}')
    price = factory.Faker('random_int', min=100, max=10000)
    is_active = True
```

</div>

<div dir="ltr">

```python
product = ProductFactory()
products = ProductFactory.create_batch(10)
```

</div>

---

## `Testing` در `DRF`

### ۷. تفاوت `APITestCase` و `TestCase` چیست؟

**پاسخ:**

`APITestCase` از `DRF` ارث‌بری می‌کند.

`APIClient` به‌جای `Client` استفاده می‌شود.

از `JSON` و فرمت‌های دیگر پشتیبانی می‌کند.

`Authentication` و `Permission`های `DRF` را شبیه‌سازی می‌کند.

---

### ۸. چگونه یک `API Endpoint` را در `DRF` تست می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from rest_framework.test import APITestCase
from rest_framework import status

class ProductAPITest(APITestCase):
    def test_list_products(self):
        response = self.client.get('/api/products/')
        self.assertEqual(response.status_code, status.HTTP_200_OK)

    def test_create_product_unauthenticated(self):
        data = {'name': 'Laptop', 'price': 1000}
        response = self.client.post('/api/products/', data)
        self.assertEqual(response.status_code, status.HTTP_401_UNAUTHORIZED)
```

</div>

---

### ۹. چگونه `Authentication` را در تست‌های `DRF` شبیه‌سازی می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from django.contrib.auth import get_user_model

User = get_user_model()

class AuthenticatedAPITest(APITestCase):
    def setUp(self):
        self.user = User.objects.create_user(
            email='test@example.com',
            password='testpass123'
        )
        self.client.force_authenticate(user=self.user)

    def test_authenticated_access(self):
        response = self.client.get('/api/profile/')
        self.assertEqual(response.status_code, 200)
```

</div>

---

### ۱۰. چگونه `Serializer` را به‌صورت مستقل تست می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
class ProductSerializerTest(TestCase):
    def test_valid_serializer(self):
        data = {'name': 'Laptop', 'price': 1000}
        serializer = ProductSerializer(data=data)
        self.assertTrue(serializer.is_valid())

    def test_invalid_serializer(self):
        data = {'name': '', 'price': -100}
        serializer = ProductSerializer(data=data)
        self.assertFalse(serializer.is_valid())
        self.assertIn('name', serializer.errors)
        self.assertIn('price', serializer.errors)
```

</div>

---

### ۱۱. چگونه `Permission`های `DRF` را تست می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
class PermissionTest(APITestCase):
    def setUp(self):
        self.admin = User.objects.create_superuser(
            email='admin@example.com',
            password='adminpass'
        )
        self.user = User.objects.create_user(
            email='user@example.com',
            password='userpass'
        )

    def test_admin_can_delete(self):
        self.client.force_authenticate(user=self.admin)
        response = self.client.delete('/api/products/1/')
        self.assertEqual(response.status_code, 204)

    def test_user_cannot_delete(self):
        self.client.force_authenticate(user=self.user)
        response = self.client.delete('/api/products/1/')
        self.assertEqual(response.status_code, 403)
```

</div>

---

### ۱۲. `Mock` و `Patch` در تست‌ها چیست؟

**پاسخ:**

`Mock` یک شیء جعلی به‌جای شیء واقعی ایجاد می‌کند.

`Patch` یک شیء واقعی را به‌صورت موقتی جایگزین می‌کند.

برای تست بدون وابستگی به سرویس‌های خارجی استفاده می‌شود.

<div dir="ltr">

```python
from unittest.mock import patch

class EmailTest(TestCase):
    @patch('myapp.services.send_email')
    def test_registration_sends_email(self, mock_send):
        self.client.post('/api/register/', {
            'email': 'new@example.com',
            'password': 'pass123'
        })
        mock_send.assert_called_once()
```

</div>

---

## `Testing` در `FastAPI`

### ۱۳. چگونه `FastAPI` را تست می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_read_products():
    response = client.get("/api/products/")
    assert response.status_code == 200

def test_create_product():
    response = client.post(
        "/api/products/",
        json={"name": "Laptop", "price": 1000}
    )
    assert response.status_code == 201
```

</div>

---

### ۱۴. چگونه تست‌های `Async` را در `FastAPI` اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
import pytest
from httpx import AsyncClient, ASGITransport
from main import app

@pytest.mark.anyio
async def test_read_products():
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as client:
        response = await client.get("/api/products/")
        assert response.status_code == 200
```

</div>

---

### ۱۵. چگونه `Dependency` را در تست‌های `FastAPI` جایگزین می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
from fastapi import Depends

def get_current_user():
    return {"id": 1, "email": "test@example.com"}

def override_get_current_user():
    return {"id": 999, "email": "admin@example.com"}

app.dependency_overrides[get_current_user] = override_get_current_user
```

</div>

---

### ۱۶. چگونه `Database` تست را در `FastAPI` مدیریت می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
import pytest
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

TEST_DATABASE_URL = "postgresql://test:test@localhost/test_db"
engine = create_engine(TEST_DATABASE_URL)
TestingSessionLocal = sessionmaker(bind=engine)

@pytest.fixture(autouse=True)
def setup_database():
    Base.metadata.create_all(bind=engine)
    yield
    Base.metadata.drop_all(bind=engine)
```

</div>

---

## ساختار پوشه‌بندی پروژه

### ۱۷. ساختار استاندارد پروژه `Django` چگونه است؟

**پاسخ:**

<div dir="ltr">

```text
myproject/
├── config/
│   ├── __init__.py
│   ├── settings/
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── development.py
│   │   └── production.py
│   ├── urls.py
│   ├── celery.py
│   └── wsgi.py
├── apps/
│   ├── users/
│   │   ├── models.py
│   │   ├── views.py
│   │   ├── serializers.py
│   │   ├── urls.py
│   │   ├── services.py
│   │   ├── tests/
│   │   │   ├── __init__.py
│   │   │   ├── test_models.py
│   │   │   ├── test_views.py
│   │   │   └── test_serializers.py
│   │   └── factories.py
│   └── products/
├── manage.py
├── requirements/
│   ├── base.txt
│   ├── development.txt
│   └── production.txt
├── docker-compose.yml
└── Dockerfile
```

</div>

---

### ۱۸. ساختار استاندارد پروژه `FastAPI` چگونه است؟

**پاسخ:**

<div dir="ltr">

```text
myproject/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   ├── security.py
│   │   └── database.py
│   ├── api/
│   │   ├── __init__.py
│   │   ├── deps.py
│   │   └── v1/
│   │       ├── __init__.py
│   │       ├── router.py
│   │       └── endpoints/
│   │           ├── users.py
│   │           └── products.py
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   └── product.py
│   ├── schemas/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   └── product.py
│   ├── services/
│   │   ├── __init__.py
│   │   ├── user_service.py
│   │   └── product_service.py
│   └── crud/
│       ├── __init__.py
│       ├── user_crud.py
│       └── product_crud.py
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   ├── test_users.py
│   └── test_products.py
├── alembic/
├── alembic.ini
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

</div>

---

### ۱۹. چرا `Settings` باید به چند فایل تقسیم شود؟

**پاسخ:**

`base.py` تنظیمات مشترک را شامل می‌شود.

`development.py` تنظیمات خاص توسعه را دارد.

`production.py` تنظیمات خاص محیط اصلی را دارد.

`Secret`ها در `Production` از `Environment Variables` خوانده می‌شوند.

با `DJANGO_SETTINGS_MODULE` فایل مناسب انتخاب می‌شود.

---

### ۲۰. `Service Layer` چیست و چرا استفاده می‌شود؟

**پاسخ:**

`Service Layer` منطق کسب‌وکار را از `View` جدا می‌کند.

`View` فقط `Request` را دریافت و `Response` برمی‌گرداند.

`Service` منطق اصلی را اجرا می‌کند.

تست‌نویسی ساده‌تر می‌شود.

کد قابل استفاده مجدد می‌شود.

<div dir="ltr">

```python
class UserService:
    def create_user(self, email: str, password: str):
        user = User.objects.create_user(email=email, password=password)
        send_welcome_email.delay(user.id)
        return user
```

</div>

---

## `Docker` و `Testing`

### ۲۱. چگونه تست‌ها را در `Docker` اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements/development.txt .
RUN pip install --no-cache-dir -r development.txt

COPY . .

CMD ["pytest", "--cov=.", "--cov-report=term-missing"]
```

</div>

<div dir="ltr">

```bash
docker build -t myapp-test .
docker run myapp-test
```

</div>

---

### ۲۲. چگونه تست‌ها را در `Docker Compose` اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```yaml
services:
  test_db:
    image: postgres:16
    environment:
      POSTGRES_DB: test_db
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U test"]
      interval: 5s
      timeout: 5s
      retries: 5

  test:
    build: .
    command: pytest --cov=. --cov-report=term-missing
    environment:
      DATABASE_URL: postgresql://test:test@test_db:5432/test_db
    depends_on:
      test_db:
        condition: service_healthy
```

</div>

<div dir="ltr">

```bash
docker compose run --rm test
```

</div>

---

### ۲۳. چگونه `Database` تست را ایزوله می‌کنید؟

**پاسخ:**

برای تست‌ها یک `Database` جداگانه استفاده می‌شود.

در `Django` با `--keepdb` از ایجاد مجدد جلوگیری می‌شود.

در `pytest` با `Fixture`های `autouse` جداول ایجاد و حذف می‌شوند.

`Database` تست هرگز با `Database` اصلی مشترک نیست.

---

## `CI/CD` و `Coverage`

### ۲۴. `Code Coverage` چیست و چگونه اندازه‌گیری می‌شود؟

**پاسخ:**

درصد کدی که در تست‌ها اجرا شده است.

با `coverage.py` اندازه‌گیری می‌شود.

در `pytest` با `pytest-cov` استفاده می‌شود.

معمولاً بالای ۸۰ درصد هدف قرار می‌گیرد.

<div dir="ltr">

```bash
pytest --cov=apps --cov-report=html --cov-fail-under=80
```

</div>

---

### ۲۵. `CI Pipeline` برای تست‌ها چگونه است؟

**پاسخ:**

در هر `Pull Request` تست‌ها اجرا می‌شوند.

`Linting` با `Ruff` یا `Flake8` انجام می‌شود.

`Type Checking` با `mypy` انجام می‌شود.

تست‌ها با `pytest` اجرا می‌شوند.

`Coverage Report` تولید می‌شود.

در صورت شکست تست، `Merge` مسدود می‌شود.

---

### ۲۶. `conftest.py` در `pytest` چیست؟

**پاسخ:**

فایل مشترک برای `Fixture`ها در تمام تست‌ها است.

`Fixture`های تعریف‌شده در آن به‌صورت خودکار در دسترس هستند.

در هر پوشه می‌توان یک `conftest.py` داشت.

<div dir="ltr">

```python
import pytest

@pytest.fixture
def api_client():
    return APIClient()

@pytest.fixture
def authenticated_user(api_client):
    user = UserFactory()
    api_client.force_authenticate(user=user)
    return user
```

</div>

---

## مباحث پیشرفته

### ۲۷. `Integration Test` و `Unit Test` چه تفاوتی دارند؟

**پاسخ:**

`Unit Test` یک واحد کوچک را به‌صورت مستقل تست می‌کند.

`Integration Test` تعامل بین اجزا را تست می‌کند.

`Unit Test` سریع‌تر است و `Mock` بیشتری دارد.

`Integration Test` واقعی‌تر است اما کندتر است.

هر دو در یک پروژه سالم ضروری هستند.

---

### ۲۸. چگونه تست‌های `Celery Task` را اجرا می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
CELERY_TASK_ALWAYS_EAGER = True
CELERY_TASK_EAGER_PROPAGATES = True
```

</div>

در این حالت `Task`ها به‌صورت `Synchronous` اجرا می‌شوند.

نیازی به `Worker` نیست.

برای تست‌های سریع و ایزوله مناسب است.

---

### ۲۹. چگونه `External API` را در تست‌ها شبیه‌سازی می‌کنید؟

**پاسخ:**

<div dir="ltr">

```python
import responses
import requests

@responses.activate
def test_external_api_call():
    responses.add(
        responses.GET,
        'https://api.external.com/data',
        json={'result': 'success'},
        status=200
    )

    result = requests.get('https://api.external.com/data')
    assert result.json()['result'] == 'success'
```

</div>

---

### ۳۰. بهترین شیوه‌های `Testing` در `Backend` چیست؟

**پاسخ:**

هر `Bug` با یک تست جدید همراه باشد.

نام تست‌ها توصیفی و خوانا باشد.

تست‌ها مستقل و ایزوله باشند.

از `Factory Boy` به‌جای `Fixture` استفاده شود.

`Mock` فقط برای وابستگی‌های خارجی استفاده شود.

`Coverage` هدف باشد اما معیار کیفیت نباشد.

تست‌ها در `CI` به‌صورت خودکار اجرا شوند.

`Happy Path` و `Edge Case`ها پوشش داده شوند.

---

## جمع‌بندی نکات کلیدی

| موضوع | نکته کلیدی |
|---|---|
| `TestCase` | `Transaction` و `Rollback` خودکار |
| `APITestCase` | برای تست `Endpoint`های `DRF` |
| `TestClient` | برای تست `FastAPI` |
| `Factory Boy` | جایگزین `Fixture` |
| `Mock` | فقط برای سرویس‌های خارجی |
| `Coverage` | بالای ۸۰ درصد |
| `Service Layer` | منطق کسب‌وکار جدا از `View` |
| `Settings` | تقسیم به `base` و `dev` و `prod` |
| `Docker Test` | `Database` ایزوله و `healthcheck` |
| `CI/CD` | تست خودکار در هر `Pull Request` |

</div>
