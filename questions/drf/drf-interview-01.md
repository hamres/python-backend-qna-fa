
<div dir="rtl">

# سوالات مصاحبه Django REST Framework

</div>

---

<div dir="rtl">

## سوال ۱. فرق ViewSet با APIView با GenericView چیست؟ کِی از کدام استفاده می‌کنی؟

</div>

<div dir="rtl">

کلاس APIView پایین‌ترین سطح است. کنترل کامل داری ولی باید هر متد HTTP را دستی بنویسی.

کلاس GenericView عملیات‌های استاندارد مثل List، Create، Retrieve را آماده دارد. برای Endpointهای ساده و CRUD مناسب است.

کلاس ViewSet مجموعه‌ای از اکشن‌هاست که با Router به URL متصل می‌شود. برای APIهای استاندارد و یکدست بهترین گزینه است.

</div>

```python
class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.filter(is_active=True)
    serializer_class = ProductSerializer
    permission_classes = [IsAuthenticated]
```

<div dir="rtl">

اگر منطق Endpoint нестандарт است، APIView. اگر CRUD معمولی است، ViewSet.

</div>

---

<div dir="rtl">

## سوال ۲. فرق `request.data` با `request.query_params` چیست؟

</div>

<div dir="rtl">

فیلد request.data محتوای Body را برمی‌گرداند. شامل JSON، Form Data و Multipart می‌شود. برای متدهای POST، PUT و PATCH استفاده می‌شود.

فیلد request.query_params پارامترهای URL را برمی‌گرداند. معادل request.GET در Django است.

</div>

```
GET /api/products/?category=5&page=2
```

<div dir="rtl">

در این مثال `category` و `page` داخل `query_params` هستند.

</div>

```python
category = request.query_params.get("category")
body = request.data
```

---

<div dir="rtl">

## سوال ۳. چطور یک Permission سفارشی می‌نویسی؟

</div>

<div dir="rtl">

کلاس را از `BasePermission` ارث می‌بریم و متد `has_permission` یا `has_object_permission` را پیاده‌سازی می‌کنیم.

</div>

```python
class IsOwnerOrReadOnly(BasePermission):

    def has_object_permission(self, request, view, obj):
        if request.method in SAFE_METHODS:
            return True
        return obj.owner == request.user
```

<div dir="rtl">
متد has_permission روی لیست و ساخت Object اجرا می‌شود. متد has_object_permission فقط بعد از واکشی یک Object مشخص صدا زده می‌شود.

</div>

---

<div dir="rtl">

## سوال ۴. Throttling در DRF چیست و چطور محدودیت درخواست اعمال می‌کنی؟

</div>

<div dir="rtl">

منظور از Throttling محدود کردن تعداد درخواست‌هایی است که یک کاربر یا IP می‌تواند در بازه زمانی مشخص ارسال کند. برای جلوگیری از Abuse و محافظت از سرور استفاده می‌شود.

</div>

```python
REST_FRAMEWORK = {
    "DEFAULT_THROTTLE_CLASSES": [
        "rest_framework.throttling.AnonRateThrottle",
        "rest_framework.throttling.UserRateThrottle",
    ],
    "DEFAULT_THROTTLE_RATES": {
        "anon": "30/minute",
        "user": "100/minute",
    },
}
```

<div dir="rtl">

برای Endpoint خاص هم می‌توان Throttle سفارشی تعریف کرد.

</div>

```python
class BurstRateThrottle(UserRateThrottle):
    scope = "burst"
```

```python
REST_FRAMEWORK = {
    "DEFAULT_THROTTLE_RATES": {
        "burst": "10/minute",
    },
}
```

---

<div dir="rtl">

## سوال ۵. Pagination در DRF چطور کار می‌کند و کدام نوع را ترجیح می‌دهی؟

</div>

<div dir="rtl">

DRF سه نوع Pagination داخلی دارد.

</div>

| نوع | کاربرد |
|------|---------|
| `PageNumberPagination` | صفحه‌بندی معمولی |
| `LimitOffsetPagination` | کنترل دقیق تعداد و آفست |
| `CursorPagination` | داده‌های بزرگ و Real-time |

<div dir="rtl">

برای داده‌های بزرگ که مدام تغییر می‌کنند، `CursorPagination` بهتر است چون مشکل جابه‌جایی داده بین صفحه‌ها را ندارد.

</div>

```python
REST_FRAMEWORK = {
    "DEFAULT_PAGINATION_CLASS": (
        "rest_framework.pagination.CursorPagination"
    ),
    "PAGE_SIZE": 20,
}
```

```python
class ProductCursorPagination(CursorPagination):
    page_size = 20
    ordering = "-created_at"
```

---

<div dir="rtl">

## سوال۶. یک Serializer برای داده‌های تو در تو (Nested) چطور می‌نویسی؟

</div>

<div dir="rtl">

ما Serializer داخلی را به‌عنوان فیلد در Serializer بیرونی تعریف می‌کنیم. برای خواندن ساده است ولی برای نوشتن باید متد `create` یا `update` را دستی مدیریت کنیم.

</div>

```python
class AddressSerializer(serializers.ModelSerializer):
    class Meta:
        model = Address
        fields = ["city", "street", "postal_code"]


class UserSerializer(serializers.ModelSerializer):
    addresses = AddressSerializer(many=True)

    class Meta:
        model = User
        fields = ["id", "username", "email", "addresses"]

    def create(self, validated_data):
        addresses_data = validated_data.pop("addresses")
        user = User.objects.create_user(**validated_data)
        for address_data in addresses_data:
            Address.objects.create(user=user, **address_data)
        return user
```

<div dir="rtl">

اگر Nested خیلی عمیق یا پیچیده شود، ترجیح می‌دهم نوشتن را به Service Layer منتقل کنم.

</div>

---

<div dir="rtl">

## سوال ۷. فرق `ModelSerializer` با `Serializer` معمولی چیست؟

</div>

<div dir="rtl">

کلاس ModelSerializer به‌صورت خودکار فیلدها را از Model می‌سازد و متدهای create و update پیش‌فرض دارد. برای Mapping مستقیم با Model مناسب است.

کلاس Serializer معمولی کنترل کامل دستی دارد. وقتی ساختار خروجی با Model یک‌به‌یک نیست یا از چند منبع مختلف داده می‌گیریم، از آن استفاده می‌کنیم.

</div>

```python
class ProductModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = "__all__"
```

```python
class ProductDashboardSerializer(serializers.Serializer):
    total_sales = serializers.IntegerField()
    top_category = serializers.CharField()
    revenue_chart = serializers.DictField()
```

---

<div dir="rtl">

## سوال ۸. چطور یک فیلد سفارشی در Serializer می‌سازی؟

</div>

<div dir="rtl">

از `SerializerMethodField` یا ساخت کلاس Field سفارشی استفاده می‌کنیم.

</div>

```python
class ProductSerializer(serializers.ModelSerializer):
    discount_price = serializers.SerializerMethodField()

    class Meta:
        model = Product
        fields = ["id", "name", "price", "discount_price"]

    def get_discount_price(self, obj):
        return obj.price * 0.9
```

<div dir="rtl">

اگر فیلد قابل استفاده مجدد است، بهتر است کلاس Field جدا بسازیم.

</div>

```python
class TomanField(serializers.Field):
    def to_representation(self, value):
        return f"{value:,} تومان"

    def to_internal_value(self, data):
        return int(data.replace(",", ""))
```

---

<div dir="rtl">

## سوال ۹. Content Negotiation در DRF چیست؟

</div>

<div dir="rtl">

منظور از Content Negotiation این است که سرور بر اساس Headerهای درخواست کلاینت تصمیم بگیرد داده را با چه فرمتی برگرداند یا چه فرمتی را بپذیرد. چارچوب DRF از طریق Rendererها و Parserها این کار را انجام می‌دهد.

</div>

```python
REST_FRAMEWORK = {
    "DEFAULT_RENDERER_CLASSES": [
        "rest_framework.renderers.JSONRenderer",
        "rest_framework.renderers.BrowsableAPIRenderer",
    ],
    "DEFAULT_PARSER_CLASSES": [
        "rest_framework.parsers.JSONParser",
        "rest_framework.parsers.FormParser",
        "rest_framework.parsers.MultiPartParser",
    ],
}
```

<div dir="rtl">

کلاینت با Header `Accept` مشخص می‌کند چه فرمتی می‌خواهد و با `Content-Type` فرمت Body ارسالی را اعلام می‌کند.

</div>

---

<div dir="rtl">

## سوال ۱۰. Exception Handling در DRF چطور کار می‌کند و چطور Handler سفارشی می‌نویسی؟

</div>

<div dir="rtl">

چارچوب DRF خطاها را با exception_handler مدیریت می‌کند و Response استاندارد JSON برمی‌گرداند. برای سفارشی‌سازی، تابع خودمان را جایگزین می‌کنیم.

</div>

```python
def custom_exception_handler(exc, context):
    response = exception_handler(exc, context)

    if response is not None:
        response.data["status_code"] = response.status_code
        response.data["error"] = response.data.get("detail")

    return response
```

```python
REST_FRAMEWORK = {
    "EXCEPTION_HANDLER": "core.exceptions.custom_exception_handler",
}
```

<div dir="rtl">

برای خطاهای Business هم Exception اختصاصی تعریف می‌کنیم.

</div>

```python
class InsufficientStock(APIException):
    status_code = 409
    default_detail = "Stock is not enough."
    default_code = "insufficient_stock"
```

---

<div dir="rtl">

## سوال ۱۱. چطور API را Versioning می‌کنی؟

</div>

<div dir="rtl">

چارچوب DRF چند روش Versioning دارد. رایج‌ترین آن‌ها URL Path و Namespace است.

</div>

```python
REST_FRAMEWORK = {
    "DEFAULT_VERSIONING_CLASS": (
        "rest_framework.versioning.NamespaceVersioning"
    ),
    "DEFAULT_VERSION": "v1",
    "ALLOWED_VERSIONS": ["v1", "v2"],
}
```

```python
urlpatterns = [
    path("api/v1/", include(("products.urls", "products"), namespace="v1")),
    path("api/v2/", include(("products_v2.urls", "products"), namespace="v2")),
]
```

<div dir="rtl">

در View هم می‌توان بر اساس نسخه رفتار متفاوت داشت.

</div>

```python
def get_serializer_class(self):
    if self.request.version == "v2":
        return ProductV2Serializer
    return ProductSerializer
```

---

<div dir="rtl">

## سوال ۱۲. فرق Authentication Class با Permission Class چیست؟

</div>

<div dir="rtl">

مکانیزم Authentication مشخص می‌کند کاربر کیست. مکانیزم Permission مشخص می‌کند کاربر اجازه انجام این کار را دارد یا نه. اول Authentication اجرا می‌شود و request.user ساخته می‌شود. سپس Permission بررسی می‌شود.

</div>

```python
class ProductViewSet(viewsets.ModelViewSet):
    authentication_classes = [JWTAuthentication]
    permission_classes = [IsAuthenticated, IsOwnerOrReadOnly]
```

<div dir="rtl">

ممکن است کاربر احراز هویت شده باشد ولی Permission دسترسی به آن Object را نداشته باشد.

</div>

---

<div dir="rtl">

## سوال ۱۳. چطور File Upload را در DRF مدیریت می‌کنی؟

</div>

<div dir="rtl">

از `MultiPartParser` استفاده می‌کنیم و فیلد را `FileField` یا `ImageField` قرار می‌دهیم.

</div>

```python
class UploadSerializer(serializers.Serializer):
    file = serializers.FileField()


class UploadView(APIView):
    parser_classes = [MultiPartParser]

    def post(self, request):
        serializer = UploadSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        serializer.save()
        return Response(status=201)
```

<div dir="rtl">

برای تولید، فایل را مستقیم در Media ذخیره نمی‌کنیم. بهتر است به Object Storage مثل S3 یا MinIO منتقل شود. همچنین حتماً نوع و حجم فایل را اعتبارسنجی می‌کنیم.

</div>

---

<div dir="rtl">

## سوال ۱۴. Serializer Performance را چطور بهبود می‌دهی؟

</div>

<div dir="rtl">

مشکل رایج Serializerها تعداد زیاد Query و فیلدهای غیرضروری است.

راهکارها:

- استفاده از `select_related` و `prefetch_related` در QuerySet
- محدود کردن فیلدها با `fields` در Meta
- استفاده از `values()` برای داده‌های خواندنی سنگین
- پرهیز از `SerializerMethodField`های پرهزینه داخل لیست
- استفاده از Pagination

</div>

```python
class ProductListSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ["id", "name", "price"]
```

<div dir="rtl">

برای Endpointهای پربازدید، گاهی از `values()` و ساخت دستی Dict استفاده می‌کنیم تا سربار Serializer کم شود.

</div>

---

<div dir="rtl">

## سوال ۱۵. `@action` در ViewSet چیست و کِی استفاده می‌کنی؟

</div>

<div dir="rtl">

دکوراتور @action برای اضافه کردن Endpointهای سفارشی به ViewSet بدون نیاز به ساخت View جداگانه است.

</div>

```python
class OrderViewSet(viewsets.ModelViewSet):
    queryset = Order.objects.all()
    serializer_class = OrderSerializer

    @action(detail=True, methods=["post"], url_path="cancel")
    def cancel_order(self, request, pk=None):
        order = self.get_object()
        order.cancel()
        return Response({"status": "canceled"})
```

<div dir="rtl">

این Endpoint روی آدرس `/orders/{id}/cancel/` در دسترس خواهد بود. برای اکشن‌های غیر CRUD مثل تأیید، لغو، ارسال مجدد و گزارش‌گیری مناسب است.

</div>

---

<div dir="rtl">

## سوال ۱۶. چطور Filtering، Search و Ordering را در DRF پیاده‌سازی می‌کنی؟

</div>

<div dir="rtl">

از `django-filter` برای Filtering و از کلاس‌های داخلی DRF برای Search و Ordering استفاده می‌کنیم.

</div>

```python
REST_FRAMEWORK = {
    "DEFAULT_FILTER_BACKENDS": [
        "django_filters.rest_framework.DjangoFilterBackend",
        "rest_framework.filters.SearchFilter",
        "rest_framework.filters.OrderingFilter",
    ],
}
```

```python
class ProductViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Product.objects.filter(is_active=True)
    serializer_class = ProductSerializer
    filterset_fields = ["category", "in_stock"]
    search_fields = ["name", "description"]
    ordering_fields = ["price", "created_at"]
```

<div dir="rtl">

کلاینت می‌تواند این‌گونه درخواست بزند:

</div>

```
GET /api/products/?category=5&search=laptop&ordering=-price
```

---

<div dir="rtl">

## سوال ۱۷. Router در DRF چه کار می‌کند و فرق DefaultRouter با SimpleRouter چیست؟

</div>

<div dir="rtl">

Router به‌صورت خودکار URLهای ViewSet را تولید می‌کند و نیازی به نوشتن دستی URL نیست.

`SimpleRouter` مسیرهای اصلی را می‌سازد. `DefaultRouter` علاوه بر آن، یک Root View برای لیست Endpointها هم اضافه می‌کند.

</div>

```python
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register("products", ProductViewSet, basename="product")

urlpatterns = [
    path("api/", include(router.urls)),
]
```

<div dir="rtl">

خروجی شامل این مسیرها می‌شود:

</div>

```
GET    /api/products/
POST   /api/products/
GET    /api/products/{id}/
PUT    /api/products/{id}/
PATCH  /api/products/{id}/
DELETE /api/products/{id}/
```

---

<div dir="rtl">

## سوال ۱۸. چطور یک Bulk Operation در DRF پیاده‌سازی می‌کنی؟

</div>

<div dir="rtl">

چارچوب DRF به‌صورت پیش‌فرض Bulk ندارد. باید خودمان مدیریت کنیم. برای Bulk Create از bulk_create در لایه Service استفاده می‌کنیم.

</div>

```python
class BulkCreateMixin:

    def get_serializer(self, *args, **kwargs):
        if isinstance(kwargs.get("data"), list):
            kwargs["many"] = True
        return super().get_serializer(*args, **kwargs)
```

```python
class ProductViewSet(BulkCreateMixin, viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

<div dir="rtl">

برای Update و Delete گروهی هم بهتر است در Service Layer با Transaction و `update()` یا `delete()` انجام شود تا تعداد Query کنترل‌شده باشد.

</div>

---

<div dir="rtl">

## سوال ۱۹. چطور خروجی API را Cache می‌کنی؟

</div>

<div dir="rtl">

از Decorator یا Middleware Cache در DRF استفاده می‌کنیم. برای Endpointهای GET پرتکرار مناسب است.

</div>

```python
from django.utils.decorators import method_decorator
from django.views.decorators.cache import cache_page


class ProductViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer

    @method_decorator(cache_page(60 * 5))
    def dispatch(self, *args, **kwargs):
        return super().dispatch(*args, **kwargs)
```

<div dir="rtl">

مقدار Cache Key باید شامل Query String هم باشد تا نتایج فیلترشده اشتباه Cache نشوند. در داده‌های پویا TTL کوتاه در نظر می‌گیریم و Invalidation را مشخص می‌کنیم.

</div>

---

<div dir="rtl">

## سوال ۲۰. چطور از N+1 در Serializer جلوگیری می‌کنی؟

</div>

<div dir="rtl">

مشکل N+1 معمولاً وقتی اتفاق می‌افتد که داخل Serializer به Relation دسترسی داریم ولی QuerySet اصلی آن را Prefetch نکرده است.

</div>

```python
class ProductSerializer(serializers.ModelSerializer):
    category_name = serializers.CharField(source="category.name")

    class Meta:
        model = Product
        fields = ["id", "name", "category_name"]
```

<div dir="rtl">

اگر QuerySet ساده باشد، برای هر Product یک Query اضافه برای Category اجرا می‌شود. راه‌حل در QuerySet View است.

</div>

```python
class ProductViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Product.objects.select_related("category")
    serializer_class = ProductSerializer
```

<div dir="rtl">

برای Relationهای ManyToMany یا Reverse هم از `prefetch_related` استفاده می‌کنیم.

</div>

```python
queryset = Category.objects.prefetch_related("products")
```

---

<div dir="rtl">

## سوال ۲۱. چطور یک Endpoint برای Report یا Aggregation می‌سازی؟

</div>

<div dir="rtl">

از Annotation و Aggregation در ORM استفاده می‌کنیم و نتیجه را با Serializer مناسب برمی‌گردانیم.

</div>

```python
from django.db.models import Count, Sum


class SalesReportView(APIView):
    permission_classes = [IsAdminUser]

    def get(self, request):
        data = (
            Order.objects
            .values("created_at__month")
            .annotate(
                total_orders=Count("id"),
                total_revenue=Sum("amount"),
            )
            .order_by("created_at__month")
        )
        return Response(data)
```

<div dir="rtl">

این کار در Database انجام می‌شود و از انتقال حجم زیاد داده به Python جلوگیری می‌کند.

</div>

---

<div dir="rtl">

## سوال ۲۲. چطور از CORS در Django و DRF مدیریت می‌کنی؟

</div>

<div dir="rtl">

مکانیزم CORS در مرورگر است که مشخص می‌کند چه Originهایی اجازه دسترسی به API را دارند. در Django با پکیج django-cors-headers مدیریت می‌شود.

</div>

```python
INSTALLED_APPS = [
    "corsheaders",
]

MIDDLEWARE = [
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.common.CommonMiddleware",
]

CORS_ALLOWED_ORIGINS = [
    "https://app.example.com",
    "http://localhost:3000",
]

CORS_ALLOW_CREDENTIALS = True
```

<div dir="rtl">

در محیط تولید `CORS_ALLOW_ALL_ORIGINS` را `True` نمی‌گذاریم و فقط Originهای مشخص را مجاز می‌کنیم.

</div>

---

<div dir="rtl">

## سوال ۲۳. فرق `source` با `read_only` و `write_only` در Serializer چیست؟

</div>

<div dir="rtl">

پارامتر source مشخص می‌کند فیلد از کدام Attribute یا متد Model خوانده شود.

پارامتر read_only یعنی فیلد فقط در خروجی نمایش داده می‌شود و در ورودی نادیده گرفته می‌شود.

پارامتر write_only یعنی فیلد فقط در ورودی پذیرفته می‌شود و در خروجی نمایش داده نمی‌شود. برای فیلدهایی مثل رمز عبور کاربرد دارد.

</div>

```python
class UserSerializer(serializers.ModelSerializer):
    full_name = serializers.CharField(source="get_full_name", read_only=True)
    password = serializers.CharField(write_only=True, min_length=8)

    class Meta:
        model = User
        fields = ["id", "username", "email", "full_name", "password"]
```

---

<div dir="rtl">

## سوال ۲۴. چطور Authentication را با JWT در DRF پیاده‌سازی می‌کنی؟

</div>

<div dir="rtl">

از `djangorestframework-simplejwt` استفاده می‌کنیم.

</div>

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework_simplejwt.authentication.JWTAuthentication",
    ],
}
```

```python
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
    TokenVerifyView,
)

urlpatterns = [
    path("auth/token/", TokenObtainPairView.as_view()),
    path("auth/token/refresh/", TokenRefreshView.as_view()),
    path("auth/token/verify/", TokenVerifyView.as_view()),
]
```

<div dir="rtl">

کلاینت توکن را در Header `Authorization` با پیشوند `Bearer` ارسال می‌کند.

</div>

```
Authorization: Bearer <access_token>
```

---

<div dir="rtl">

## سوال ۲۵. اگر بخواهی یک فیلد فقط تحت شرط خاصی در Serializer نمایش داده شود، چه می‌کنی؟

</div>

<div dir="rtl">

در متد `to_representation` فیلد را حذف یا اضافه می‌کنیم.

</div>

```python
class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ["id", "name", "price", "internal_code"]

    def to_representation(self, instance):
        data = super().to_representation(instance)
        request = self.context.get("request")

        if not request or not request.user.is_staff:
            data.pop("internal_code", None)

        return data
```

<div dir="rtl">

این روش برای فیلدهای حساس که فقط ادمین باید ببیند مناسب است. اگر شرط پیچیده‌تر باشد، Serializer جداگانه برای نقش‌های مختلف تعریف می‌کنیم.

</div>

---

<div align="left">

سازنده: معین رضایی
ایمیل: moeinrezaie516@gmail.com

</div>
# ۳۰ سؤال تخصصی و سطح بالا در مصاحبه `Django REST Framework`

این فایل شامل ۳۰ سؤال پرتکرار و سطح بالا در مصاحبه‌های شغلی `Backend` با تمرکز بر `Django REST Framework` است. پاسخ‌ها به‌صورت کوتاه، مستقیم و بدون حاشیه برای مرور سریع و حرفه‌ای طراحی شده‌اند.

---

## Serializers

### ۱. تفاوت `Serializer` و `ModelSerializer` چیست؟
**پاسخ:** `Serializer` برای تعریف دستی `Field`ها استفاده می‌شود. `ModelSerializer` به‌طور خودکار `Field`ها را از `Model` می‌سازد. `ModelSerializer` همچنین متدهای `create()` و `update()` را به‌صورت پیش‌فرض پیاده‌سازی می‌کند.

### ۲. کاربرد `SerializerMethodField` چیست؟
**پاسخ:** این `Field` فقط برای خواندن (`Read-Only`) است. برای محاسبه و برگرداندن داده‌هایی استفاده می‌شود که مستقیماً در `Model` وجود ندارند. مقدار آن توسط متدی با نام `get_<field_name>` تولید می‌شود.

### ۳. چگونه `Nested Serializer` را برای عملیات `Write` پیاده‌سازی می‌کنید؟
**پاسخ:** `ModelSerializer` به‌صورت پیش‌فرض از `Write` روی `Nested Serializer` پشتیبانی نمی‌کند. باید متد `create()` یا `update()` را در `Serializer` بازنویسی کنید. در این متد باید `Nested Data` را به‌صورت دستی مدیریت و `Model`های مرتبط را ایجاد یا آپدیت کنید.

### ۴. چگونه `Validation` سفارشی را در سطح `Object` پیاده‌سازی می‌کنید؟
**پاسخ:** با بازنویسی متد `validate()` در `Serializer`. این متد یک `Dictionary` از تمام `Data`های اعتبارسنجی‌شده را دریافت می‌کند. در صورت بروز خطا باید `ValidationError` را `Raise` کنید.

```python
def validate(self, data):
    if data['start_date'] > data['end_date']:
        raise serializers.ValidationError("Start date must be before end date.")
    return data
```

### ۵. تفاوت `validate_<field_name>` و `validate` چیست؟
**پاسخ:** `validate_<field_name>` فقط برای اعتبارسنجی یک `Field` خاص استفاده می‌شود. متد `validate()` برای اعتبارسنجی‌هایی است که به چند `Field` به‌صورت همزمان وابسته هستند.

### ۶. کاربرد `Context` در `Serializer` چیست؟
**پاسخ:** `Context` یک `Dictionary` است که معمولاً شامل `Request`، `View` و `Format` است. این `Data`ها از `View` به `Serializer` منتقل می‌شوند تا در متدهایی مانند `to_representation()` یا `validate()` در دسترس باشند.

### ۷. کاربرد `to_representation()` در `Serializer` چیست؟
**پاسخ:** این متد برای سفارشی‌سازی خروجی نهایی `API` استفاده می‌شود. می‌توان `Field`ها را تغییر نام داد، فرمت `Date` را تغییر داد یا `Data`های جدیدی را به `Response` اضافه کرد.

```python
def to_representation(self, instance):
    representation = super().to_representation(instance)
    representation['full_name'] = f"{instance.first_name} {instance.last_name}"
    return representation
```

### ۸. کاربرد `read_only_fields` در `ModelSerializer` چیست؟
**پاسخ:** `Field`هایی که در این لیست قرار می‌گیرند در زمان `Validation` نادیده گرفته می‌شوند. این `Field`ها فقط در خروجی `Response` نمایش داده می‌شوند و کاربر نمی‌تواند آن‌ها را ارسال کند.

---

## Views و ViewSets

### ۹. کاربرد `get_queryset()` در `View`ها چیست؟
**پاسخ:** این متد برای محدود کردن `QuerySet` بر اساس کاربر لاگین‌شده یا پارامترهای `URL` استفاده می‌شود. این کار باعث می‌شود کاربر فقط به داده‌های مجاز خود دسترسی داشته باشد.

### ۱۰. تفاوت `APIView` و `GenericAPIView` چیست؟
**پاسخ:** `APIView` پایه‌ای‌ترین کلاس `View` در DRF است و فقط `Request` و `Response` را مدیریت می‌کند. `GenericAPIView` قابلیت‌های بیشتری مانند `QuerySet`، `Serializer` و `Pagination` را به‌صورت پیش‌فرض ارائه می‌دهد.

### ۱۱. کاربرد `perform_create()` چیست؟
**پاسخ:** این متد در `GenericAPIView` برای سفارشی‌سازی عملیات ذخیره‌سازی استفاده می‌شود. معمولاً برای اضافه کردن `Request.user` به `Model` قبل از ذخیره نهایی کاربرد دارد.

```python
def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
```

### ۱۲. تفاوت `CreateModelMixin` و `UpdateModelMixin` چیست؟
**پاسخ:** `CreateModelMixin` متد `create()` و `perform_create()` را برای عملیات `POST` ارائه می‌دهد. `UpdateModelMixin` متدهای `update()` و `partial_update()` را برای عملیات `PUT` و `PATCH` فراهم می‌کند.

### ۱۳. تفاوت `ViewSet` و `APIView` از نظر `URL Routing` چیست؟
**پاسخ:** در `APIView` باید `URL`ها را به‌صورت دستی در `urls.py` تعریف کنید. در `ViewSet` از `Router`ها استفاده می‌شود که `URL`ها را به‌صورت خودکار بر اساس `HTTP Methods` تولید می‌کنند.

### ۱۴. کاربرد `action()` در `ViewSet` چیست؟
**پاسخ:** برای تعریف `Endpoint`های سفارشی که جزو عملیات استاندارد `CRUD` نیستند استفاده می‌شود. با استفاده از `detail=True` یا `detail=False` مشخص می‌شود که آیا این `Action` روی یک `Object` تکی کار می‌کند یا یک لیست.

### ۱۵. تفاوت `PUT` و `PATCH` در DRF چیست؟
**پاسخ:** `PUT` برای جایگزینی کامل یک `Object` استفاده می‌شود و تمام `Field`های اجباری باید ارسال شوند. `PATCH` برای آپدیت جزئی استفاده می‌شود و فقط `Field`های ارسال‌شده تغییر می‌کنند. `Partial=True` در `Serializer` این کار را انجام می‌دهد.

---

## Permissions، Authentication و Security

### ۱۶. تفاوت `Authentication` و `Permission` چیست؟
**پاسخ:** `Authentication` هویت کاربر را تأیید می‌کند (اینکه کاربر کیست). `Permission` تعیین می‌کند که آیا کاربر تأیید‌شده اجازه انجام یک عمل خاص را دارد یا خیر.

### ۱۷. کاربرد `has_object_permission()` چیست؟
**پاسخ:** این متد در `BasePermission` برای بررسی دسترسی به یک `Object` خاص استفاده می‌شود. این متد فقط زمانی فراخوانی می‌شود که `View` روی یک `Object` تکی (`Detail View`) کار می‌کند.

### ۱۸. چگونه `JWT Authentication` را مدیریت می‌کنید؟
**پاسخ:** معمولاً از `Simple JWT` استفاده می‌شود. `Access Token` برای احراز هویت و `Refresh Token` برای دریافت `Access Token` جدید استفاده می‌شود. برای امنیت بیشتر از `Refresh Token Rotation` و `Token Blacklisting` استفاده می‌شود.

### ۱۹. چگونه `Throttling` سفارشی بر اساس `IP` ایجاد می‌کنید؟
**پاسخ:** باید از `AnonRateThrottle` یا `UserRateThrottle` ارث‌بری کنید. متد `get_cache_key()` را بازنویسی کنید تا `Key` مورد نظر را بر اساس `IP` تولید کند.

### ۲۰. کاربرد `perform_destroy()` چیست؟
**پاسخ:** این متد برای سفارشی‌سازی عملیات حذف `Object` استفاده می‌شود. به‌جای حذف فیزیکی، می‌توان یک `Field` مانند `is_deleted` را `True` کرد (`Soft Delete`).

### ۲۱. چگونه `Exception Handler` سفارشی را پیاده‌سازی می‌کنید؟
**پاسخ:** باید یک تابع بنویسید که `Exception` را دریافت و `Response` سفارشی برگرداند. سپس مسیر این تابع را در `settings.py` در بخش `REST_FRAMEWORK` و کلید `EXCEPTION_HANDLER` تنظیم کنید.

---

## Optimization و Performance

### ۲۲. چگونه `N+1 Query Problem` را در DRF حل می‌کنید؟
**پاسخ:** با استفاده از `select_related()` برای روابط `One-To-One` و `ForeignKey`. برای روابط `Many-To-Many` و `Reverse ForeignKey` از `prefetch_related()` در متد `get_queryset()` استفاده می‌شود.

### ۲۳. کاربرد `SerializerMethodField` در ایجاد مشکل `N+1` چیست؟
**پاسخ:** اگر در `SerializerMethodField` یک کوئری به `Database` زده شود، برای هر `Object` یک کوئری جداگانه اجرا می‌شود. این کار مشکل `N+1` را تشدید می‌کند و باید از آن پرهیز کرد.

### ۲۴. چگونه `Pagination` سفارشی را پیاده‌سازی می‌کنید؟
**پاسخ:** باید یک کلاس جدید از `PageNumberPagination` ارث‌بری کنید. سپس `Page Size` و `Query Parameters` را تغییر دهید. در نهایت این کلاس را در `settings.py` یا `pagination_class` در `View` تنظیم کنید.

### ۲۵. چگونه `Bulk Create` را در DRF پیاده‌سازی می‌کنید؟
**پاسخ:** در `Serializer` باید پارامتر `many=True` را هنگام `Initialize` کردن ارسال کنید. برای بهینه‌سازی بهتر، در متد `create()` از متد `bulk_create()` در `Django ORM` استفاده می‌شود.

### ۲۶. چگونه `Caching` را در DRF پیاده‌سازی می‌کنید؟
**پاسخ:** می‌توان از `@cache_page` استفاده کرد، اما بهتر است از `Django's Cache Framework` در سطح `Database` یا `Redis` استفاده شود. برای `API`های `GET` می‌توان `Cache` را در متد `get()` یا با استفاده از `Middleware` مدیریت کرد.

---

## Advanced Features و Architecture

### ۲۷. تفاوت `DefaultRouter` و `SimpleRouter` چیست؟
**پاسخ:** `DefaultRouter` یک `API Root View` به‌صورت خودکار تولید می‌کند که لیست تمام `Endpoint`ها را برمی‌گرداند. همچنین از `Format Suffixes` پشتیبانی می‌کند. `SimpleRouter` این قابلیت‌ها را ندارد.

### ۲۸. چگونه `API Versioning` را پیاده‌سازی می‌کنید؟
**پاسخ:** می‌توان از `URLPathVersioning` استفاده کرد که `Version` را در `URL` قرار می‌دهد. همچنین `AcceptHeaderVersioning` از `HTTP Headers` برای تعیین `Version` استفاده می‌کند. این تنظیمات در `DEFAULT_VERSIONING_CLASS` انجام می‌شود.

### ۲۹. کاربرد `Renderer`ها و `Parser`ها چیست؟
**پاسخ:** `Parser` داده‌های ورودی `Request` را به `Python Objects` تبدیل می‌کند (مانند `JSON`). `Renderer` داده‌های `Python` را به فرمت خروجی (مانند `JSON` یا `XML`) برای `Response` تبدیل می‌کند.

### ۳۰. چگونه `File Upload` را در DRF مدیریت می‌کنید؟
**پاسخ:** از `MultiPartParser` یا `FormParser` در `View` استفاده می‌شود. در `Serializer` باید از `FileField` یا `ImageField` استفاده کرد. همچنین باید `MEDIA_URL` و `MEDIA_ROOT` در `Django` تنظیم شوند.
