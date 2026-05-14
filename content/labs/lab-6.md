## Тема, Мета, Місце розташування

Тема: «Документування API за допомогою Swagger. Деплой Node.js-додатку. Підсумковий проєкт: REST API з MySQL».

Мета: підготувати фінальну версію backend-застосунку **Puppy Haven**, додавши Swagger/OpenAPI документацію, Swagger UI для тестування endpoint-ів та конфігурацію для деплою на Vercel.

Місце розташування:
- [GitHub](https://github.com/GlebTiK/IA-34_appWEB-HlibSukhoruchkin-FIOT-2026/tree/lab6)
- [Live demo](https://labs-demo-6.771707.xyz/)

---

## Постановка задачі

У лабораторній роботі №6 потрібно було:

- підключити Swagger/OpenAPI до Express-застосунку;
- описати основні REST API endpoint-и;
- створити Swagger UI;
- додати OpenAPI JSON endpoint;
- протестувати API через Swagger UI;
- підготувати застосунок до деплою;
- продемонструвати фінальний REST API з MySQL.

---

## Що повторно використано

У Lab 6 повторно використано:

- frontend Puppy Haven з Lab 3;
- REST API з попередніх лабораторних робіт;
- підключення до MySQL через Sequelize;
- моделі `Puppy`, `VisitRequest`, `User`, `RefreshToken`;
- JWT-автентифікацію;
- логування, моніторинг і захист API;
- кешування та оптимізовані маршрути з Lab 5.

---

## Нові можливості Lab 6

До проєкту додано:

- `swagger-ui-express`;
- `swagger-jsdoc`;
- endpoint `/api-docs`;
- endpoint `/openapi.json`;
- опис моделей даних;
- опис основних API-маршрутів;
- production-ready налаштування для Vercel;
- змінну `PUBLIC_URL` для коректного server URL у Swagger.

---

## Структура маршрутів

### Static frontend

```text
/              -> public/index.html
/about.html    -> public/about.html
/css/*         -> public/css/*
/js/*          -> public/js/*
/assets/*      -> public/assets/*
```

### Backend API

```text
GET  /api/status
GET  /api/health
GET  /api/puppies
POST /api/puppies
GET  /api/visit_requests
POST /api/visit_requests
POST /api/auth/register
POST /api/auth/login
POST /api/auth/refresh
POST /api/auth/logout
GET  /api/auth/me
```

### Swagger

```text
GET /api-docs
GET /openapi.json
```

---

## Swagger/OpenAPI

Swagger UI дозволяє переглядати і тестувати API без Postman.

Для відкриття документації:

```text
https://your-vercel-app.vercel.app/api-docs
```

OpenAPI JSON:

```text
https://your-vercel-app.vercel.app/openapi.json
```

У документації описано:

- HTTP-методи;
- endpoint-и;
- параметри;
- request body;
- JSON-відповіді;
- HTTP status codes;
- схеми моделей.

---

## Змінні середовища

Мінімальний набір:

```env
MYSQL_URL=mysql://user:password@host:port/database
DB_SSL=true
JWT_ACCESS_SECRET=your_access_secret
JWT_REFRESH_SECRET=your_refresh_secret
SEQUELIZE_SYNC=false
```

Додатково для Lab 6:

```env
PUBLIC_URL=https://your-vercel-app.vercel.app
```

Опціонально:

```env
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=your_admin_password
ADMIN_FULL_NAME=Administrator
LOG_LEVEL=info
```

---

## Деплой на Vercel

Для Vercel було додано:

- `api/index.js`;
- `vercel.json`;
- static frontend з папки `public`;
- backend API через `/api/*`;
- окремі rewrites для `/api-docs` і `/openapi.json`.

Схема маршрутизації:

```text
/              -> static frontend
/about.html    -> static page
/api/*         -> serverless backend
/api-docs      -> Swagger UI
/openapi.json  -> OpenAPI specification
```

---

## Приклади перевірки

### Перевірка frontend

```text
https://your-vercel-app.vercel.app/
```

### Перевірка API

```bash
curl https://your-vercel-app.vercel.app/api/health
```

```bash
curl https://your-vercel-app.vercel.app/api/puppies
```

### Перевірка Swagger

```text
https://your-vercel-app.vercel.app/api-docs
```

---

## Результат виконання

У результаті виконання лабораторної роботи №6 проєкт Puppy Haven отримав Swagger/OpenAPI документацію, можливість тестування API через браузерний Swagger UI та конфігурацію для деплою на Vercel. Застосунок зберігає static frontend з попередніх робіт і надає backend API через маршрути `/api/*`.

---

## Висновки

Під час виконання Lab 6 було вивчено документування REST API за допомогою Swagger/OpenAPI. Swagger UI спрощує перевірку endpoint-ів і робить API зрозумілим для користувачів та розробників. Також було закріплено навички підготовки Node.js + Express застосунку до деплою у serverless-середовищі.
