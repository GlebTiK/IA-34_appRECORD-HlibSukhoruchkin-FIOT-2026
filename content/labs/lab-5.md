## Тема, Мета, Місце розташування

Тема: «Безпека та продуктивність серверних додатків. Безпека Node.js-додатків. Оптимізація запитів і кешування. Тестування API».

Мета: розширити backend-застосунок **Puppy Haven** засобами базової безпеки, кешування, оптимізації API, автоматизованого тестування та перевірки продуктивності.

Місце розташування:
- [GitHub](https://github.com/GlebTiK/IA-34_appWEB-HlibSukhoruchkin-FIOT-2026/tree/lab5)
- [Live demo](https://labs-demo-5.771707.xyz/)

---

## Постановка задачі

У лабораторній роботі №5 потрібно було реалізувати:

- захист HTTP-заголовків через Helmet;
- rate limiting для обмеження кількості запитів;
- валідацію вхідних даних;
- кешування відповідей API;
- оптимізацію одного з маршрутів;
- автоматизоване тестування API;
- навантажувальне тестування;
- аналіз продуктивності до та після кешування;
- адаптацію застосунку до Vercel.

---

## Що повторно використано з попередніх лабораторних робіт

У Lab 5 повторно використано:

- static frontend Puppy Haven;
- маршрути каталогу цуценят;
- форму заявки на візит;
- підключення до MySQL;
- Sequelize-моделі;
- JWT-автентифікацію;
- логування і моніторинг з Lab 4.

---

## Нові можливості Lab 5

До проєкту додано:

- `helmet` для захисту HTTP-заголовків;
- загальний rate limiter;
- параметри `CACHE_TTL_SECONDS` і `RATE_LIMIT_MAX`;
- кешування маршруту `/api/puppies`;
- підтримку Redis як опціонального кешу;
- fallback на in-memory cache, якщо Redis не налаштовано;
- пагінацію для списку цуценят;
- Jest/Supertest тести;
- Artillery сценарій для навантажувального тестування.

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
GET  /api/puppies?page=1&limit=5
POST /api/puppies
GET  /api/visit_requests
POST /api/visit_requests
POST /api/auth/register
POST /api/auth/login
POST /api/auth/refresh
POST /api/auth/logout
```

---

## Кешування

Для маршруту каталогу реалізовано кешування відповіді.

Перший запит може повертати дані з бази:

```json
{
  "source": "database",
  "data": []
}
```

Повторний запит протягом часу кешування повертає дані з кешу:

```json
{
  "source": "cache",
  "data": []
}
```

Якщо `REDIS_URL` не заданий, застосунок використовує in-memory cache.

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

Додаткові змінні Lab 5:

```env
CACHE_TTL_SECONDS=60
RATE_LIMIT_MAX=120
REDIS_URL=
CORS_ORIGIN=
```

Опціонально:

```env
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=your_admin_password
ADMIN_FULL_NAME=Administrator
LOG_LEVEL=info
```

---

## Тестування

### Автоматизовані тести

```bash
npm test
```

Тести перевіряють працездатність основних API-маршрутів.

### Навантажувальне тестування

```bash
npm run artillery
```

Або приклад команди:

```bash
artillery quick --count 50 --num 20 http://localhost:3000/api/puppies
```

---

## Особливості запуску на Vercel

Для Vercel використано:

- `api/index.js`;
- `vercel.json`;
- routing тільки для `/api/*`;
- static frontend напряму з папки `public`;
- console logging замість запису у файлову систему;
- in-memory cache як стандартний варіант без Redis.

---

## Приклади перевірки

```bash
curl https://your-vercel-app.vercel.app/api/puppies
```

```bash
curl "https://your-vercel-app.vercel.app/api/puppies?page=1&limit=5"
```

```bash
curl https://your-vercel-app.vercel.app/api/status
```

---

## Результат виконання

У результаті виконання лабораторної роботи №5 backend-застосунок було доповнено засобами безпеки, кешування та тестування. Було реалізовано захист HTTP-заголовків, обмеження кількості запитів, кешування відповідей API, пагінацію та автоматизовані тести. Проєкт також був підготовлений до запуску на Vercel.

---

## Висновки

Під час виконання Lab 5 було закріплено практичні навички захисту Node.js API, оптимізації REST endpoint-ів і перевірки продуктивності. Кешування дозволяє зменшити кількість звернень до бази даних і пришвидшити повторні запити, а автоматизовані тести допомагають перевірити стабільність backend-логіки.
