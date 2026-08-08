<div dir="rtl" lang="fa" style="text-align: right; font-family: Tahoma, 'B Nazanin', sans-serif;">

# ۱. پایتون چگونه اجرا می‌شود؟

کد منبع **Python** به‌صورت مستقیم توسط CPU اجرا نمی‌شود. در عوض، قبل از اجرا یک فرآیند چندمرحله‌ای را طی می‌کند که شامل ترکیبی از **کامپایل (Compilation)** و **تفسیر (Interpretation)** است.

در مصاحبه‌های Python معمولاً یکی از سؤال‌های پایه این است:

#### «آیا Python یک زبان کامپایل‌شده است یا مفسری؟»

پاسخ دقیق این است:

**پایتون یک زبان کامپایل‌شده و مفسری (Hybrid) است.**

یعنی کد Python ابتدا به یک فرمت میانی به نام **Bytecode** کامپایل می‌شود و سپس این Bytecode توسط مفسر Python اجرا می‌شود.

---

## کامپایل و تفسیر (Compilation & Interpretation)

پایتون از یک مدل ترکیبی استفاده می‌کند تا بین **قابلیت اجرا روی سیستم‌های مختلف (Portability)** و **سادگی توسعه (Ease of Development)** تعادل برقرار کند.

### ۱. کامپایل به Bytecode

وقتی یک فایل Python اجرا می‌شود، اولین مرحله این است که مفسر Python، کد سطح بالای نوشته‌شده توسط برنامه‌نویس را به **Bytecode** تبدیل می‌کند.

بایت کد یک نمایش میانی از کد است که:

- وابسته به سیستم‌عامل یا CPU خاصی نیست.
- مستقیماً توسط CPU اجرا نمی‌شود.
- توسط ماشین مجازی Python اجرا می‌شود.

معمولاً این Bytecode در مسیرهایی با نام:

<div dir="ltr" align="right">

```
__pycache__
```

</div>

ذخیره می‌شود و فایل‌هایی با پسوند:

<div dir="ltr" align="right">

```
.pyc
```

</div>

ایجاد می‌کند.

مثلاً:

<div dir="ltr" align="right">

```
main.py
```

</div>

بعد از اجرا ممکن است تبدیل شود به:

<div dir="ltr" align="right">

```
__pycache__/main.cpython-312.pyc
```

</div>

#### نکته مصاحبه‌ای

اگر از شما پرسیده شد:

**"ایا پایتون یک زبان کامپایلری؟"**

نگویید:

❌ نگیید که نه پایتون مفسریه.

پاسخ صحیح:

✅ بله پایتون ابتدا کد مارو تبدیل میکنه به بایت کد بعد این بایت کد به وسیله ماشین مجازی به صورت مفسری اجرا میشه.

---

## ۲. ماشین مجازی Python (Python Virtual Machine - PVM)

بعد از تولید Bytecode، مرحله بعدی اجرای آن است.

این کار توسط **Python Virtual Machine (PVM)** انجام می‌شود.

در واقع PVM موتور اجرایی Python است که:

۱.  ابتدا Bytecode را دریافت می‌کند.
۲. دستورهای آن را یکی‌یکی بررسی می‌کند.
۳. عملیات مورد نیاز را روی سیستم انجام می‌دهد.

به زبان ساده:

<div dir="ltr" align="right">

```
Python Code (.py)
        |
        v
Compiler
        |
        v
Bytecode (.pyc)
        |
        v
Python Virtual Machine
        |
        v
Machine Operations
```

</div>

این مدل باعث می‌شود Python روی سیستم‌های مختلف اجرا شود؛ چون یک Bytecode مشابه می‌تواند روی هر سیستمی که PVM سازگار داشته باشد اجرا شود.

---

## Bytecode در مقابل Machine Code

در زبان‌هایی مثل C یا C++، کد مستقیماً به **Machine Code** تبدیل می‌شود.

نکته : Machine Code یعنی دستورهایی که CPU مستقیماً می‌تواند اجرا کند.

مثلاً:

<div dir="ltr" align="right">

```
C++ Source Code
        |
        v
Compiler
        |
        v
Machine Code
        |
        v
CPU Execution
```

</div>

اما Python مسیر متفاوتی دارد:

<div dir="ltr" align="right">

```
Python Source Code
        |
        v
Bytecode
        |
        v
Python Virtual Machine
        |
        v
CPU
```

</div>

بایت کد یک سطح بالاتر از Machine Code قرار دارد.

مزیت این روش:

- قابل حمل بودن بیشتر
- اجرا روی سیستم‌های مختلف
- توسعه آسان‌تر

اما یک هزینه دارد:

- وجود یک لایه اضافی به نام PVM باعث کاهش سرعت نسبت به زبان‌های کامپایل‌شده می‌شود.

با این حال، نسخه‌های جدید Python با استفاده از:

- بهینه‌سازی‌های داخلی (Internal Optimizations)
- دستورهای تخصصی‌تر (Specialized Opcodes)

این اختلاف سرعت را کاهش داده‌اند.

---

## مراحل تبدیل Source Code به Bytecode

تبدیل کد Python از متن ساده به دستورهای قابل اجرا چند مرحله استاندارد دارد.

### ۱. تحلیل لغوی (Lexical Analysis)

در این مرحله، کد به بخش‌های کوچک‌تر به نام **Token** تقسیم می‌شود.

در واقع Tokenها شامل مواردی مثل:

- Keywordها
- Variable nameها
- Literalها

هستند.

مثلاً:

<div dir="ltr">

```python
x = 10
```

</div>

به چیزی شبیه این تبدیل می‌شود:

<div dir="ltr">

```
NAME(x)
=
NUMBER(10)
```

</div>

---

### ۲. تحلیل نحوی (Syntax Parsing)

در این مرحله Tokenها بر اساس قوانین گرامر Python سازمان‌دهی می‌شوند.

خروجی این مرحله:

- Parse Tree
- یا Abstract Syntax Tree (AST)

است.

مثلاً Python بررسی می‌کند:

آیا این دستور از نظر ساختار درست است؟

<div dir="ltr" >

```python
if x > 10:
    print(x)
```

</div>

---

### ۳. تحلیل معنایی (Semantic Analysis)

در این مرحله، مفهوم کد بررسی می‌شود.

مواردی مثل:

- Scope متغیرها
- Resolution نام‌ها
- ارتباط بین بخش‌های مختلف کد

بررسی می‌شوند.

مثلاً:

<div dir="ltr">

```python
print(value)
```

</div>

Python باید بداند:

- آیا `value` وجود دارد؟
- از کدام Scope باید آن را پیدا کند؟

---

### ۴. تولید Bytecode (Bytecode Generation)

در مرحله آخر، AST به مجموعه‌ای از دستورهای Bytecode تبدیل می‌شود.

این دستورها همان چیزهایی هستند که PVM اجرا می‌کند.

---

## کامپایل Just-In-Time (JIT) و بهینه‌سازی‌های جدید

در نسخه‌های جدید Python، مخصوصاً Python 3.11 به بعد، CPython مکانیزمی به نام:

**Specializing Adaptive Interpreter**

معرفی کرد.

ایده اصلی این سیستم:

Python هنگام اجرا بررسی می‌کند کدام بخش‌های کد زیاد اجرا می‌شوند.

به این بخش‌ها می‌گوییم:

**Hot Code**

سپس دستورهای عمومی Bytecode را با نسخه‌های تخصصی‌تر جایگزین می‌کند.

مثلاً اگر Python متوجه شود یک عملیات همیشه روی عدد صحیح انجام می‌شود، می‌تواند اجرای آن را سریع‌تر کند.

---

### JIT Compiler در Python 3.13

در Python 3.13 یک JIT Compiler آزمایشی معرفی شد.

این JIT بر اساس معماری:

**Copy-and-Patch**

کار می‌کند.

هدف آن این است که بعضی از بخش‌های Bytecode را مستقیماً به Machine Code تبدیل کند.

نتیجه:

- کاهش فاصله سرعت Python با زبان‌های کامپایل‌شده
- اجرای سریع‌تر بخش‌های پرتکرار

البته این قابلیت هنوز در مراحل آزمایشی قرار دارد.

---

## بررسی Bytecode با ماژول dis

در Python می‌توانیم Bytecode تولیدشده را با ماژول داخلی:

<div dir="ltr" align="right">

```python
dis
```

</div>

مشاهده کنیم.

مثال:

<div dir="ltr">

```python
import dis

def example_func():
    # Constant folding: 15 * 20 is calculated at compile-time
    return 15 * 20


dis.dis(example_func)
```

</div>

خروجی:

<div dir="ltr">

```
3           0 LOAD_CONST               1 (300)
            2 RETURN_VALUE
```

</div>

---

## تحلیل خروجی Bytecode

اینجا دو دستور داریم:

### LOAD_CONST

مقدار ثابت را روی Stack داخلی Python قرار می‌دهد.

در این مثال:

<div dir="ltr">

```python
15 * 20
```

</div>

به جای اینکه هنگام اجرا محاسبه شود، قبلاً توسط Compiler محاسبه شده است:

<div dir="ltr">

```
15 * 20 = 300
```

</div>

پس Bytecode فقط مقدار:

<div dir="ltr" >

```
300
```

</div>

را Load می‌کند.

---

### RETURN_VALUE

این مقدار را به Caller برمی‌گرداند.

یعنی:

<div dir="ltr">

```python
return 300
```

</div>

---

## نکته مهم مصاحبه‌ای: Constant Folding

این مثال یک بهینه‌سازی مهم Python را نشان می‌دهد:

**Constant Folding**

یعنی Compiler محاسبات ثابت را قبل از اجرای برنامه انجام می‌دهد.

مثلاً:

<div dir="ltr">

```python
x = 10 * 20
```

</div>

ممکن است قبل از اجرا تبدیل شود به:

<div dir="ltr">

```python
x = 200
```

</div>

چون نتیجه از قبل مشخص است.

---

## خلاصه برای مصاحبه Python

اگر در مصاحبه پرسیدند:

### پایتون چگونه اجرا می‌شود؟

پاسخ کوتاه و حرفه‌ای:

> در Python، کد ابتدا به Bytecode کامپایل می‌شود، سپس Python Virtual Machine این Bytecode را تفسیر و اجرا می‌کند. به همین دلیل Python یک زبان کامپایل‌شده و مفسری است، نه صرفاً یک زبان مفسری. این معماری باعث قابل حمل بودن Python می‌شود، و بهینه‌سازی‌های جدید باعث افزایش سرعت اجرای آن شده‌اند.

---

### مسیر کامل اجرا:

<div dir="ltr">

```
.py File
   |
   v
Lexical Analysis
   |
   v
Parsing
   |
   v
AST
   |
   v
Bytecode Compilation
   |
   v
Python Virtual Machine
   |
   v
Execution
```

</div>
---
# 2. PEP 8 چیست و چرا مهم است؟


این استاندارد مجموعه‌ای از قوانین و پیشنهادها برای **فرمت‌بندی، ساختاردهی و سازمان‌دهی کد Python** ارائه می‌دهد تا کدها در اکوسیستم جهانی Python:

- خواناتر (**Readable**) باشند.
- یکپارچگی (**Consistency**) داشته باشند.
- راحت‌تر نگهداری (**Maintainable**) شوند.

---

## چرا PEP 8 مهم است؟

خالق پایتون Guido van Rossum،  جمله معروفی دارد:

> "Code is read much more often than it is written."

ترجمه:

> «کدها خیلی بیشتر از اینکه نوشته شوند، خوانده می‌شوند.»

یعنی در یک پروژه واقعی، برنامه‌نویس‌ها معمولاً زمان بیشتری را صرف خواندن و بررسی کدهای موجود می‌کنند تا نوشتن کد جدید.

رعایت PEP 8 باعث می‌شود:

- ذهن برنامه‌نویس کمتر درگیر ظاهر کد شود.
- تمرکز روی منطق برنامه باقی بماند.
- همکاری بین اعضای تیم ساده‌تر شود.
- باعث میشود Code Review سریع‌تر و دقیق‌تر انجام شود.

---

## اصول طراحی اصلی PEP 8

PEP 8 بر سه اصل اصلی تمرکز دارد:

### ۱. خوانایی (Readability)

هدف اصلی PEP 8 این است که کد برای انسان قابل فهم باشد.

اگر یک انتخاب در سبک نوشتن باعث شود کد سخت‌تر خوانده شود، **خوانایی مهم‌تر از آن قانون است.**

مثلاً:

کد ناخوانا:

<div dir="ltr" >

```python
x=[1,2,3];y=[i*2 for i in x];print(y)
```

</div>

کد خواناتر:

<div dir="ltr">

```python
numbers = [1, 2, 3]

doubled_numbers = [
    number * 2
    for number in numbers
]

print(doubled_numbers)
```

</div>

---

### ۲. یکپارچگی (Consistency)

در PEP 8 کمک می‌کند کدهای یک پروژه ظاهر مشابهی داشته باشند.

مثلاً اگر یک تیم همه متغیرها را با:

<div dir="ltr">

```
snake_case
```

</div>

بنویسد، خواندن کل پروژه برای همه آسان‌تر می‌شود.

---

### ۳. روش Pythonic

PEP 8 فلسفه Python را تقویت می‌کند:

> برای حل یک مسئله، باید یک روش واضح و ترجیحاً تنها یک روش صحیح وجود داشته باشد.

به این سبک می‌گوییم:

<div dir="ltr" >

```text
Pythonic Code
```

</div>

یعنی کدی که با فلسفه و استانداردهای Python هماهنگ است.

---

## قوانین فرمت و ساختار در PEP 8

### ۱. استفاده Indentation (تورفتگی)

پایتون  برای مشخص کردن بلوک‌های کد از indentation استفاده می‌کند.

قانون PEP 8:

✅ همیشه از **۴ فاصله (4 spaces)** استفاده کنید.

❌ از Tab استفاده نکنید.

مثال:

<div dir="ltr" >

```python
if user_logged_in:
    print("Welcome")
```

</div>

---

### ۲. طول خطوط (Line Length)

طبق PEP 8:

حداکثر طول هر خط باید:

<div dir="ltr" >

```text
79 characters
```

</div>

باشد.

دلیل:

- خواندن راحت‌تر کد
- نمایش بهتر در کنار فایل‌های دیگر
- سازگاری بهتر با ابزارهای مختلف

البته ابزارهای مدرن مانند:

- Black
- Ruff

معمولاً از طول:

<div dir="ltr" align="right">

```text
88 یا 100 کاراکتر
```

</div>

استفاده می‌کنند.

اما استاندارد سنتی PEP 8 همچنان:

<div dir="ltr" align="right">

```text
79 characters
```

</div>

است.

---

### ۳. خطوط خالی (Blank Lines)

برای جدا کردن بخش‌های مختلف کد:

**بین تعریف‌های سطح بالا:**

مانند:

- Class
- Function

از دو خط خالی استفاده می‌کنیم.

مثال:

<div dir="ltr" ">

```python
class User:
    pass


def login():
    pass
```

</div>

**بین Methodهای داخل یک Class:**

از یک خط خالی استفاده می‌کنیم.

مثال:

<div dir="ltr" >

```python
class User:

    def login(self):
        pass

    def logout(self):
        pass
```

</div>

---

## قوانین نام‌گذاری (Naming Styles)

در PEP 8 برای نام‌گذاری بخش‌های مختلف Python استاندارد مشخصی دارد.

### Classها

از:

<div dir="ltr" >

```text
CapWords
```

</div>

یا:

<div dir="ltr" align="right">

```text
PascalCase
```

</div>

استفاده می‌کنیم.

مثال:

<div dir="ltr" >

```python
class UserProfile:
    pass
```

</div>

### Function و Variableها

از:

<div dir="ltr" >

```text
lower_case_with_underscores
```

</div>

یا:

<div dir="ltr" >

```text
snake_case
```

</div>

استفاده می‌کنیم.

مثال:

<div dir="ltr" >

```python
user_name = "Ali"


def calculate_total_price():
    pass
```

</div>

### Constantها

از حروف بزرگ با underscore استفاده می‌شود.

مثال:

<div dir="ltr" >

```python
MAX_CONNECTIONS = 100
```

</div>

### Moduleها

نام Moduleها باید:

- کوتاه
- با حروف کوچک

باشند.

مثال:

<div dir="ltr" >

```python
database.py
```

</div>

استفاده از underscore توصیه نمی‌شود مگر در موارد ضروری.

---

## استفاده از فاصله‌ها (Whitespace)

### اطراف Operatorها

بین operatorها باید یک فاصله وجود داشته باشد.

صحیح:

<div dir="ltr" >

```python
x = 10

if x == 10:
    pass
```

</div>

غلط:

<div dir="ltr" >

```python
x=10

if x==10:
    pass
```

</div>

### داخل Parentheses و Brackets

نباید فاصله اضافی وجود داشته باشد.

صحیح:

<div dir="ltr" >

```python
numbers = [1, 2, 3]

print(numbers[0])
```

</div>

غلط:

<div dir="ltr" >

```python
numbers = [ 1, 2, 3 ]

print(numbers[ 0 ])
```

</div>

### بعد از Comma

بعد از comma باید یک فاصله قرار بگیرد.

صحیح:

<div dir="ltr" >

```python
items = ["apple", "banana", "orange"]
```

</div>

غلط:

<div dir="ltr" >

```python
items = ["apple","banana","orange"]
```

</div>

---

## مستندسازی (Documentation)

### Docstring

برای توضیح:

- Moduleهای عمومی
- Functionها
- Classها
- Methodها

از Docstring استفاده می‌شود.

طبق PEP 8 بهتر است از سه کوتیشن دوتایی استفاده شود:

<div dir="ltr" align="right">

```python
"""
This function calculates total price.
"""
```

</div>

### Comments

کامنت‌ها باید:

- به‌روز باشند.
- دلیل انجام کار را توضیح دهند، نه چیزی که واضح است.

مثال بد:

<div dir="ltr" >

```python
# increment i by 1
i += 1
```

</div>

این کامنت چیزی اضافه نمی‌کند.

مثال بهتر:

<div dir="ltr" >

```python
# Retry because the external API may fail temporarily
retry_count += 1
```

</div>

---

## مثال کامل مطابق PEP 8

این مثال از استانداردهای PEP 8 استفاده می‌کند و از `pathlib` که روش Pythonic برای کار با مسیرها است استفاده می‌کند.

<div dir="ltr" >

```python
from pathlib import Path


class DirectoryScanner:
    """Provides utilities for scanning file systems."""

    def __init__(self, target_directory: str):
        self.target_path = Path(target_directory)

    def scan_files(self) -> None:
        """Recursively iterates through directory and prints file paths."""
        if not self.target_path.is_dir():
            return

        for file_path in self.target_path.rglob("*"):
            if file_path.is_file():
                print(file_path.resolve())


if __name__ == "__main__":
    scanner = DirectoryScanner("/path/to/data")
    scanner.scan_files()
```

</div>

---

## نکات مهم PEP 8 برای مصاحبه Python


---

### سؤال 1: آیا رعایت PEP 8 اجباری است؟

پاسخ:

خیر.

در واقع PEP 8 یک قانون اجباری زبان Python نیست، بلکه یک استاندارد و guideline است.

پایتون کدی که مطابق PEP 8 نباشد را همچنان اجرا می‌کند.

اما در پروژه‌های حرفه‌ای معمولاً رعایت آن الزامی است.

---

### سؤال 2: ابزارهایی برای بررسی PEP 8 چیست؟

ابزارهای رایج:

- `flake8`
- `pycodestyle`
- `Ruff`
- `Black`

این ابزارها به صورت خودکار کد را بررسی و فرمت می‌کنند.

---

## خلاصه نهایی برای مصاحبه

اگر پرسیده شد:

### «پیپ 8 چیست و چرا مهم است؟»


>  در واقع PEP 8 راهنمای رسمی سبک Python است که قوانین مربوط به فرمت‌بندی، نام‌گذاری و ساختار کد را مشخص می‌کند. رعایت آن باعث افزایش خوانایی، یکپارچگی، همکاری تیمی و قابلیت نگهداری پروژه‌های Python می‌شود.

---

### نکته طلایی مصاحبه

نکته : PEP 8 فقط درباره زیبایی کد نیست؛ هدف اصلی آن:

<div dir="ltr" align="right">

```text
Readable Code → Maintainable Code → Better Collaboration
```

</div>

است.

یعنی:

```text
کد خواناتر
        ↓
نگهداری آسان‌تر
        ↓
همکاری بهتر در تیم
```
---
# مدیریت تخصیص حافظه و جمع‌آوری زباله (Garbage Collection) در پایتون چگونه انجام می‌شود؟

در پایتون، **تخصیص حافظه (Memory Allocation)** و **جمع‌آوری زباله (Garbage Collection)** به‌صورت خودکار توسط محیط اجرای پایتون (Runtime) مدیریت می‌شوند و این وظیفه عمدتاً بر عهدهٔ **Python Memory Manager** است. این سیستم عملیات پیچیدهٔ مربوط به حافظه را از دید برنامه‌نویس پنهان می‌کند و در نتیجه، هم امنیت را افزایش می‌دهد و هم فرایند توسعه را ساده‌تر می‌کند.

## تخصیص حافظه (Memory Allocation)

پایتون یک **Heap خصوصی (Private Heap)** دارد که تمام اشیا (Objects) و ساختارهای داده در آن قرار می‌گیرند. **Python Memory Manager** این Heap را از طریق چندین لایه مدیریت می‌کند:

* **تخصیص سلسله‌مراتبی (Hierarchical Allocation):** برای اشیای کوچک (حداکثر **۵۱۲ بایت**)، پایتون از تخصیص‌دهندهٔ ویژه‌ای به نام `obmalloc` استفاده می‌کند. این تخصیص‌دهنده حافظه را به **Arena**های ۲۵۶ کیلوبایتی تقسیم می‌کند؛ هر Arena به **Pool**های ۴ کیلوبایتی تقسیم می‌شود و Poolها نیز به **Block**ها تقسیم می‌شوند.
* **تخصیص‌دهنده‌های اختصاصی برای اشیا (Object-Specific Allocators):** برخی انواع داده، مانند `int` یا `list`، از **Free List**های اختصاصی استفاده می‌کنند تا سرعت تخصیص حافظه افزایش پیدا کند و **Fragmentation** (تکه‌تکه‌شدن حافظه) کاهش یابد.
* **حافظهٔ خام (Raw Memory):** اشیای بزرگ (معمولاً بزرگ‌تر از **۵۱۲ بایت**) از `obmalloc` عبور می‌کنند و مستقیماً از `malloc()` استاندارد زبان C برای درخواست حافظه از سیستم‌عامل استفاده می‌کنند.
* **Stack در مقابل Heap:** **Heap** اشیا و داده‌های واقعی را نگهداری می‌کند، در حالی که **Stack** ارجاع‌ها (References) به این اشیا و همچنین Frameهای مربوط به اجرای برنامه را نگهداری می‌کند.

## جمع‌آوری زباله (Garbage Collection)

پایتون از **Reference Counting (شمارش ارجاع)** به‌عنوان مکانیزم اصلی استفاده می‌کند و در کنار آن، یک **Generational Garbage Collector (جمع‌آور زبالهٔ نسلی)** برای مدیریت وابستگی‌های چرخه‌ای (Cyclic Dependencies) دارد.

### Reference Counting

هر شیء پایتون در Header خود دارای فیلدی به نام `ob_refcnt` است.

* وقتی شیء به یک متغیر نسبت داده می‌شود یا به یک Container اضافه می‌شود، تعداد Referenceهای آن افزایش پیدا می‌کند.
* وقتی یک Reference حذف می‌شود یا از Scope خارج می‌شود، این تعداد کاهش پیدا می‌کند.
* زمانی که `refcount == 0` شود، حافظهٔ شیء بلافاصله آزاد (Deallocate) می‌شود.
* نسخه‌های جدید پایتون (۳.۱۲ به بعد) همچنین از **Immortal Objects** استفاده می‌کنند؛ این اشیا دارای Reference Count ثابتی هستند تا عملکرد برنامه برای ثابت‌های مشترک (Shared Constants) بهبود پیدا کند.

```python
import sys

a = [1, 2, 3]
print(sys.getrefcount(a))  # Output: 2 (variable 'a' and the argument to getrefcount)
b = a
print(sys.getrefcount(a))  # Output: 3
```

### Generational Garbage Collector

برای آزاد کردن **Circular References (ارجاع‌های چرخه‌ای)**، یعنی زمانی که اشیا به یکدیگر اشاره می‌کنند، ماژول `gc` پایتون از یک الگوریتم تشخیص چرخه (Cycle-Detecting Algorithm) استفاده می‌کند.

* **نسل‌ها (Generations):** اشیا در سه نسل **G0، G1 و G2** دسته‌بندی می‌شوند.
* **ارتقا (Promotion):** اشیای جدید ابتدا در **G0** قرار می‌گیرند. اگر یک شیء از یک چرخهٔ Garbage Collection جان سالم به در ببرد، به **G1** منتقل می‌شود و در نهایت می‌تواند به **G2** ارتقا پیدا کند.
* **آستانه‌ها (Thresholds):** زمانی که تعداد تخصیص‌های حافظه منهای تعداد آزادسازی‌ها از یک **Threshold (آستانه)** مشخص بیشتر شود، فرایند Collection فعال می‌شود. **G0** با بیشترین دفعات بررسی می‌شود، در حالی که **G2** کمترین دفعات بررسی را دارد. این رفتار بر اساس این فرض است که اشیای جدیدتر احتمال بیشتری دارند که زودتر از بین بروند.

## مدیریت حافظه در پایتون در مقایسه با C

معماری پایتون، **امنیت را بر کنترل دستی** ترجیح می‌دهد:

* **انتزاع (Abstraction):** برخلاف C که برنامه‌نویس از `malloc()` و `free()` استفاده می‌کند، **Python Memory Manager** به‌صورت خودکار از **Memory Leak** و **Dangling Pointer** جلوگیری می‌کند.
* **سربار (Overhead):** اشیای پایتون سربار قابل‌توجهی دارند؛ هر شیء باید **Type Pointer** و **Reference Count** خود را ذخیره کند. یک `int` چهار بایتی در C به‌مراتب کوچک‌تر از یک شیء `int` در پایتون است.
* **کارایی (Performance):** مدیریت خودکار حافظه در پایتون می‌تواند در طول چرخه‌های **Generational GC** باعث توقف‌های گاه‌به‌گاه **Stop-the-World** شود؛ در حالی که C عملکرد قابل‌پیش‌بینی‌تری ارائه می‌دهد، اما این مزیت به قیمت مدیریت دستی حافظه به دست می‌آید.

---

## خلاصهٔ پاسخ برای مصاحبه

> در Python، مدیریت حافظه به‌صورت خودکار توسط **Python Memory Manager** انجام می‌شود. Objects داخل یک Private Heap قرار می‌گیرند و برای Objects کوچک، Python از `obmalloc` و برای Objects بزرگ‌تر معمولاً از `malloc()` استفاده می‌کند.
>
> برای Garbage Collection، مکانیزم اصلی **Reference Counting** است؛ یعنی وقتی Reference Count یک Object به صفر برسد، حافظهٔ آن آزاد می‌شود. برای **Circular References** که Reference Counting به‌تنهایی نمی‌تواند آن‌ها را تشخیص دهد، Python از **Generational Garbage Collector** با نسل‌های G0، G1 و G2 استفاده می‌کند.
>
> در نتیجه، Python مدیریت حافظه را برای برنامه‌نویس ساده و ایمن می‌کند، ولی در مقابل نسبت به C دارای Memory Overhead بیشتری است.

---

این مورد را هم می‌توانی با همان ساختار قبلی داخل فایل Markdown قرار بدهی. در انتها هم یک پاسخ کوتاه و مناسب برای بیان در مصاحبه اضافه کردم.

# انواع داده‌های داخلی (Built-in Data Types) در پایتون چه هستند؟

پایتون انواع مختلفی از **Built-in Data Types** را ارائه می‌دهد که هرکدام قابلیت‌ها و کاربردهای متفاوتی برای مدیریت کارآمد حافظه و دست‌کاری داده‌ها دارند. این انواع داده معمولاً بر اساس **Mutable** یا **Immutable** بودن دسته‌بندی می‌شوند.

## انواع دادهٔ تغییرناپذیر (Immutable Data Types)

### 1. `int`

برای نمایش **اعداد صحیح با دقت دلخواه (Arbitrary-Precision Integers)** استفاده می‌شود؛ مانند `42` یا `-10`.

در نسخه‌های مدرن پایتون، نوع `int` به‌صورت خودکار تمام اندازه‌های اعداد صحیح را مدیریت می‌کند و دیگر نیازی به نوع جداگانه‌ای مانند `long` وجود ندارد.

### 2. `float`

برای نمایش **اعداد ممیز شناور با دقت دوبرابر (Double-Precision Floating-Point Numbers)** استفاده می‌شود؛ مانند `3.14` یا `-0.001` و از استاندارد **IEEE 754** پیروی می‌کند.

### 3. `complex`

برای محاسبات ریاضی شامل بخش حقیقی و موهومی استفاده می‌شود و به شکل زیر نمایش داده می‌شود:

```text
z = a + bj
```

که در آن `j` واحد موهومی است.

### 4. `bool`

زیرکلاسی از `int` است که مقادیر منطقی را نمایش می‌دهد:

* `True` که در داخل برابر با `1` است.
* `False` که در داخل برابر با `0` است.

### 5. `str`

یک Sequence تغییرناپذیر (**Immutable**) از کاراکترهای **Unicode** است.

رشته‌های پایتون برای Performance بهینه شده‌اند و قابلیت‌های گسترده‌ای برای **Slicing** و **Formatting** دارند.

### 6. `tuple`

یک مجموعهٔ مرتب و تغییرناپذیر از آیتم‌ها است.

تاپل **Tuple**ها معمولاً برای نگهداری داده‌های ناهمگون (Heterogeneous Data) استفاده می‌شوند و **Hashable** هستند؛ بنابراین می‌توان از آن‌ها به‌عنوان Key در Dictionary استفاده کرد.

### 7. `frozenset`

نسخهٔ تغییرناپذیر `set` است.

فروزن ست `frozenset` قابلیت **Hashable** بودن دارد و شامل عناصر Unique است؛ بنابراین می‌تواند به‌عنوان Key در Dictionary یا به‌عنوان عنصری از یک Set دیگر استفاده شود.

### 8. `bytes`

یک Sequence تغییرناپذیر از Byteهای منفرد (مقادیر ۸ بیتی) است.

بیشتر برای کار با **Binary Data** مانند تصاویر، متن Encode‌شده یا Network Packets استفاده می‌شود.

### 9. `range`

یک Sequence تغییرناپذیر از اعداد است که معمولاً برای Iteration در Loopها استفاده می‌شود.

از نظر حافظه بهینه است، زیرا مقادیر را در حافظه ذخیره نمی‌کند و آن‌ها را در صورت نیاز محاسبه می‌کند (**Lazy Evaluation**).

### 10. `NoneType`

نوع مربوط به Singleton Object به نام **`None`** است.

در واقع`None` برای نشان دادن نبود مقدار، حالت Null یا یک وضعیت پیش‌فرض استفاده می‌شود.

---

## انواع دادهٔ تغییرپذیر (Mutable Data Types)

### 1. `list`

یک Collection پویا و مرتب از آیتم‌ها است.

لیست **List**ها بسیار انعطاف‌پذیر هستند و از Indexing، Slicing و تغییرات In-place مانند `append()` و `extend()` پشتیبانی می‌کنند.

### 2. `set`

یک Collection بدون ترتیب (Unordered) از عناصر Unique و **Hashable** است.

`set`ها برای بررسی عضویت (**Membership Testing**) به‌طور میانگین پیچیدگی زمانی `O(1)` دارند و عملیات ریاضی مانند **Intersection** و **Union** را نیز پشتیبانی می‌کنند.

### 3. `dict`

مجموعه‌ای از **Key-Value Pairs** است.

از Python 3.7 به بعد، حفظ **Insertion Order** در Dictionary به‌عنوان یک ویژگی تضمین‌شدهٔ زبان پایتون درآمده است و Lookup در آن به‌طور میانگین پیچیدگی `O(1)` دارد.

### 4. `bytearray`

نسخهٔ Mutable نوع `bytes` است.

امکان تغییر In-place داده‌های Binary را فراهم می‌کند، بدون اینکه برای هر تغییر نیاز به ساخت یک Object جدید باشد.

### 5. `memoryview`

یک نوع دسترسی عمومی به داده است که امکان دسترسی به دادهٔ داخلی Objectهایی که از **Buffer Protocol** پشتیبانی می‌کنند، مانند `bytes` و `bytearray`، را بدون Copy کردن داده فراهم می‌کند.

### 6. `array` (`array.array`)

برای ذخیره‌سازی کم‌حجم مقادیر پایه مانند Integer و Float استفاده می‌شود و تمام عناصر آن باید از **یک Type مشخص** باشند.

این نوع از طریق ماژول `array` در دسترس است و برای Datasetهای بزرگی از انواع دادهٔ ساده، نسبت به `list` حافظهٔ کمتری مصرف می‌کند.

### 7. `deque` (`collections.deque`)

یک **Double-Ended Queue** است که برای `append` و `pop` از هر دو انتها پیچیدگی زمانی `O(1)` دارد.

این نوع در ماژول `collections` ارائه شده و برای پیاده‌سازی Queue معمولاً انتخاب مناسب‌تری نسبت به `list` است.

### 8. `object`

بنیادی‌ترین Base Type در پایتون است.

تمام Classها به‌صورت مستقیم یا غیرمستقیم از `object` ارث‌بری می‌کنند و می‌توان از آن برای ساخت یک Object یکتا به‌عنوان **Sentinel** استفاده کرد.

### 9. `types.SimpleNamespace`

یک Object ساده است که امکان ایجاد Attributeهای دلخواه و دسترسی به آن‌ها با **Dot Notation** را فراهم می‌کند.

معمولاً به‌عنوان یک جایگزین سبک، Mutable و ساده برای یک Empty Class استفاده می‌شود.

### 10. `types.FunctionType`

نوع داخلی مربوط به Functionهای تعریف‌شده توسط کاربر است.

تابع یا Functionها در پایتون **First-Class Citizens** هستند؛ یعنی می‌توان آن‌ها را به‌عنوان Argument به Function دیگری ارسال کرد، از Function دیگری برگرداند یا به آن‌ها Attribute اختصاص داد.

## مثال
<div dir="ltr">

```python
# Illustrating core built-in types
integer_val = 10                        # int
string_val = "Python 2026"              # str
list_val = [1, 2, 3]                    # list (mutable)
tuple_val = (1, 2, 3)                   # tuple (immutable)
dict_val = {"key": "value"}             # dict (mapping)

# Using complex numbers and sets
complex_num = 2 + 3j                    # complex
unique_elements = {1, 2, 2, 3}          # set: {1, 2, 3}

# Binary and specialized types
raw_data = b"binary"                    # bytes
mutable_buffer = bytearray(raw_data)    # bytearray
```
</div>

---

## خلاصهٔ پاسخ برای مصاحبه

> در Python، Built-in Data Types را می‌توان به دو دستهٔ اصلی **Mutable** و **Immutable** تقسیم کرد.
>
> از انواع Immutable می‌توان به `int`، `float`، `complex`، `bool`، `str`، `tuple`، `frozenset`، `bytes`، `range` و `NoneType` اشاره کرد.
>
> انواع Mutable شامل `list`، `set`، `dict`، `bytearray` و `memoryview` هستند. همچنین Python انواع تخصصی‌تری مثل `array`، `deque` و `SimpleNamespace` هم دارد.
>
> تفاوت اصلی این است که **Immutable Objects بعد از ایجاد قابل تغییر نیستند**، اما **Mutable Objects را می‌توان بعد از ایجاد تغییر داد**.

---

این مورد را هم با همان فرمت قبلی، مناسب فایل Markdown و با **خلاصهٔ پاسخ قابل ارائه در مصاحبه** تنظیم کردم:

# مدیریت Exceptionها در پایتون چگونه انجام می‌شود؟

**Exception Handling** در پایتون یک مکانیزم ساختاریافته برای مدیریت خطاهای زمان اجرا (**Runtime Errors**) است که به برنامه اجازه می‌دهد خطاها را به‌شکل کنترل‌شده مدیریت کند و پایداری برنامه را حفظ کند.

این مکانیزم بر پایهٔ Syntax بلوکی است و امکان Catch کردن، پردازش و بازیابی از Exceptionها را فراهم می‌کند، بدون اینکه اجرای برنامه به‌صورت غیرمنتظره متوقف شود.

## اجزای اصلی (Core Components)

**در Exception Handling، چهار Block اصلی داریم:**

* **بلوک `try`**: کدی را شامل می‌شود که ممکن است باعث ایجاد Exception شود.
* **بلوک `except`**: زمانی اجرا می‌شود که در `try` یک Exception رخ دهد. بهتر است به‌جای استفاده از `except:` خالی، Exceptionهای مشخصی مانند `ValueError` را Catch کنیم.
* **بلوک `else`**: فقط زمانی اجرا می‌شود که Block مربوط به `try` بدون Exception به پایان برسد.
* **بلوک `finally`**: صرف‌نظر از نتیجه، همیشه اجرا می‌شود و معمولاً برای **Resource Cleanup**، مانند بستن File Descriptorها یا Database Connectionها، استفاده می‌شود.


```python id="m0v7e2"
try:
    file = open("data.txt", "r")
    data = int(file.read())
except FileNotFoundError:
    print("Error: File missing.")
except ValueError:
    print("Error: Invalid data format.")
else:
    print(f"Data processed: {data}")
finally:
    if 'file' in locals():
        file.close()
```

## Exception Groups و `except*`

از Python 3.11، **Exception Groups** با استفاده از `ExceptionGroup` امکان انتقال چند Exception مستقل را به‌صورت همزمان فراهم می‌کنند.

این قابلیت به‌خصوص برای Taskهای Asynchronous یا اجرای Concurrent، مانند `asyncio`، کاربرد دارد.

سینتکس Syntax مربوط به **`except*`** اجازه می‌دهد Exceptionهای مشخصی را از داخل یک Exception Group مدیریت کنیم.

```python id="nq7f2p"
try:
    raise ExceptionGroup(
        "Batch error",
        [
            ValueError("Invalid ID"),
            TypeError("Limit reached")
        ]
    )
except* ValueError as eg:
    for e in eg.exceptions:
        print(f"Handled ValueError: {e}")
except* TypeError as eg:
    for e in eg.exceptions:
        print(f"Handled TypeError: {e}")
```

## Context Managers و عبارت `with`

Keyword مربوط به `with` پروتکل **Context Manager** را پیاده‌سازی می‌کند و از متدهای `__enter__` و `__exit__` استفاده می‌کند.

این مکانیزم باعث می‌شود Resourceها به‌صورت خودکار آزاد شوند و Exceptionهایی که هنگام استفاده از Resource رخ می‌دهند نیز بدون نیاز به نوشتن صریح `finally` مدیریت شوند.

```python id="j5f2kq"
with open("log.txt", "a") as f:
    f.write("Log entry...")

# File is closed automatically, even if write fails.
```

## Raising و Chaining

با استفاده از `raise` می‌توان به‌صورت دستی یک Exception ایجاد کرد.

  و **Implicit و Explicit Chaining** امکان حفظ Traceback مربوط به علت اصلی خطا را فراهم می‌کنند. Keyword `from` برای Explicit Chaining استفاده می‌شود و در Debugging خطاهای پیچیده بسیار مفید است.

```python id="h8c4zn"
def fetch_api():
    try:
        return perform_request()
    except ConnectionError as e:
        # Explicitly chain the original error to a custom exception
        raise RuntimeError("API unavailable") from e
```

در این مثال، `RuntimeError` به‌عنوان خطای جدید ایجاد می‌شود، اما Exception اصلی یعنی `ConnectionError` نیز به‌عنوان علت آن حفظ می‌شود.

## غنی‌سازی Exception و Global Hooks

در نسخه‌های جدید پایتون می‌توان با استفاده از متد `add_note()` به Exceptionها اطلاعات و Metadata بیشتری اضافه کرد.

این قابلیت از Python 3.11 معرفی شده و برای اضافه کردن Context به Exception بدون تغییر Message اصلی آن کاربرد دارد.

### Global Hooks

برای Exceptionهایی که Handle نشده‌اند، `sys.excepthook` امکان تعریف یک Handler سراسری را فراهم می‌کند.

از این قابلیت می‌توان برای **Logging** یا **Telemetry** قبل از خروج Interpreter استفاده کرد.

```python id="k2v6ms"
import sys

def global_logger(exctype, value, tb):
    print(f"CRITICAL: {value}")

sys.excepthook = global_logger
```

## کنترل جریان: `pass` و `continue`

در داخل یک `except` block:

*  عبارت **`pass`**: در واقغ  Exception را بدون انجام هیچ کاری نادیده می‌گیرد. استفاده از آن باید با احتیاط انجام شود.
* عبارت **`continue`**: در یک Loop، Iteration فعلی را رد می‌کند و باعث می‌شود پردازش Iteration بعدی ادامه پیدا کند.

این روش برای مثال زمانی مفید است که بخواهیم یک Record خراب را نادیده بگیریم و پردازش بقیهٔ Dataset ادامه پیدا کند.

```python id="q4d8ws"
for item in raw_data:
    try:
        process(item)
    except DataEntryError:
        continue  # Skip corrupted record and move to next
```

---

## خلاصهٔ پاسخ برای مصاحبه

> در Python، Exception Handling معمولاً با چهار Block اصلی انجام می‌شود: **`try`، `except`، `else` و `finally`**.
>
> کدی که ممکن است خطا ایجاد کند داخل `try` قرار می‌گیرد، `except` برای Handle کردن Exception استفاده می‌شود، `else` فقط در صورت موفقیت `try` اجرا می‌شود و `finally` برای Cleanup منابع استفاده می‌شود و تقریباً در هر شرایطی اجرا خواهد شد.
>
> بهتر است همیشه Exceptionهای مشخص مثل `ValueError` یا `FileNotFoundError` را Catch کنیم و از `except:` خالی تا حد امکان استفاده نکنیم.
>
> برای مدیریت منابع می‌توان از **Context Manager** و `with` استفاده کرد که Resourceهایی مثل File را به‌صورت خودکار Cleanup می‌کند.
>
> همچنین در Python 3.11 قابلیت‌هایی مثل **ExceptionGroup** و `except*` برای مدیریت چند Exception به‌صورت همزمان، و `add_note()` برای اضافه کردن اطلاعات بیشتر به Exceptionها معرفی شده‌اند. برای ایجاد و انتقال خطا نیز از `raise` و در صورت نیاز از `raise ... from ...` برای **Exception Chaining** استفاده می‌کنیم.

---

# تفاوت `==` و `is` در پایتون چیست؟

هر دو Operator یعنی **`==`** و **`is`** برای مقایسه در پایتون استفاده می‌شوند، اما نوع مقایسه‌ای که انجام می‌دهند متفاوت است.

## تفاوت `==` و `is`

* **عملگر `==`**: برابری مقدار (**Value Equality**) را بررسی می‌کند. یعنی بررسی می‌کند که دادهٔ موجود در دو Object از نظر مقدار با یکدیگر برابر باشند. این کار معمولاً با فراخوانی متد `__eq__` انجام می‌شود.
* **عملگر `is`**: هویت Object (**Object Identity**) را بررسی می‌کند. یعنی مشخص می‌کند که آیا دو Variable دقیقاً به **یک Instance یکسان در حافظه** اشاره می‌کنند یا خیر.

هر Object در پایتون یک Identifier منحصربه‌فرد دارد که توسط Interpreter به آن اختصاص داده می‌شود. عملگر `is` بررسی می‌کند که آیا دو Variable دقیقاً به یک Object اشاره می‌کنند یا نه.

اگر:

```text
id(a) == id(b)
```

باشد، آنگاه:

```python
a is b
```

برابر با `True` خواهد بود.

## منطق مقایسه

* **مقایسه با `is`**: هویت (**Identity**) دو Object را بررسی می‌کند؛ یعنی آیا هر دو Reference به یک Object یکسان اشاره می‌کنند یا خیر.
* **مقایسه با `==`**: مقدار (**Value**) دو Object را مقایسه می‌کند و بررسی می‌کند که آیا محتوای آن‌ها از نظر منطقی برابر است یا خیر.

## مثال

```python id="s7k2qa"
# Initialize two lists with identical values
list_a = [1, 2, 3]
list_b = [1, 2, 3]
list_c = list_a

print(list_a == list_b)  # True: The values are the same
print(list_a is list_b)  # False: They are different objects in memory
print(list_a is list_c)  # True: Both point to the same object
```

در این مثال:

* `list_a == list_b` برابر `True` است، چون مقدار هر دو List یکسان است.
* `list_a is list_b` برابر `False` است، چون این دو List، دو Object متفاوت هستند.
* `list_a is list_c` برابر `True` است، چون هر دو Variable به یک Object یکسان اشاره می‌کنند.

## بهترین روش استفاده

* **عملگر `==`**: برای مقایسهٔ معمول برابری استفاده می‌شود؛ مثلاً هنگام مقایسهٔ Stringها، Numberها یا Data Structureها، زمانی که مقدار و محتوای Object اهمیت دارد.
* **عملگر `is`**: بهتر است برای مقایسه با **Singleton**ها استفاده شود؛ رایج‌ترین مورد آن بررسی `None` است:

```python
if val is None:
    ...
```

برای مقایسهٔ Literalهایی مانند Integer یا String نباید به `is` تکیه کرد، زیرا ممکن است به رفتارهای وابسته به Implementation مانند **Interning** وابسته شود.

---

## خلاصهٔ پاسخ برای مصاحبه

> تفاوت اصلی `==` و `is` در نوع مقایسه‌ای است که انجام می‌دهند. `==` **Value Equality** را بررسی می‌کند؛ یعنی آیا مقدار دو Object برابر است یا نه. اما `is` **Object Identity** را بررسی می‌کند؛ یعنی آیا دو Variable دقیقاً به یک Object یکسان در حافظه اشاره می‌کنند یا خیر.
>
> مثلاً ممکن است دو List مقدار یکسانی داشته باشند و `==` برای آن‌ها `True` باشد، اما چون دو Object متفاوت هستند، `is` برای آن‌ها `False` خواهد بود.
>
> به‌طور معمول از `==` برای مقایسهٔ مقادیر و از `is` برای بررسی Singletonهایی مثل `None` استفاده می‌کنیم؛ مثلاً `value is None`.

---

این مورد را هم با همان قالب قبلی، با ترجمهٔ وفادار و روان و در انتها با **پاسخ کوتاه مناسب مصاحبه** آماده کردم.

# تابع پایتون چگونه کار می‌کند؟

**توابع پایتون (Python Functions)**، **First-Class Objects** هستند که منطق برنامه را در خود کپسوله می‌کنند، امکان استفادهٔ مجدد از کد را فراهم می‌کنند و از طریق Scope، وضعیت و دسترسی به متغیرها را مدیریت می‌کنند.

در پایتون، یک Function نمونه‌ای از کلاس `function` است؛ بنابراین می‌توان آن را به‌عنوان Argument به Function دیگری ارسال کرد، از یک Function دیگر برگرداند یا در یک Variable ذخیره کرد.

## اجزای اصلی (Key Components)

* **امضای تابع (Function Signature):** با Keyword مربوط به `def` تعریف می‌شود و شامل نام Function، پارامترها (Positional، Keyword-only یا Variadic) و در صورت نیاز **Type Hints** برای Static Analysis است.

* **بدنهٔ تابع (Function Body):** یک Block کد با Indentation است که منطق Function را شامل می‌شود. پایتون این کد را هنگام تعریف Function به **Bytecode** تبدیل می‌کند که در Attribute مربوط به `__code__` قرار می‌گیرد.

* **دستور `return`:** اجرای Function را به‌صورت صریح با یک مقدار به پایان می‌رساند. اگر `return` وجود نداشته باشد، Function به‌صورت ضمنی Singleton مربوط به `None` را برمی‌گرداند.

* **شیء تابع Function Object:** هنگام تعریف Function، پایتون یک Name Binding در Namespace فعلی ایجاد می‌کند که به Function Object ذخیره‌شده در حافظه اشاره می‌کند.

## فرایند اجرای تابع (Execution Process)

وقتی یک Function فراخوانی می‌شود، Python Interpreter مراحل زیر را انجام می‌دهد:

### 1. ایجاد Frame

یک **Frame Object** روی **Call Stack** قرار می‌گیرد.

این Frame محیط اجرای Function را در خود نگه می‌دارد؛ از جمله Local Symbol Table و Evaluation Stack.

### 2. اتصال پارامترها (Parameter Binding)

در واقع Arguments با استفاده از مدل **Pass-by-Object-Reference** که با نام **Pass-by-Assignment** نیز شناخته می‌شود، به Parameters متصل می‌شوند.

اگر یک Mutable Object مانند `list` به Function ارسال شود، تغییرات ایجادشده روی آن داخل Function، روی همان Object اصلی نیز اثر می‌گذارد.

### 3. اجرای Bytecode

در واقع **Python Virtual Machine (PVM)**، میاد Bytecode را اجرا می‌کند.

از Python 3.11 به بعد، **Specializing Adaptive Interpreter** می‌تواند Bytecode را در زمان اجرا بهینه‌سازی کند تا Performance افزایش پیدا کند.

### 4. بازگشت و Cleanup

وقتی `return` اجرا شود یا اجرای Function به انتهای Block برسد، مقدار Return به Context فراخواننده برگردانده می‌شود، Frame از Call Stack خارج می‌شود و Local Variables می‌توانند از طریق **Reference Counting** در معرض **Garbage Collection** قرار بگیرند.

## Scope و Variable Resolution

پایتون برای پیدا کردن Nameها هنگام اجرای برنامه از **LEGB Rule** استفاده می‌کند.

### قانون LEGB

برای پیدا کردن Nameها هنگام اجرای برنامه، پایتون از LEGB Rule استفاده می‌کند:

محدودهٔ محلی (Local): Nameهایی که داخل Function تعریف شده‌اند و به‌عنوان global اعلام نشده‌اند.

محدودهٔ بیرونی (Enclosing): Nameهایی که در Local Scope مربوط به Functionهای بیرونی قرار دارند و در Closures اهمیت دارند.

محدودهٔ سراسری (Global): Nameهایی که در سطح بالای Module تعریف شده‌اند یا با استفاده از Keyword مربوط به global مشخص شده‌اند.

محدودهٔ داخلی پایتون (Built-in): Nameهایی که از قبل در Module مربوط به builtins تعریف شده‌اند؛ مانند len و range.

## مثال

```python id="t8j4qm"
def outer_function(x: int):
    # Enclosing scope
    y = 10

    def inner_function(z: int) -> float:
        # Local scope accessing Enclosing (x, y) and Global
        return (x + y + z) * 1.0

    return inner_function


# Usage
closure = outer_function(5)
result = closure(3)  # Result: 18.0
```

در این مثال، `inner_function` به متغیرهای `x` و `y` از Scope مربوط به Function بیرونی دسترسی دارد. این ویژگی نمونه‌ای از **Closure** است.

## مکانیزم‌های پیشرفتهٔ Function

* **توابع بسته (Closures)** Functionهایی هستند که مقادیر موجود در Enclosing Lexical Scope خود را حتی پس از پایان اجرای Function بیرونی به خاطر می‌سپارند. این اطلاعات در Attribute مربوط به `__closure__` ذخیره می‌شود.
* **دکوراتورها (Decorators)** Higher-Order Functionهایی هستند که یک Function را به‌عنوان Argument دریافت کرده و یک Function جدید برمی‌گردانند. معمولاً برای اضافه کردن Behavior جدید بدون تغییر Source Code اصلی Function استفاده می‌شوند.
* **محدودیت بازگشت بازگشتی (Recursion Limits)** پایتون برای جلوگیری از مصرف بیش از حد C Stack در Recursive Callهای بی‌نهایت، یک **Maximum Recursion Depth** دارد که مقدار پیش‌فرض آن معمولاً حدود `1000` است؛ البته Interpreterهای مدرن مدیریت Frameها را به شکل کارآمدتری انجام می‌دهند.

## جلوگیری از Side Effectها

در حالت ایده‌آل، Functionها باید **Pure** باشند؛ یعنی خروجی آن‌ها فقط بر اساس Inputها تعیین شود و State سراسری برنامه را تغییر ندهند.

استفاده از Keywordهای `nonlocal` یا `global` امکان تغییر Scopeهای بیرونی را فراهم می‌کند، اما باعث افزایش Complexity و کاهش Predictability می‌شود.

کپسوله کردن منطق برنامه داخل Functionها باعث می‌شود داده‌ای که توسط `f(x)` پردازش می‌شود تا حد امکان ایزوله باقی بماند و در نتیجه **Maintainability** برنامه افزایش پیدا کند.

---

## خلاصهٔ پاسخ برای مصاحبه

> در Python، Functionها **First-Class Objects** هستند؛ یعنی می‌توان آن‌ها را داخل Variable ذخیره کرد، به‌عنوان Argument ارسال کرد یا از یک Function دیگر برگرداند.
>
> وقتی یک Function تعریف می‌شود، Python کد آن را به **Bytecode** تبدیل می‌کند و یک Function Object ایجاد می‌کند. هنگام فراخوانی، یک **Frame** روی Call Stack ساخته می‌شود، Arguments به Parameters متصل می‌شوند و Python Virtual Machine، Bytecode را اجرا می‌کند.
>
> برای پیدا کردن Variableها و Nameها، Python از **LEGB Rule** یعنی Local، Enclosing، Global و Built-in استفاده می‌کند.
>
> همچنین Functionها می‌توانند قابلیت‌هایی مثل **Closure** و **Decorator** داشته باشند. Closure باعث می‌شود Function بتواند مقادیر Scope بیرونی خود را حتی بعد از پایان اجرای Function بیرونی حفظ کند.


</div>
