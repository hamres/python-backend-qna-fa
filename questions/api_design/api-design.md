# API چیست و کاربردهای اصلی آن چیست؟

**رابط برنامه‌نویسی کاربردی (API - Application Programming Interface)** مجموعه‌ای از Definitionها و Protocolها است که به نرم‌افزارهای مختلف اجازه می‌دهد با یکدیگر ارتباط برقرار کنند.

**رابط API** به‌عنوان یک واسط بین سیستم‌های مختلف عمل می‌کند و پیچیدگی‌های داخلی یک سیستم را از دید برنامه‌نویس پنهان می‌کند. در نتیجه، توسعه‌دهندگان می‌توانند قابلیت‌ها یا داده‌های مشخصی را بدون نیاز به درک جزئیات داخلی سیستم، به برنامهٔ خود اضافه کرده و از آن‌ها استفاده کنند.

## کاربردهای اصلی API

### ۱. انتزاع (Abstraction)

**انتزاع (Abstraction)** جزئیات پیچیدهٔ داخلی یک سیستم را پنهان می‌کند و یک Interface ساده‌تر در اختیار برنامه‌نویس قرار می‌دهد.

**برای مثال،** هنگام استفاده از یک API برای ارسال Email، برنامه‌نویس نیازی ندارد جزئیات پیچیدهٔ برقراری Connection شبکه با Mail Server را بداند.

### ۲. استانداردسازی (Standardization)

**استانداردسازی (Standardization)** قوانین و Formatهای مشترکی را برای نحوهٔ ارتباط بین سیستم‌ها مشخص می‌کند.

**این موضوع** باعث می‌شود تعاملات بین سیستم‌ها یکپارچه‌تر و قابل پیش‌بینی‌تر باشند و پیاده‌سازی و مدیریت آن‌ها ساده‌تر شود.

### ۳. جداسازی (Decoupling)

**جداسازی (Decoupling)** اجزای مختلف سیستم را از یکدیگر جدا می‌کند و به آن‌ها اجازه می‌دهد تا به‌صورت مستقل تکامل پیدا کنند.

**یعنی** اگر سیستم داخلی تغییر کند، تا زمانی که Interface خارجی API حفظ شود، معمولاً نیازی نیست مصرف‌کنندگان API تغییرات داخلی را بدانند یا تغییر کنند.

### ۴. قابلیت استفادهٔ مجدد (Reusability)

**قابلیت استفادهٔ مجدد (Reusability)** این امکان را فراهم می‌کند که قابلیت‌ها به‌شکل Modular در اختیار قرار بگیرند و در سیستم‌ها یا Applicationهای مختلف مورد استفاده قرار گیرند.

### ۵. امنیت و کنترل دسترسی (Security and Access Control)

**امنیت و کنترل دسترسی (Security and Access Control)** به API اجازه می‌دهد مکانیزم‌هایی برای Authentication و کنترل دسترسی فراهم کند تا فقط Userها یا Softwareهای مجاز بتوانند با آن تعامل داشته باشند.

**همچنین** مدیریت Security را در یک نقطه متمرکز می‌کند که می‌تواند نسبت به ایمن‌سازی تک‌تک Componentها به‌صورت جداگانه، مؤثرتر باشد.

### ۶. یکپارچه‌سازی داده‌ها و سرویس‌ها (Consolidation of Data and Services)

**یکپارچه‌سازی داده‌ها و سرویس‌ها (Consolidation of Data and Services)** به API اجازه می‌دهد داده‌ها یا Serviceها را از منابع مختلف جمع‌آوری کرده و یک View یکپارچه در اختیار Consumer قرار دهد.

**این قابلیت** به‌خصوص در Distributed Systems ارزشمند است؛ جایی که داده‌های مختلف ممکن است روی Serverهای متعدد یا سرویس‌های Cloud مختلف قرار داشته باشند.

---

## خلاصهٔ پاسخ برای مصاحبه

> **رابط برنامه‌نویسی کاربردی (API)** مجموعه‌ای از قوانین، Definitionها و Protocolهاست که به نرم‌افزارهای مختلف اجازه می‌دهد با یکدیگر ارتباط برقرار کنند.
>
> **هدف اصلی API** این است که پیچیدگی‌های داخلی یک سیستم را پنهان کند و یک Interface ساده برای استفاده از قابلیت‌ها یا داده‌های آن در اختیار برنامه‌نویس قرار دهد.
>
> **مهم‌ترین کاربردهای API** شامل Abstraction، Standardization، Decoupling، Reusability و Security هستند. همچنین API می‌تواند داده‌ها و Serviceهای مختلف را از منابع متفاوت دریافت کرده و در قالب یک Interface یکپارچه ارائه کند.

---

# تفاوت API و Web Service چیست؟

**رابط برنامه‌نویسی کاربردی (API)** و **سرویس وب (Web Service)** هر دو برای برقراری ارتباط بین دو سیستم مستقل استفاده می‌شوند، اما روش و دامنهٔ استفادهٔ آن‌ها با یکدیگر متفاوت است.

## مقایسهٔ API و Web Service

### رابط برنامه‌نویسی کاربردی (API)

**رابط API** عمدتاً برای برقراری ارتباط بین یک Web Service و یک Client Application استفاده می‌شود. معمولاً Scope محدودتری دارد و می‌تواند Functionها یا Methodهایی را به‌عنوان نقاط ورود مشخص در اختیار Client قرار دهد.

### سرویس وب (Web Service)

**سرویس Web Service** دامنهٔ گسترده‌تری دارد و امکان تعامل نه‌تنها با Clientها، بلکه با Softwareهای دیگر را نیز فراهم می‌کند و در نتیجه می‌تواند بخشی از یک معماری جامع‌تر مبتنی بر Service باشد.

## تفاوت‌های کلیدی

* **تفاوت در داده و قابلیت‌های ارائه‌شده (Data and Functionality Exposure):** Web Serviceها عمدتاً روی ارائهٔ Data تمرکز دارند که معمولاً در قالب XML یا JSON منتقل می‌شود و Business Logic را به‌صورت مستقیم در اختیار قرار نمی‌دهند. در مقابل، APIها می‌توانند علاوه بر Data، Functionality و قابلیت‌های مختلف را نیز ارائه کنند.
* **تفاوت در پروتکل ارتباطی (Communication Protocols):** Web Serviceها لزوماً به یک Communication Protocol خاص محدود نیستند. در مقابل، RESTful APIها معمولاً از HTTP استفاده می‌کنند و سرویس‌های مبتنی بر SOAP می‌توانند از Protocolهای استانداردی مانند SMTP و TCP استفاده کنند.
* **تفاوت در ساختار Interface (Interface Structure):** Web Serviceها معمولاً از Data Formatها و Protocolهای استانداردی مانند XML، SOAP یا WSDL پیروی می‌کنند. در مقابل، APIها می‌توانند از روش‌های متنوع‌تری مانند REST و GraphQL استفاده کنند.
* **تفاوت در سهولت استفاده (Ease of Use):** APIها معمولاً استفادهٔ ساده‌تری دارند و اغلب با HTTP Callهای مستقیم و Formatهای رایجی مانند JSON کار می‌کنند. Web Serviceها ممکن است پیچیده‌تر باشند و به Tooling، Protocolها و Data Formatهای مشخصی نیاز داشته باشند.
* **تفاوت در تمرکز امنیتی (Security Focus):** Web Serviceها معمولاً تمرکز بیشتری روی Security دارند و اغلب با چندین لایه از Security Protocolها محافظت می‌شوند.

## اجزای اصلی (Building Blocks)

* **نقطهٔ پایانی (Endpoint):** درخواست‌های API به URLهای مشخصی ارسال می‌شوند که به آن‌ها Endpoint گفته می‌شود. Web Serviceها نیز URLهایی دارند که عملیات مختلف به آن‌ها متصل می‌شوند.
* **متد (Method):** APIها معمولاً برای عملیات مختلف از Methodهای متفاوتی استفاده می‌کنند؛ برای مثال `POST` برای ایجاد و `GET` برای دریافت داده. در مقابل، Web Serviceها معمولاً از یک Method مانند `POST` برای مدیریت انواع مختلف عملیات استفاده می‌کنند.
* **درخواست و پاسخ (Request/Response):** هر دو مدل API و Web Service بر پایهٔ مفهوم Request و Response کار می‌کنند.

## مثال کدنویسی API Endpoint

**در این مثال** با استفاده از کتابخانهٔ `requests` یک GET Request به یک API Endpoint ارسال می‌شود:

```python
import requests

# Make a GET request to a specific API endpoint
response = requests.get('https://api.example.com/data')
print(response.json())
```

## مثال کدنویسی Web Service Endpoint

**در این مثال** یک POST Request به یک Web Service Endpoint ارسال می‌شود:

```python
import requests
from datetime import datetime

# Make a POST request to a specific web service endpoint
url = 'https://webservice.example.com/process_data'

data = {
    'action': 'process',
    'data': 'some data',
    'timestamp': str(datetime.now())
}

response = requests.post(url, data=data)

print(response.text)
```

---

## خلاصهٔ پاسخ برای مصاحبه

> **به‌طور خلاصه، API و  و Web Service هر دو برای ارتباط بین سیستم‌ها استفاده می‌شوند، اما API مفهوم گسترده‌تری دارد و می‌تواند قابلیت‌ها و Functionality مختلف را در اختیار Client قرار دهد.**
>
> وب سرویس **Web Service** معمولاً به سرویس‌هایی گفته می‌شود که از طریق شبکه و با استفاده از Protocolها و Formatهای استاندارد مانند SOAP، XML یا HTTP با سیستم‌های دیگر ارتباط برقرار می‌کنند.
>
> و **APIها** می‌توانند از روش‌هایی مانند REST و GraphQL استفاده کنند و معمولاً استفادهٔ ساده‌تر و انعطاف‌پذیرتری دارند. در عمل، یک Web Service می‌تواند API داشته باشد، اما هر API الزاماً Web Service نیست.

---

# اصول RESTful API چیست؟

**رابط‌های RESTful API** از مجموعه‌ای از اصول معماری پیروی می‌کنند که انعطاف‌پذیری **Web** را با قدرت APIهای مدرن ترکیب می‌کند. این APIها به‌گونه‌ای طراحی شده‌اند که **Stateless** باشند و امکان مدیریت منطقی **Resourceها** و پیمایش بین آن‌ها از طریق **Hyperlinkها** را فراهم کنند.

## اصول اصلی RESTful API

### ۱. جداسازی Client و Server (Client-Server Separation)

**جداسازی Client و Server** به این معناست که Client و Server مستقل از یکدیگر هستند.

کلاینت **Client** مسئول Interface و User Experience است، در حالی که **Server** مدیریت Resourceها و ذخیره‌سازی Data را بر عهده دارد.

### ۲. بدون وضعیت بودن (Statelessness)

**بدون وضعیت بودن (Statelessness)** یعنی هر Request از طرف Client باید تمام اطلاعات موردنیاز برای پردازش آن Request را در خود داشته باشد.

سرور **Server** نباید State مربوط به Client را بین Requestهای مختلف ذخیره کند. بنابراین هر Request باید به‌صورت مستقل قابل پردازش باشد.

### ۳. قابلیت Cache شدن (Cacheability)

**قابلیت Cache شدن (Cacheability)** به این معناست که Responseهای Server باید به‌صورت مشخص اعلام کنند که آیا قابلیت Cache شدن دارند یا خیر.

**مکانیزم‌های Cache Control** برای استانداردسازی این فرایند استفاده می‌شوند و می‌توانند Performance سیستم را بهبود دهند.

### ۴. سیستم لایه‌ای (Layered System)

**سیستم لایه‌ای (Layered System)** به این معناست که API می‌تواند از چندین Layer یا واسط میانی مانند Gateway و Proxy تشکیل شود.

کلایت **Client** نیازی ندارد بداند Server دقیقاً در کجا قرار دارد. این موضوع می‌تواند به بهبود **Scalability** و **Security** سیستم کمک کند.

### ۵. رابط یکپارچه (Uniform Interface)

**رابط یکپارچه (Uniform Interface)** یعنی تمام قابلیت‌های یک REST API از طریق یک Interface استاندارد و مستقل از نوع Command در دسترس باشند.

**برای مثال،** REST APIها معمولاً از HTTP Methodهایی مانند `GET`، `POST`، `PUT` و `DELETE`، همچنین HTTP Status Codeها و Content Typeهایی مانند JSON یا XML استفاده می‌کنند.

### ۶. اجرای کد در سمت Client (Code on Demand)

**اجرای کد در سمت Client (Code on Demand)** یک اصل اختیاری در REST است که به امکان ارسال Executable Code از Server به Client اشاره دارد.

**برای مثال،** یک Web Application می‌تواند Scriptهایی را از Server دریافت کند و با اجرای آن‌ها Behavior خود را در Browser تغییر دهد.

این اصل برخلاف سایر اصول، **اجباری نیست** و در همهٔ REST APIها مورد استفاده قرار نمی‌گیرد.

## تفاوت REST و RESTful

**اصطلاح RESTful** برای سیستم‌هایی استفاده می‌شود که از اصول REST پیروی می‌کنند.

در واقع **REST** در ابتدا مجموعه‌ای از اصول معماری مرتبط با ارتباطات Web بود و بعدها به مفهومی عمومی‌تر تبدیل شد. در مقابل، **RESTful System** به سیستمی گفته می‌شود که این اصول را برای تبادل Data، معمولاً از طریق HTTP، به کار می‌گیرد.

## مثال در دنیای واقعی

**برای مثال،** یک Weather Service را در نظر بگیرید که با طراحی RESTful ساخته شده است.

**در این حالت،** یک Client مانند Application مربوط به Weather روی دستگاه کاربر، یک Request برای دریافت اطلاعات آب‌وهوا به API Server ارسال می‌کند.

**در این Request،** اطلاعاتی مانند Endpoint مربوط به Resource موردنظر، مدت‌زمان Cache شدن Data، Content Type مورد انتظار مانند JSON و HTTP Method مربوطه مانند `GET` مشخص می‌شود.

**سپس Server،** Request را پردازش کرده و اطلاعات آب‌وهوای موردنظر را در قالب Response برمی‌گرداند.

**در نهایت،** Response می‌تواند به‌عنوان یک Response قابل Cache شدن مشخص شود و از استانداردهای تعریف‌شده برای Content Typeها پیروی کند.

این رویکرد باعث می‌شود **Resourceها به‌صورت مستقل مدیریت شوند**، ارتباط بین Client و Server ساختار مشخصی داشته باشد و پیچیدگی‌های مربوط به ارائهٔ Web Serviceها کاهش پیدا کند.

---

## خلاصهٔ پاسخ برای مصاحبه

> در واقع **RESTful API** یک API است که از اصول معماری REST پیروی می‌کند. مهم‌ترین اصول آن شامل **Client-Server Separation، Statelessness، Cacheability، Layered System و Uniform Interface** هستند و **Code on Demand** نیز یک اصل اختیاری است.
>
> مهم‌ترین نکته این است که در یک  RESTful API، هر Request باید مستقل باشد و Server نباید State مربوط به Client را بین Requestها نگه دارد. همچنین Resourceها معمولاً از طریق HTTP Methodهایی مانند `GET`، `POST`، `PUT` و `DELETE` مدیریت می‌شوند.
>
> هدف اصلی این اصول، ایجاد APIهایی **قابل توسعه، مقیاس‌پذیر، قابل Cache و ساده برای تعامل بین Client و Server** است.
