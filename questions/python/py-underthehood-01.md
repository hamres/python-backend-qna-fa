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

</div>
