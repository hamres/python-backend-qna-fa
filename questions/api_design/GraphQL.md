

# 1. GraphQL چیست؟ — به زبان ساده

GraphQL یک **زبان query برای API** است.

یعنی کلاینت می‌تواند دقیقاً بگوید:

- چه داده‌هایی نیاز دارد؟
- با چه ساختاری نیاز دارد؟
- چه فیلدهایی را می‌خواهد؟
- چه ارتباطاتی بین داده‌ها نیاز دارد؟

و سرور بر اساس همان درخواست، داده را برمی‌گرداند.

در REST معمولاً سرور تصمیم می‌گیرد response چه شکلی داشته باشد.

مثلاً یک endpoint:

```http
GET /api/tasks/1/
```

ممکن است این response را برگرداند:

```json
{
  "id": 1,
  "title": "Write report",
  "description": "Long description...",
  "status": "pending",
  "created_at": "2024-01-01",
  "updated_at": "2024-01-02",
  "owner": 5
}
```

اما شاید کلاینت فقط این‌ها را لازم داشته باشد:

```text
id
title
status
```

در REST، کلاینت معمولاً نمی‌تواند بگوید فقط این فیلدها را بده.  
یا باید endpoint جداگانه داشته باشیم، یا داده‌ی اضافی دریافت کنیم.

در GraphQL کلاینت شکل داده را مشخص می‌کند:

```graphql
query {
  task(id: 1) {
    id
    title
    status
  }
}
```

یعنی:

> من task با شناسه‌ی 1 را می‌خواهم، اما فقط فیلدهای id و title و status را نیاز دارم.

---

# 2. GraphQL دقیقاً چه مشکلی را حل می‌کند؟

GraphQL چند مشکل رایج REST را حل می‌کند.

---

## مشکل اول: Over-fetching

یعنی کلاینت داده‌ی بیشتر از نیازش دریافت کند.

مثلاً برای نمایش لیست taskها، REST ممکن است این‌ها را برگرداند:

```json
[
  {
    "id": 1,
    "title": "Task A",
    "description": "...",
    "created_at": "...",
    "updated_at": "...",
    "owner": {
      "id": 10,
      "username": "ali",
      "email": "ali@example.com"
    }
  }
]
```

اما UI فقط این‌ها را لازم دارد:

```text
task.id
task.title
owner.username
```

در GraphQL کلاینت فقط همین‌ها را درخواست می‌کند.

---

## مشکل دوم: Under-fetching

یعنی کلاینت برای ساخت یک صفحه مجبور است چند request بزند.

مثلاً برای صفحه‌ی پروفایل کاربر:

در REST ممکن است لازم باشد:

```http
GET /api/users/1/
GET /api/users/1/tasks/
GET /api/users/1/profile/
```

اما در GraphQL می‌توان همه را در یک query گرفت:

```graphql
query {
  user(id: 1) {
    username
    email
    tasks {
      id
      title
      status
    }
  }
}
```

یعنی یک درخواست، ولی داده‌های مرتبط با هم.

---

## مشکل سوم: تعدد endpointها

در REST معمولاً برای هر منبع endpoint داریم:

```text
GET /api/users/
GET /api/users/1/
GET /api/users/1/tasks/
GET /api/tasks/
GET /api/tasks/1/
POST /api/tasks/
PATCH /api/tasks/1/
DELETE /api/tasks/1/
```

در GraphQL معمولاً یک endpoint داریم:

```text
POST /graphql
```

اما داخل همان endpoint، operationهای مختلف اجرا می‌شوند:

```graphql
query {
  task(id: 1) {
    title
  }
}
```

یا:

```graphql
mutation {
  createTask(input: { title: "New task" }) {
    id
    title
  }
}
```

---

# 3. آیا GraphQL جایگزین REST است؟

لزوماً نه.

GraphQL می‌تواند:

- جایگزین REST باشد.
- کنار REST استفاده شود.
- فقط برای بخش‌هایی از API استفاده شود.

در پروژه‌ی ما، GraphQL را کنار REST نگه می‌داریم تا تفاوت‌ها را عمیق ببینی.

یعنی بعضی قابلیت‌ها را با REST پیاده می‌کنیم، بعد همان قابلیت را با GraphQL هم پیاده می‌کنیم.

---

# 4. مقایسه‌ی سریع REST و GraphQL

| موضوع | REST | GraphQL |
|---|---|---|
| دسترسی به داده | معمولاً چند endpoint | معمولاً یک endpoint |
| شکل response | سرور تصمیم می‌گیرد | کلاینت تعیین می‌کند |
| دریافت داده‌های مرتبط | اغلب چند request | امکان nested query |
| تغییر داده | POST / PUT / PATCH / DELETE | Mutation |
| اعتبارسنجی | Serializer / Validator | Schema + Resolver + Input |
| مستندات | Swagger/OpenAPI | Schema / Introspection |
| caching | راحت‌تر با HTTP cache | پیچیده‌تر، نیازمند طراحی |
| امنیت | endpoint-based | resolver/field-based |
| نسخه‌بندی | مثلاً `/api/v1/` | معمولاً schema evolution |

---

# 5. مثال ساده: دریافت یک Task

---

## در REST

```http
GET /api/tasks/1/
```

ممکن است response کامل task را برگرداند.

مثلاً:

```json
{
  "id": 1,
  "title": "Learn GraphQL",
  "description": "Very long description",
  "status": "pending",
  "created_at": "2024-01-01",
  "owner": 2
}
```

---

## در GraphQL

```graphql
query {
  task(id: 1) {
    id
    title
    status
  }
}
```

response فقط شامل همان فیلدهایی است که کلاینت خواسته:

```json
{
  "data": {
    "task": {
      "id": 1,
      "title": "Learn GraphQL",
      "status": "pending"
    }
  }
}
```

این یکی از مهم‌ترین تفاوت‌هاست:

> در REST، سرور shape داده را تعیین می‌کند.  
> در GraphQL، کلاینت shape داده را تعیین می‌کند.

---

# 6. مثال مهم‌تر: Relationship

فرض کن می‌خواهیم اطلاعات یک task و owner آن را بگیریم.

---

## در REST

ممکن است نیاز داشته باشیم:

```http
GET /api/tasks/1/
```

بعد ببینیم owner_id چیست:

```http
GET /api/users/2/
```

یا شاید endpoint داشته باشیم:

```http
GET /api/tasks/1/owner/
```

---

## در GraphQL

می‌توانیم یک‌جا بگوییم:

```graphql
query {
  task(id: 1) {
    id
    title
    status
    owner {
      id
      username
      email
    }
  }
}
```

این query می‌گوید:

> task را بده، و داخل آن، owner آن task را هم با این فیلدها بده.

اینجاست که قدرت GraphQL مشخص می‌شود.

---

# 7. تعریف مفاهیم پایه — فعلاً در حد درک ذهنی

---

## Schema

Schema قرارداد بین کلاینت و سرور است.

در Schema مشخص می‌کنیم:

- چه Typeهایی داریم؟
- هر Type چه فیلدهایی دارد؟
- چه Queryهایی وجود دارد؟
- چه Mutationهایی وجود دارد؟
- چه Inputهایی وجود دارد؟

در REST این قرارداد بیشتر implicit است یا با OpenAPI/Swagger مستند می‌شود.

در GraphQL، Schema بخشی اصلی و اجرایی سیستم است.

---

## Type

Type شکل یک object را مشخص می‌کند.

مثلاً:

```graphql
type Task {
  id: ID!
  title: String!
  status: String!
}
```

یعنی Task این فیلدها را دارد.

در Django/DRF، این شبیه به model + serializer است، اما با تفاوت‌های مهم.

---

## Field

هر property داخل Type یک Field است.

در مثال بالا:

```graphql
id
title
status
```

فیلدهای Task هستند.

---

## Query

Query برای خواندن داده استفاده می‌شود.

معادل REST:

```http
GET /api/tasks/
```

یا:

```http
GET /api/tasks/1/
```

در GraphQL:

```graphql
query {
  tasks {
    id
    title
  }
}
```

---

## Mutation

Mutation برای تغییر داده استفاده می‌شود.

معادل REST:

```http
POST /api/tasks/
PATCH /api/tasks/1/
DELETE /api/tasks/1/
```

در GraphQL:

```graphql
mutation {
  createTask(title: "New Task") {
    id
    title
  }
}
```

نکته مهم:

> در GraphQL، create/update/delete همه معمولاً Mutation هستند.

چون Mutation یعنی operationای که داده را تغییر می‌دهد.

---

## Resolver

Resolver تابعی است که مقدار یک Field را برمی‌گرداند.

وقتی کلاینت می‌پرسد:

```graphql
task(id: 1) {
  title
}
```

سرور باید بداند `task` از کجا بیاید و `title` چگونه پر شود.

اینجا Resolverها وارد می‌شوند.

در Django/DRF، معادل تقریبی resolver می‌تواند view method یا serializer field باشد، اما در GraphQL resolver مفهوم مرکزی‌تری دارد.

---

## Argument

Argument ورودی یک Field است.

مثلاً:

```graphql
task(id: 1) {
  title
}
```

در اینجا `id: 1` یک Argument برای fieldای به نام `task` است.

معادل REST:

```http
GET /api/tasks/1/
```

یا:

```http
GET /api/tasks/?status=pending
```

---

## Input Type

Input Type برای تعریف objectهای ورودی استفاده می‌شود.

مثلاً برای ساخت task:

```graphql
input CreateTaskInput {
  title: String!
  description: String
}
```

بعد:

```graphql
mutation {
  createTask(input: { title: "Learn GraphQL" }) {
    id
    title
  }
}
```

در REST این شبیه body یک POST request است.

---

## Context

Context یک object مرتبط با request فعلی است.

مثلاً می‌تواند شامل:

- request
- user
- token
- database session
- serviceها

باشد.

در GraphQL، context نقش مهمی در authentication و authorization دارد.

معادل ذهنی در DRF:

```python
request.user
```

یا permission classes.

---

# 8. معماری پیشنهادی پروژه

ما پروژه را طوری می‌سازیم که REST و GraphQL هر دو از یک لایه‌ی business استفاده کنند.

یعنی:

```text
Client
  |
  |--> REST APIRouter --> Service --> ORM --> Database
  |
  |--> GraphQL Resolver --> Service --> ORM --> Database
```

این معماری خیلی مهم است.

اگر GraphQL مستقیماً به ORM وصل شود، به‌سرعت شلوغ و غیرقابل نگهداری می‌شود.

پس ساختار هدف ما این است:

```text
GraphQL Query/Mutation
        |
     Resolver
        |
     Service
        |
  SQLAlchemy / ORM
        |
     Database
```

و برای REST:

```text
URL
  |
APIRouter
  |
Pydantic Schema / Validation
  |
Service
  |
SQLAlchemy / ORM
  |
Database
```

این معماری باعث می‌شود:

- منطق business تکرار نشود.
- تست‌ها ساده‌تر شوند.
- authorization یک‌جا مدیریت شود.
- تفاوت REST و GraphQL را بهتر ببینی.

---

# 9. پروژه‌ی آموزشی پیشنهادی

پیشنهاد من این است که همان **Task Management** را بسازیم.

دلایل:

- User و Task relationship طبیعی دارند.
- Authorization معنادار می‌شود:
  - هر کاربر فقط taskهای خودش را ببیند.
  - admin همه را ببیند.
- CRUD کامل دارد.
- Filtering و Pagination برایش طبیعی است.
- برای GitHub هم پروژه‌ی قابل فهمی است.

اگر ترجیح بدهی Book Management هم می‌توانیم کار کنیم، ولی Task Management با ساختار قبلی پروژه‌ات سازگارتر است.

---

## موجودیت‌های اصلی

### User

فیلدهای احتمالی:

```text
id
username
email
password
role
is_active
created_at
```

### Task

فیلدهای احتمالی:

```text
id
title
description
status
priority
owner_id
created_at
updated_at
```

---

# 10. Milestoneهای یادگیری

این مسیر را مرحله‌به‌مرحله طی می‌کنیم.

---

## Milestone 1: درک ذهنی GraphQL

هدف:

- بفهمی GraphQL چیست.
- تفاوتش با REST را درک کنی.
- Query و Field و Argument را بشناسی.
- بتوانی یک GraphQL query ساده را روی کاغذ طراحی کنی.

در این مرحله فعلاً کد پروژه نمی‌نویسیم.

---

## Milestone 2: اولین GraphQL Schema با داده‌ی static

هدف:

- اولین GraphQL endpoint را بسازیم.
- یک Type ساده مثل Task تعریف کنیم.
- یک Query ساده مثل `tasks` یا `task(id)` داشته باشیم.
- Resolver را ببینیم.

در این مرحله هنوز دیتابیس نداریم.  
داده‌ها را موقتاً در memory نگه می‌داریم.

---

## Milestone 3: Mutation و تغییر داده

هدف:

- createTask
- updateTask
- deleteTask

را با GraphQL یاد بگیریم.

و مقایسه کنیم با:

```http
POST /api/tasks/
PATCH /api/tasks/1/
DELETE /api/tasks/1/
```

---

## Milestone 4: اتصال GraphQL به Service و ORM

هدف:

- GraphQL را به FastAPI service layer وصل کنیم.
- Resolverها را از service جدا نکنیم.
- معماری درست را تمرین کنیم.

---

## Milestone 5: Relationshipها

هدف:

- Task.owner
- User.tasks
- nested queryها

مثلاً:

```graphql
query {
  user(id: 1) {
    username
    tasks {
      id
      title
    }
  }
}
```

---

## Milestone 6: Authentication با JWT

هدف:

- JWT چیست؟
- token چگونه به GraphQL request اضافه می‌شود؟
- context چیست؟
- resolver چگونه کاربر فعلی را می‌شناسد؟

مثلاً:

```graphql
query {
  me {
    id
    username
    email
  }
}
```

---

## Milestone 7: Authorization و Permission

هدف:

- user معمولی فقط taskهای خودش را ببیند.
- admin همه taskها را ببیند.
- permission در REST و GraphQL مقایسه شود.

---

## Milestone 8: Filtering و Pagination

هدف:

- فیلتر کردن taskها
- pagination ساده با limit/offset
- بعد cursor-based pagination در صورت نیاز

---

## Milestone 9: Validation و Error Handling

هدف:

- چگونه input نامعتبر را مدیریت کنیم؟
- خطاها در REST چگونه بودند؟
- خطاها در GraphQL چگونه برگردانده می‌شوند؟
- تفاوت HTTP status code با GraphQL errors.

---

## Milestone 10: Caching و Security

هدف:

- REST caching
- GraphQL caching
- query depth
- query complexity
- introspection
- abuse prevention
- rate limiting
- جلوگیری از queryهای سنگین

---

## Milestone 11: آماده‌سازی برای GitHub

هدف:

- README
- تست‌ها
- مثال‌های GraphQL
- ساختار تمیز
- commit history حرفه‌ای

---

# 11. شروع Milestone 1

الان وارد اولین milestone می‌شویم.

---

## هدف Milestone 1

باید بتوانی به این سؤال‌ها پاسخ بدهی:

1. GraphQL چه چیزی از REST را متفاوت می‌کند؟
2. کلاینت در GraphQL چه چیزی را تعیین می‌کند؟
3. سرور در GraphQL چه چیزی را تعیین می‌کند؟
4. Query برای چیست؟
5. Field چیست؟
6. Argument چیست؟
7. چرا GraphQL برای داده‌های مرتبط مفید است؟

---

## مثال تحلیلی

فرض کن یک صفحه‌ی لیست task داریم.

کلاینت فقط این‌ها را لازم دارد:

```text
task.id
task.title
task.status
```

---

### در REST

ممکن است endpoint این باشد:

```http
GET /api/tasks/
```

اما سرور ممکن است فیلدهای اضافی هم برگرداند:

```json
[
  {
    "id": 1,
    "title": "Learn GraphQL",
    "description": "...",
    "status": "pending",
    "created_at": "...",
    "updated_at": "...",
    "owner_id": 2
  }
]
```

این یعنی over-fetching.

---

### در GraphQL

کلاینت دقیقاً می‌گوید چه می‌خواهد:

```graphql
query {
  tasks {
    id
    title
    status
  }
}
```

و سرور فقط همان فیلدها را برمی‌گرداند.

---

## مثال دوم: Task با Owner

کلاینت برای نمایش یک task و صاحب آن، فقط این‌ها را می‌خواهد:

```text
task.id
task.title
task.status
owner.username
owner.email
```

در GraphQL:

```graphql
query {
  task(id: 1) {
    id
    title
    status
    owner {
      username
      email
    }
  }
}
```

این query یک درخواست دارد، اما داده‌ی nested هم می‌گیرد.

در REST ممکن بود حداقل دو endpoint لازم شود:

```text
GET /api/tasks/1/
GET /api/users/2/
```

یا endpoint اختصاصی دیگر.

---

# 12. اشتباه‌های رایج در شروع GraphQL

---

## اشتباه اول: فکر کنیم GraphQL یک زبان query برای دیتابیس است

نه.

GraphQL برای API است، نه مستقیماً برای دیتابیس.

ما باز هم از ORM استفاده می‌کنیم:

```text
GraphQL -> Resolver -> Service -> ORM -> Database
```

---

## اشتباه دوم: فکر کنیم GraphQL جای REST را همیشه می‌گیرد

نه.

گاهی REST ساده‌تر است.  
گاهی GraphQL بهتر است.  
گاهی هر دو کنار هم منطقی هستند.

---

## اشتباه سوم: فکر کنیم GraphQL خودش authentication و authorization دارد

نه.

GraphQL فقط query language است.

authentication و authorization را خودمان پیاده می‌کنیم.

---

## اشتباه چهارم: هر field را مستقیم به دیتابیس وصل کنیم

این باعث مشکل N+1 می‌شود.

مثلاً:

```graphql
query {
  tasks {
    id
    title
    owner {
      username
    }
  }
}
```

اگر برای هر task یک query جدا برای owner اجرا شود، عملکرد ضعیف می‌شود.

این موضوع را بعداً با DataLoader و بهینه‌سازی بررسی می‌کنیم.

---

# 13. تمرین Milestone 1

این تمرین کد پروژه ندارد. فقط می‌خواهم مفهوم را بنویسی.

---

## صورت تمرین

فرض کن یک کلاینت می‌خواهد جزئیات یک task را نمایش دهد.

برای این صفحه فقط این داده‌ها لازم است:

```text
task.id
task.title
task.status
task.owner.username
```

### سؤال 1

در REST، اگر طراحی سنتی داشته باشیم، ممکن است چه endpoint یا endpointهایی لازم شود؟

---

### سؤال 2

در GraphQL، یک query بنویس که task با id برابر 5 را بگیرد و فقط فیلدهای بالا را درخواست کند.

اگر syntax دقیق را بلد نبودی مشکلی نیست.  
منظورم این است که شکل درخواست را حدست بزنی.

می‌توانی شبیه این بنویسی:

```graphql
query {
  task(...) {
    ...
  }
}
```

---

### سؤال 3

به نظر خودت، این GraphQL query چه مشکلی را نسبت به REST حل می‌کند؟

در یک یا دو جمله بنویس.

---

# 14. Git Commit

چون این مرحله کاملاً مفهومی است، commit کد ندارد.

اگر بخواهی همین یادداشت‌ها یا roadmap را در GitHub نگه داری، می‌توانی یک commit مستندسازی بزنی:

```text
docs: add GraphQL learning roadmap
```

اما commit اصلی بعدی، بعد از اولین پیاده‌سازی GraphQL خواهد بود، احتمالاً چیزی شبیه:

```text
feat(graphql): add initial task query with static data
```

---
