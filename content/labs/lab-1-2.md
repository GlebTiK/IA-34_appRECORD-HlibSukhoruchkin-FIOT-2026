## Тема, Мета, Місце розташування

**Тема:** «Веб-орієнтований застосунок каталогу цуценят "Puppy Haven".»

**Мета:** прискорити вибір цуценя для користувача за рахунок зручного веб-каталогу з фото та короткими характеристиками і простої форми заявки на візит, щоб швидко переглянути варіанти та одразу залишити запит на зустріч/бронювання.  
Додатково (Частина 2): створити та запустити HTTP-сервер на Node.js з використанням Express.js і реалізувати просте REST API з маршрутами **GET/POST/PUT/DELETE** для ресурсу `/students`.

**Місце розташування:**

- [GitHub](https://github.com/GlebTiK/IA-34_appWEB-HlibSukhoruchkin-FIOT-2026/)
- [Live demo](https://labs-demo.771707.xyz/)

---

## Опис предметного середовища

**Предметна галузь:** веб-каталог цуценят "Puppy Haven". Сайт надає перелік цуценят (у вигляді таблиці), коротку інформацію про проєкт/команду, контакти та форму для подачі заявки на візит.

---

## Структура веб-застосунку

- **Головна сторінка** — шапка з навігацією, hero-блок «цуценя тижня», блок категорій, прев’ю «Про нас», каталог (таблиця), форма заявки, секція «Відгуки» (заготовка), футер з контактами.
- **Про нас** — сторінка з описом проєкту, бізнес-логікою та переліком вимог, оформлена в стилі головної сторінки (шапка + футер).

### Додано у Частині 2 (backend)

- **Node.js + Express сервер** (`server.js`) — запускає HTTP-сервер.
- **REST API**:
  - `GET /` — повертає текст `Hello from Node.js server` (перевірка роботи сервера).
  - `GET /students` — повертає список студентів у форматі JSON.
  - `POST /students` — додає нового студента (поля: `id`, `name`, `group`).
  - `PUT /students/:id` — оновлює дані студента.
  - `DELETE /students/:id` — видаляє студента.
- **Статичний сайт Puppy Haven** віддається сервером як статичні файли за шляхом:
  - `GET /app/` → `public/index.html` та інші ресурси (css/js/assets).

---

## Сценарій взаємодії (бізнес-логіка)

1. Користувач відкриває сайт і переходить до розділу **Каталог** (через меню або прокрутку).
2. Переглядає таблицю з цуценятами (фото, ім’я, опис, вік, внесок/ціна).
3. Переходить до розділу **Заявка на візит** і обирає цуценя зі списку у формі.
4. Заповнює форму (ім’я, телефон, дата/час, коментар) і надсилає заявку.
5. Отримує клієнтське підтвердження відправки (на сторінці).
6. Адміністратор зв’язується для уточнення деталей (поза межами сайту, за контактним номером/поштою).

---

## Вимоги

### Функціональні вимоги

- Головна навігація по розділах сторінки та перехід на сторінку «Про нас».
- Адаптивна навігація (бургер-меню на мобільних).
- Відображення каталогу у вигляді таблиці (рядки заповнюються через JavaScript).
- Форма заявки на візит з обов’язковими полями та відправкою (клієнтське повідомлення-підтвердження).
- Блок контактів у футері.

### Нефункціональні вимоги

- Інтуїтивний інтерфейс і зрозуміла навігація.
- Коректна робота форми (перевірка обов’язкових полів у браузері).
- Підтримка сучасних браузерів (Chrome, Firefox, Edge, Safari).
- Оптимізовані зображення (щоб сторінки завантажувались швидко).
- Адаптивність для різних розмірів екранів.
- Головна сторінка має завантажуватися менше ніж за 2 секунди за нормальних умов мережі та на сучасних браузерах

---

## Стек технологій

- **HTML5** — семантична розмітка сторінок і секцій.
- **CSS3** — стилі та адаптивність (flex/grid, media queries).
- **JavaScript (Vanilla)** — бургер-меню, заповнення каталогу даними, обробка відправки форми.
- **Git + GitHub** — контроль версій, зберігання коду.
- **Статичний хостинг** — публікація сайту як статичних сторінок.
- **Node.js** — середовище виконання JavaScript на стороні сервера (Частина 2).
- **Express.js** — фреймворк для створення HTTP-сервера та REST API (Частина 2).

---

## Діаграми

---

### UML-case діаграма

![UML-case діаграма](/assets/labs/lab-1/uml.png)

---

### ER-діаграма

![ER-діаграма](/assets/labs/lab-1/er.png)

---

## Структура документа

Нижче наведено узагальнену структуру HTML для двох сторінок проєкту — **index.html** (Головна) та **about.html** (Про нас). Це спрощені DOM-скелети, які демонструють основні блоки: `header` (логотип + навігація), `main` (секції), `footer` (контакти).

> Примітка: у backend-версії ці файли знаходяться в папці `public/` і доступні через сервер за адресою `http://localhost:3000/app/`.  
> Тому всі посилання та ресурси на сторінках використовують префікс `/app/` (наприклад `/app/css/style.css`).

### 1) Структура сторінки `index.html` (Головна)

```html
<!DOCTYPE html>
<html lang="uk">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Puppy Haven - Головна</title>
    <link rel="stylesheet" href="/app/css/style.css" />
  </head>
  <body>
    <header class="site-header">
      <div class="logo">
        <img src="/app/assets/images/logo.png" alt="Puppy Haven logo" />
      </div>

      <button class="burger" aria-label="Відкрити меню" aria-expanded="false">
        <span></span><span></span><span></span>
      </button>

      <nav class="nav" aria-label="Головна навігація">
        <ul>
          <li><a href="/app/index.html#catalog">Каталог</a></li>
          <li><a href="/app/about.html">Про нас</a></li>
          <li><a href="/app/index.html#pick">Підібрати цуценя</a></li>
          <li><a href="/app/index.html#contacts">Контакти</a></li>
        </ul>
      </nav>
    </header>

    <main>
      <section id="hero" class="hero">
        <div class="hero-text">
          <h2>Цуценя тижня: "Луна"</h2>
          <p>Дружня, грайлива та дуже любить людей. Ідеальна для сім'ї.</p>
          <a class="btn" href="/app/index.html#catalog">Перейти в каталог</a>
        </div>
        <div class="hero-image">
          <img src="/app/assets/images/puppy-hero.png" alt="Фото цуценяти Луна" />
        </div>
      </section>

      <section id="categories" class="categories">
        <div class="category">
          <img src="/app/assets/images/small.png" alt="Малі породи" />
          <p>Малі породи</p>
        </div>
        <div class="category">
          <img src="/app/assets/images/medium.png" alt="Середні породи" />
          <p>Середні породи</p>
        </div>
        <div class="category">
          <img src="/app/assets/images/big.png" alt="Великі породи" />
          <p>Великі породи</p>
        </div>
      </section>

      <section id="about-preview" class="about-preview">
        <h2>Про нас</h2>
        <div class="about-container">
          <div class="about-text">
            <p>
              Puppy Haven - це сервіс, де можна безпечно обрати цуценя,
              дізнатись про умови догляду та залишити заявку на візит. Ми дбаємо
              про соціалізацію та здоров'я малюків.
            </p>
          </div>
          <div class="about-image">
            <img src="/app/assets/images/team.png" alt="Команда Puppy Haven" />
          </div>
        </div>
      </section>

      <section id="catalog" class="catalog">
        <h2>Каталог</h2>
        <table class="catalog-table">
          <thead>
            <tr>
              <th>Фото</th>
              <th>Ім'я</th>
              <th>Опис</th>
              <th>Вік</th>
              <th>Внесок/ціна</th>
            </tr>
          </thead>
          <tbody>
          </tbody>
        </table>
      </section>

      <section id="pick" class="pick">
        <h2>Заявка на візит</h2>
        <form class="request-form">
          <label
            >Ваше ім'я
            <input type="text" name="name" required />
          </label>

          <label
            >Телефон
            <input type="tel" name="phone" required />
          </label>

          <label
            >Оберіть цуценя
            <select name="puppy" required>
              <option value="">- оберіть -</option>
              <option value="luna">Луна</option>
              <option value="max">Макс</option>
            </select>
          </label>

          <label
            >Дата/час
            <input type="datetime-local" name="datetime" required />
          </label>

          <label
            >Коментар
            <textarea name="note" rows="4"></textarea>
          </label>

          <button type="submit">Надіслати заявку</button>
        </form>
      </section>

      <section id="feedback" class="feedback">
        <h2>Відгуки</h2>
      </section>
    </main>

    <footer id="contacts" class="site-footer">
      <div class="contacts">
        <p><strong>Контакти:</strong> +38 (0XX) XXX-XX-XX, info@puppyhaven</p>
        <p><strong>Графік:</strong> Пн–Сб 10:00–19:00</p>
        <p><strong>Адреса:</strong> м. Київ, вул. Прикладна, 1</p>
      </div>
      <p>© 2026 Puppy Haven</p>
    </footer>

    <script src="/app/js/main.js"></script>
  </body>
</html>
```

### 2) Структура сторінки `about.html` (Про нас)

```html
<!doctype html>
<html lang="uk">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Про нас — Puppy Haven</title>
    <link rel="stylesheet" href="/app/css/style.css" />
  </head>
  <body>
    <header class="site-header">
      <div class="logo">
        <img src="/app/assets/images/logo.png" alt="Puppy Haven logo" />
      </div>

      <button class="burger" aria-label="Відкрити меню" aria-expanded="false">
        <span></span><span></span><span></span>
      </button>

      <nav class="nav" aria-label="Головна навігація">
        <ul>
          <li><a href="/app/index.html#catalog">Каталог</a></li>
          <li><a href="/app/about.html">Про нас</a></li>
          <li><a href="/app/index.html#pick">Підібрати цуценя</a></li>
          <li><a href="/app/about.html#contacts">Контакти</a></li>
        </ul>
      </nav>
    </header>

    <main>
      <section id="hero" class="hero">
        <div class="hero-text">
          <h2>Про Puppy Haven</h2>
          <p>
            Puppy Haven — це веб-каталог цуценят, де можна переглянути фото та
            опис, дізнатися ключові параметри і залишити заявку на візит.
          </p>
          <a class="btn" href="/app/index.html#catalog">Перейти в каталог</a>
        </div>
        <div class="hero-image">
          <img src="/app/assets/images/team.png" alt="Команда Puppy Haven" />
        </div>
      </section>

      <section id="domain">
        <h2>Предметна область</h2>
        <p>
          Сайт орієнтований на зручний перегляд списку цуценят та швидке
          оформлення заявки. Кожна позиція в каталозі містить фото, короткий
          опис і базові характеристики.
        </p>
      </section>

      <section id="logic">
        <h2>Бізнес-логіка (сценарій)</h2>
        <ol>
          <li>Користувач відкриває сайт і переходить у каталог.</li>
          <li>Переглядає цуценят та обирає потрібного.</li>
          <li>Заповнює форму заявки на візит.</li>
          <li>Адміністратор підтверджує заявку та узгоджує деталі.</li>
        </ol>
      </section>

      <section id="requirements">
        <h2>Вимоги</h2>

        <h3>Функціональні</h3>
        <ul>
          <li>Каталог цуценят з фото та описом.</li>
          <li>Форма заявки на візит.</li>
          <li>Доступні розділи «Про нас» і «Контакти» з навігації.</li>
          <li>Адаптивність для ПК і мобільних пристроїв.</li>
        </ul>

        <h3>Нефункціональні</h3>
        <ul>
          <li>Зрозуміла структура та проста навігація.</li>
          <li>Коректна робота форм (перевірка обов’язкових полів).</li>
          <li>Підтримка сучасних браузерів.</li>
          <li>Оптимізовані зображення для швидкого завантаження.</li>
        </ul>
      </section>
    </main>

    <footer id="contacts" class="site-footer">
      <div class="contacts">
        <p><strong>Контакти:</strong> +38 (0XX) XXX-XX-XX, info@puppyhaven</p>
        <p><strong>Графік:</strong> Пн–Сб 10:00–19:00</p>
        <p><strong>Адреса:</strong> м. Київ, вул. Прикладна, 1</p>
      </div>
      <p>© 2026 Puppy Haven</p>
    </footer>
  </body>
</html>
```

---

## Скріншоти

![Скрін index.html](/assets/labs/lab-1/screen-index.png)  
![Скрін about.html](/assets/labs/lab-1/screen-about.png)  
- ![Скрін відповіді API /students](/assets/labs/lab-1-2/screen-students-api.png)

Додаткові (за потреби):
- ![Скрін запуску Node.js сервера](/assets/labs/lab-1-2/screen-server.png)

---

## Як запустити backend (Частина 2)

1. Перейти в папку з проєктом та встановити залежності:

```bash
npm install
```

2. Запустити сервер:

```bash
node server.js
```

(або `npm start`)

3. Перевірити в браузері/через Postman:

- `http://localhost:3000/` → `Hello from Node.js server`
- `http://localhost:3000/students` → JSON список студентів
- `http://localhost:3000/app/` → статичний сайт Puppy Haven

---

## Висновки

У ході виконання роботи було сформовано структуру веб-застосунку "Puppy Haven", описано предметну область і бізнес-логіку, визначено функціональні та нефункціональні вимоги, а також наведено приклади семантичної HTML-розмітки (таблиця каталогу, форма заявки, навігація та футер з контактами). Було створено Node.js проєкт з Express.js, реалізовано HTTP-сервер і REST API з маршрутами `/students` (GET/POST/PUT/DELETE) та організовано віддачу статичних сторінок Puppy Haven через Express.
