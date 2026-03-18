## Тема, мета, місце розташування

**Тема:** «Створення бази даних у MySQL. Підключення Node.js до MySQL. Робота з ORM Sequelize».

**Мета:**
- навчитися створювати базу даних у MySQL;
- освоїти виконання SQL-запитів `SELECT`, `INSERT`, `UPDATE`, `DELETE`;
- підключити серверну програму на Node.js до бази даних;
- використати ORM Sequelize для роботи з БД;
- реалізувати зв’язок One-to-Many між таблицями.

**Місце розташування:**
- [GitHub](https://github.com/GlebTiK/IA-34_appRECORD-HlibSukhoruchkin-FIOT-2026)
- [URL для Vercel-деплою після публікації](https://your-project.vercel.app)

---

## Опис предметної області

У межах лабораторної роботи реалізовано серверний застосунок на Node.js для роботи з двома сутностями: `users` і `posts`.  
Кожен користувач може мати декілька постів, тому між таблицями реалізовано зв’язок **One-to-Many**: один `User` → багато `Post`.

Застосунок демонструє два способи роботи з даними:
- через **mysql2** і виконання SQL-запитів безпосередньо;
- через **Sequelize ORM**, де таблиці представлені у вигляді моделей JavaScript.

---

## Короткі теоретичні відомості

### MySQL

MySQL — це реляційна система керування базами даних, яка використовує мову SQL для зберігання, вибірки, оновлення та видалення даних.

### Node.js

Node.js — це середовище виконання JavaScript, яке дозволяє створювати серверні застосунки та API.

### mysql2

`mysql2` — це драйвер для Node.js, який дозволяє підключатися до MySQL і виконувати SQL-запити з коду.

### Sequelize

Sequelize — це ORM-бібліотека для Node.js, яка дозволяє працювати з реляційною базою даних через JavaScript-об’єкти, а не лише через ручне написання SQL-запитів.

### Зв’язок One-to-Many

Зв’язок One-to-Many означає, що одному запису з таблиці `users` може відповідати багато записів із таблиці `posts`.  
У Sequelize це реалізується за допомогою методів:
- `User.hasMany(Post)`
- `Post.belongsTo(User)`

---

## Постановка завдання

Під час виконання лабораторної роботи необхідно:
1. створити базу даних `web_backend_lab`;
2. створити таблиці `users` та `posts`;
3. виконати SQL-запити `SELECT`, `INSERT`, `UPDATE`, `DELETE`;
4. підключити Node.js до MySQL через пакет `mysql2`;
5. реалізувати роботу з базою через Sequelize;
6. створити моделі `User` та `Post`;
7. реалізувати зв’язок One-to-Many;
8. підготувати серверний застосунок, який може працювати локально і бути розгорнутим на Vercel.

---

## Структура проєкту

```text
lab2-vercel/
├── config/
│   └── database.js
├── db/
│   └── raw.js
├── lib/
│   └── bootstrap.js
├── models/
│   ├── index.js
│   ├── Post.js
│   └── User.js
├── sql/
│   └── schema.sql
├── .env.example
├── package.json
├── README.md
├── server.js
└── vercel.json
```

---

## Хід виконання роботи

### 1. Створення проєкту та встановлення пакетів

Для виконання лабораторної роботи було створено окремий Node.js-проєкт і встановлено потрібні бібліотеки:

```bash
mkdir lab2-vercel
cd lab2-vercel
npm init -y
npm install express mysql2 sequelize
```

### 2. Підготовка змінних середовища

Для локального запуску та для деплою на Vercel параметри підключення до БД винесено у змінні середовища.

```env
PORT=3000
MYSQL_URL=
DB_HOST=localhost
DB_PORT=3306
DB_NAME=web_backend_lab
DB_USER=root
DB_PASSWORD=password
DB_SSL=false
```

Такий підхід дозволяє не записувати логін і пароль до MySQL безпосередньо в коді.

### 3. Підключення до MySQL через Sequelize

У файлі `config/database.js` створюється підключення до БД. Підтримуються два режими:
- через рядок підключення `MYSQL_URL`;
- через окремі змінні `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`.

```js
const { Sequelize } = require('sequelize');

const sequelize = process.env.MYSQL_URL
  ? new Sequelize(process.env.MYSQL_URL, {
      dialect: 'mysql',
      logging: false
    })
  : new Sequelize(
      process.env.DB_NAME || 'web_backend_lab',
      process.env.DB_USER || 'root',
      process.env.DB_PASSWORD || 'password',
      {
        host: process.env.DB_HOST || 'localhost',
        port: Number(process.env.DB_PORT || 3306),
        dialect: 'mysql',
        logging: false
      }
    );

module.exports = sequelize;
```

### 4. Підключення до MySQL через mysql2

Для демонстрації прямого виконання SQL-запитів використано пул з’єднань `mysql2/promise`.

```js
const mysql = require('mysql2/promise');

const pool = mysql.createPool({
  host: process.env.DB_HOST || 'localhost',
  port: Number(process.env.DB_PORT || 3306),
  user: process.env.DB_USER || 'root',
  password: process.env.DB_PASSWORD || 'password',
  database: process.env.DB_NAME || 'web_backend_lab',
  waitForConnections: true,
  connectionLimit: 5
});

module.exports = pool;
```

### 5. Створення моделей Sequelize

#### Модель `User`

```js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/database');

const User = sequelize.define(
  'User',
  {
    id: {
      type: DataTypes.INTEGER,
      autoIncrement: true,
      primaryKey: true
    },
    name: {
      type: DataTypes.STRING(100),
      allowNull: false
    },
    email: {
      type: DataTypes.STRING(150),
      allowNull: false,
      unique: true
    }
  },
  {
    tableName: 'users',
    timestamps: true
  }
);

module.exports = User;
```

#### Модель `Post`

```js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/database');

const Post = sequelize.define(
  'Post',
  {
    id: {
      type: DataTypes.INTEGER,
      autoIncrement: true,
      primaryKey: true
    },
    title: {
      type: DataTypes.STRING(150),
      allowNull: false
    },
    content: {
      type: DataTypes.TEXT,
      allowNull: false
    }
  },
  {
    tableName: 'posts',
    timestamps: true
  }
);

module.exports = Post;
```

### 6. Налаштування зв’язку One-to-Many

```js
const User = require('./User');
const Post = require('./Post');

User.hasMany(Post, {
  foreignKey: 'userId',
  as: 'posts',
  onDelete: 'CASCADE'
});

Post.belongsTo(User, {
  foreignKey: 'userId',
  as: 'user'
});
```

Це означає, що:
- один користувач може мати багато постів;
- кожен пост належить одному користувачу;
- у таблиці `posts` створюється зовнішній ключ `userId`.

---

## SQL-скрипт створення таблиць

```sql
CREATE DATABASE IF NOT EXISTS web_backend_lab;
USE web_backend_lab;

CREATE TABLE IF NOT EXISTS users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) NOT NULL UNIQUE,
  createdAt DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updatedAt DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(150) NOT NULL,
  content TEXT NOT NULL,
  userId INT NOT NULL,
  createdAt DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updatedAt DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  CONSTRAINT fk_posts_user
    FOREIGN KEY (userId) REFERENCES users(id)
    ON DELETE CASCADE
);
```

---

## Основний серверний код

У файлі `server.js` реалізовано:
- перевірку підключення до БД;
- автоматичну синхронізацію таблиць через `sequelize.sync()`;
- маршрути для роботи через `mysql2`;
- маршрути для роботи через `Sequelize`;
- експорт Express-застосунку для деплою на Vercel.

### Фрагмент `server.js`

```js
const express = require('express');
const initApp = require('./lib/bootstrap');
const pool = require('./db/raw');
const { User, Post } = require('./models');

const app = express();
app.use(express.json());

app.use(async (req, res, next) => {
  try {
    await initApp();
    next();
  } catch (error) {
    res.status(500).json({ message: 'Database initialization failed' });
  }
});

app.get('/sql/users', async (req, res) => {
  const [rows] = await pool.execute(
    'SELECT id, name, email, createdAt, updatedAt FROM users ORDER BY id ASC'
  );
  res.json(rows);
});

app.post('/orm/posts', async (req, res) => {
  const { title, content, userId } = req.body;
  const post = await Post.create({ title, content, userId });
  res.status(201).json(post);
});

module.exports = app;
```

---

## Опис маршрутів API

### Маршрути для роботи через mysql2

| Метод | Маршрут | Призначення |
|---|---|---|
| `GET` | `/sql/users` | отримати список користувачів |
| `POST` | `/sql/users` | додати нового користувача |
| `PUT` | `/sql/users/:id` | оновити користувача |
| `DELETE` | `/sql/users/:id` | видалити користувача |

### Маршрути для роботи через Sequelize

| Метод | Маршрут | Призначення |
|---|---|---|
| `GET` | `/orm/users` | список користувачів разом з постами |
| `POST` | `/orm/users` | створити користувача |
| `GET` | `/orm/posts` | список постів разом з авторами |
| `POST` | `/orm/posts` | створити пост |
| `GET` | `/orm/users/:id/posts` | отримати одного користувача з його постами |

---

## Приклади SQL-запитів

```sql
SELECT * FROM users;

INSERT INTO users (name, email)
VALUES ('Гліб Сухоручкін', 'hlib@example.com');

UPDATE users
SET name = 'Гліб Сухоручкін IA-34'
WHERE id = 1;

DELETE FROM users
WHERE id = 9999;
```

---

## Приклади тестування через curl

### Перевірка роботи API

```bash
curl http://localhost:3000/health
```

### Додавання користувача через mysql2

```bash
curl -X POST http://localhost:3000/sql/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Гліб Сухоручкін","email":"hlib@example.com"}'
```

### Оновлення користувача через mysql2

```bash
curl -X PUT http://localhost:3000/sql/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Гліб Сухоручкін IA-34"}'
```

### Видалення користувача через mysql2

```bash
curl -X DELETE http://localhost:3000/sql/users/1
```

### Створення користувача через Sequelize

```bash
curl -X POST http://localhost:3000/orm/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Іван Петренко","email":"ivan@example.com"}'
```

### Створення поста через Sequelize

```bash
curl -X POST http://localhost:3000/orm/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"Мій перший пост","content":"Hello Sequelize","userId":1}'
```

### Отримання користувача з постами

```bash
curl http://localhost:3000/orm/users/1/posts
```

---

## Запуск проєкту локально

```bash
npm install
node server.js
```

Після запуску сервер доступний за адресою:

```text
http://localhost:3000
```

---

## Особливості деплою на Vercel

Для розгортання на Vercel підготовлено Express-застосунок, який експортується через:

```js
module.exports = app;
```

Також у проєкті використано файл `vercel.json`:

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "functions": {
    "server.js": {
      "maxDuration": 10
    }
  }
}
```

Для деплою потрібно:
1. завантажити код у GitHub;
2. імпортувати репозиторій у Vercel;
3. додати змінні середовища для підключення до MySQL;
4. виконати повторний деплой після збереження змінних.

---

## Скріншоти

![Скрін головної відповіді API](/assets/labs/lab-2/screen-root.png)

![Скрін списку користувачів через mysql2](/assets/labs/lab-2/screen-sql-users.png)

![Скрін списку користувачів і постів через Sequelize](/assets/labs/lab-2/screen-orm-users.png)

![Скрін налаштування змінних середовища у Vercel](/assets/labs/lab-2/screen-vercel-env.png)

---

## Відповіді на контрольні питання

1. **Що таке реляційна база даних?**  
   Це база даних, у якій інформація зберігається у вигляді таблиць, пов’язаних між собою.

2. **Для чого використовується MySQL?**  
   Для зберігання, пошуку, оновлення та видалення структурованих даних у серверних застосунках.

3. **Що таке ORM?**  
   ORM — це технологія відображення таблиць бази даних у вигляді програмних об’єктів.

4. **Для чого використовується Sequelize?**  
   Для зручної роботи з реляційною БД через JavaScript-моделі замість ручного написання великої кількості SQL-коду.

5. **Яка різниця між `hasMany` та `belongsTo`?**  
   `hasMany` означає, що один запис пов’язаний з багатьма іншими, а `belongsTo` означає, що поточний запис належить одному батьківському запису.

6. **Що таке зв’язок One-to-Many?**  
   Це зв’язок, де одному запису з першої таблиці відповідає багато записів з другої таблиці.

7. **Як виконати SQL-запит з Node.js?**  
   SQL-запит можна виконати через драйвер `mysql2`, наприклад за допомогою методу `execute()`.

---

## Висновки

У ході виконання лабораторної роботи було створено базу даних MySQL, реалізовано таблиці `users` і `posts`, виконано основні SQL-запити та налаштовано підключення до БД із Node.js. Також було використано ORM Sequelize для опису моделей і реалізації зв’язку One-to-Many між користувачами та постами.

У результаті отримано серверний застосунок, який працює локально, підтримує CRUD-операції через `mysql2` і `Sequelize`, а також підготовлений до розгортання на Vercel.
