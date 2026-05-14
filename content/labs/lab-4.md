## Тема, Мета, Місце розташування

Тема: «Розширені можливості Node.js-додатків: логування, завантаження файлів, моніторинг продуктивності».

Мета: розширити backend-застосунок **Puppy Haven**, створений у попередніх лабораторних роботах, додавши логування HTTP-запитів, файлове/консольне логування подій, завантаження файлів на сервер, валідацію файлів та endpoint для моніторингу стану серверного застосунку.

Місце розташування:
- [GitHub](https://github.com/GlebTiK/IA-34_appWEB-HlibSukhoruchkin-FIOT-2026/tree/lab4)
- [Live demo](https://labs-demo-4.771707.xyz/)

---

## Постановка задачі

У лабораторній роботі №4 потрібно було доповнити Node.js + Express застосунок такими можливостями:

- логування HTTP-запитів через Morgan;
- логування подій та помилок через Winston;
- централізована обробка помилок;
- завантаження одного файлу;
- завантаження кількох файлів;
- перевірка типу та розміру файлу;
- endpoint для перевірки стану сервера;
- вимірювання часу обробки запитів;
- адаптація застосунку до запуску на Vercel.

---

## Що повторно використано з Lab 3

У роботі повторно використано:

- статичний frontend з папки `public`;
- головну сторінку `public/index.html`;
- сторінку `public/about.html`;
- CSS та JS файли frontend;
- маршрути `/api/puppies` та `/api/visit_requests`;
- підключення до MySQL через Sequelize;
- наявні моделі `Puppy`, `VisitRequest`, `User`, `RefreshToken`;
- JWT-автентифікацію з попередньої лабораторної роботи.

Таким чином Lab 4 не створює новий проєкт з нуля, а продовжує розвиток Lab 3.

---

## Нові можливості Lab 4

До проєкту було додано:

- `utils/logger.js` для централізованого логування;
- middleware для логування HTTP-запитів;
- middleware для вимірювання часу відповіді;
- endpoint `/api/status`;
- маршрути для завантаження файлів;
- перевірку формату файлів;
- обмеження розміру файлу;
- Vercel-compatible routing.

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
GET  /api/visit_requests
POST /api/visit_requests
POST /api/files/upload
POST /api/files/upload-multiple
```

---

## Особливості запуску на Vercel

Для Vercel було додано:

- `api/index.js` — serverless entrypoint;
- `vercel.json` — правила маршрутизації;
- розділення static frontend і backend API;
- безпечне логування у console замість запису у `/var/task/logs`;
- використання `/tmp` для тимчасових файлів під час upload.

На Vercel не можна створювати папки у `/var/task`, тому файлові логи були замінені на консольні логи.

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

Опціонально:

```env
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=your_admin_password
ADMIN_FULL_NAME=Administrator
LOG_LEVEL=info
```

Адмін-змінні потрібні лише для автоматичного створення адміністратора.

---

## Приклади перевірки

### Перевірка frontend

```text
/
```

Очікуваний результат: відкривається головна сторінка Puppy Haven.

```text
/about.html
```

Очікуваний результат: відкривається сторінка «Про нас».

### Перевірка backend

```bash
curl https://your-vercel-app.vercel.app/api/status
```

Очікуваний результат:

```json
{
  "ok": true,
  "uptime": 123,
  "memory": {}
}
```

### Завантаження одного файлу

У Postman:

```text
POST /api/files/upload
Body -> form-data
key: file
type: File
```

---

## Результат виконання

У результаті виконання лабораторної роботи №4 застосунок Puppy Haven було розширено можливостями логування, моніторингу та завантаження файлів. Backend отримав endpoint для перегляду стану сервера, middleware для вимірювання часу відповіді, а також маршрути для завантаження одного або кількох файлів. Проєкт було адаптовано до запуску на Vercel з коректним розділенням static frontend і backend API.

---

## Висновки

Під час виконання Lab 4 було вивчено практичне застосування Morgan, Winston і Multer у Node.js + Express застосунку. Також було розглянуто особливості запуску backend-застосунку в serverless-середовищі Vercel, де не можна покладатися на постійний файловий запис у директорії проєкту.
