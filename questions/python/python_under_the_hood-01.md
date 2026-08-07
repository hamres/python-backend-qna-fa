<div dir="rtl" lang="fa" style="text-align: right; font-family: Tahoma, 'B Nazanin', sans-serif;">

# ۱. پایتون چگونه اجرا می‌شود؟

کد منبع **Python** به‌صورت مستقیم توسط CPU اجرا نمی‌شود. در عوض، قبل از اجرا یک فرآیند چندمرحله‌ای را طی می‌کند که شامل ترکیبی از **کامپایل (Compilation)** و **تفسیر (Interpretation)** است.

در مصاحبه‌های Python معمولاً یکی از سؤال‌های پایه این است:

> «آیا Python یک زبان کامپایل‌شده است یا مفسری؟»

پاسخ دقیق این است:

**Python یک زبان کامپایل‌شده و مفسری (Hybrid) است.**

یعنی کد Python ابتدا به یک فرمت میانی به نام **Bytecode** کامپایل می‌شود و سپس این Bytecode توسط مفسر Python اجرا می‌شود.

---

## کامپایل و تفسیر (Compilation & Interpretation)

Python از یک مدل ترکیبی استفاده می‌کند تا بین **قابلیت اجرا روی سیستم‌های مختلف (Portability)** و **سادگی توسعه (Ease of Development)** تعادل برقرار کند.

### ۱. کامپایل به Bytecode

وقتی یک فایل Python اجرا می‌شود، اولین مرحله این است که مفسر Python، کد سطح بالای نوشته‌شده توسط برنامه‌نویس را به **Bytecode** تبدیل می‌کند.

Bytecode یک نمایش میانی از کد است که:

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

**"Does Python compile code?"**

نگویید:

❌ No, Python is only interpreted.

پاسخ صحیح:

✅ Yes. Python source code is compiled into bytecode first, then the bytecode is interpreted by the Python Virtual Machine.

---

## ۲. ماشین مجازی Python (Python Virtual Machine - PVM)

بعد از تولید Bytecode، مرحله بعدی اجرای آن است.

این کار توسط **Python Virtual Machine (PVM)** انجام می‌شود.

PVM موتور اجرایی Python است که:

۱. Bytecode را دریافت می‌کند.
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

Machine Code یعنی دستورهایی که CPU مستقیماً می‌تواند اجرا کند.

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

Bytecode یک سطح بالاتر از Machine Code قرار دارد.

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

Tokenها شامل مواردی مثل:

- Keywordها
- Variable nameها
- Literalها

هستند.

مثلاً:

<div dir="ltr" align="right">

```python
x = 10
```

</div>

به چیزی شبیه این تبدیل می‌شود:

<div dir="ltr" align="right">

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

<div dir="ltr" align="right">

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

<div dir="ltr" align="right">

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

<div dir="ltr" align="right">

```python
import dis

def example_func():
    # Constant folding: 15 * 20 is calculated at compile-time
    return 15 * 20


dis.dis(example_func)
```

</div>

خروجی:

<div dir="ltr" align="right">

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

<div dir="ltr" align="right">

```python
15 * 20
```

</div>

به جای اینکه هنگام اجرا محاسبه شود، قبلاً توسط Compiler محاسبه شده است:

<div dir="ltr" align="right">

```
15 * 20 = 300
```

</div>

پس Bytecode فقط مقدار:

<div dir="ltr" align="right">

```
300
```

</div>

را Load می‌کند.

---

### RETURN_VALUE

این مقدار را به Caller برمی‌گرداند.

یعنی:

<div dir="ltr" align="right">

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

<div dir="ltr" align="right">

```python
x = 10 * 20
```

</div>

ممکن است قبل از اجرا تبدیل شود به:

<div dir="ltr" align="right">

```python
x = 200
```

</div>

چون نتیجه از قبل مشخص است.

---

## خلاصه برای مصاحبه Python

اگر در مصاحبه پرسیدند:

### Python چگونه اجرا می‌شود؟

پاسخ کوتاه و حرفه‌ای:

> Python source code is first compiled into bytecode. This bytecode is then executed by the Python Virtual Machine. Unlike C or C++, Python does not compile directly into machine code. This design provides portability, while modern optimizations like adaptive interpretation and JIT compilation improve performance.

---

### مسیر کامل اجرا:

<div dir="ltr" align="right">

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

</div>
