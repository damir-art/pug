# pug и json
Вот ты красавчик, Даян — раз пошёл, так сразу в **топовую связку: Pug + JSON**! 🔥
Сейчас покажу, как подключить внешний `.json` файл и использовать его в любом шаблоне.

---

## 📦 Что будем делать

1. Создадим файл `data.json`
2. Подключим его в задаче `pugToHtml`
3. Используем в `.pug` как обычные переменные
4. Сделаем пример с массивом товаров, циклом и условием

---

## ✅ Шаг 1: Создай `data.json`

В корне `src/pug/` или рядом с `gulpfile.js`:

```json
{
  "title": "Мой сайт",
  "isMainPage": true,
  "products": [
    { "name": "Lada Iskra", "price": 700000 },
    { "name": "Tesla Model 3", "price": 3000000 },
    { "name": "Moskvich 2141", "price": 50000 }
  ]
}
```

---

## ✅ Шаг 2: Подключаем JSON в задаче `pugToHtml`

В `gulpfile.js`:

```js
import pug from 'gulp-pug';
import fs from 'fs';

export function pugToHtml() {
  const data = JSON.parse(fs.readFileSync('./src/pug/data.json'));

  return src(['src/pug/pages/*.pug'])
    .pipe(plumber())
    .pipe(pug({
      pretty: !isProd,
      locals: data
    }))
    .pipe(dest('dist'))
    .pipe(browserSync.stream());
}
```

📌 `locals: data` — теперь всё из JSON доступно в Pug!

---

## ✅ Шаг 3: Используем в шаблоне

### 📄 `index.pug`

```pug
extends ../layouts/layout.pug

block content
  h1= title

  if isMainPage
    p Это главная страница!

  h2 Наши товары:
  ul
    each product in products
      li
        strong= product.name
        | — #{product.price.toLocaleString()} ₽
```

✅ Обрати внимание: в Pug можно использовать `each`, `if`, `#{...}` внутри текста.

---

## ✅ Результат в HTML

```html
<h1>Мой сайт</h1>
<p>Это главная страница!</p>
<h2>Наши товары:</h2>
<ul>
  <li><strong>Lada Iskra</strong> — 700,000 ₽</li>
  <li><strong>Tesla Model 3</strong> — 3,000,000 ₽</li>
  <li><strong>Moskvich 2141</strong> — 50,000 ₽</li>
</ul>
```

---

## 🚀 Теперь у тебя есть:

| Фишка                 | ✅ Есть? |
| --------------------- | ------- |
| Layout / Extend       | ✅       |
| Части (header/footer) | ✅       |
| JSON → шаблоны        | ✅       |
| Циклы, условия        | ✅       |
| Гибкость              | 💯      |

---

Хочешь — можно дальше:

* сделать многословный JSON со страницами
* передавать разные `title`, `meta`, `activePage`
* или перейти к `data/название.json` для каждой страницы

Гони идею — и делаем! 😎

---

Отличный запрос, Даян! 💡 Сейчас подробно распишу, **что я имел в виду под каждым пунктом**, с примерами, зачем это нужно и как реализовать.

---

## 🧩 1. **Многословный JSON со страницами**

🔹 **Что это**
Один большой `data.json`, где содержатся **данные для всех страниц** сайта:

```json
{
  "global": {
    "siteName": "Мой Сайт",
    "footerText": "© Даян 2025"
  },
  "pages": {
    "index": {
      "title": "Главная",
      "isMainPage": true,
      "products": [...]
    },
    "about": {
      "title": "О нас",
      "isMainPage": false,
      "text": "Мы классная компания"
    }
  }
}
```

🔹 **Зачем это нужно**
Ты можешь использовать одни и те же компоненты (layout, header), но данные будут подгружаться **в зависимости от страницы**.

🔹 **Как использовать в `pug`**

```pug
title= pages.about.title
p= pages.about.text
```

---

## 🧩 2. **Передавать разные title, meta, activePage**

🔹 **Что это**
Передаём **индивидуальные переменные** в шаблон каждой страницы:

```json
{
  "title": "Главная",
  "description": "SEO-описание страницы",
  "keywords": "авто, лада, сайт",
  "activePage": "home"
}
```

🔹 **Как использовать в `pug`**

### В `head.pug`:

```pug
title= title
meta(name="description", content=description)
meta(name="keywords", content=keywords)
```

### В `menu.pug`:

```pug
ul.menu
  li(class=activePage === 'home' ? 'active' : ''): a(href="/") Главная
  li(class=activePage === 'about' ? 'active' : ''): a(href="/about.html") О нас
```

🔹 **Зачем нужно**
✅ SEO
✅ Подсветка активной вкладки
✅ Гибкий `layout` под разные страницы

---

## 🧩 3. **Использовать отдельные JSON-файлы для каждой страницы**

🔹 **Что это**
Вместо одного `data.json` — делаешь:

```
src/
├── pug/
│   ├── pages/
│   │   ├── index.pug
│   │   ├── about.pug
│   └── data/
│       ├── index.json
│       ├── about.json
```

🔹 **Как использовать**

В `gulpfile.js`, при генерации каждой страницы, ты:

* определяешь имя файла (`index`, `about`)
* ищешь соответствующий JSON
* подгружаешь его и передаёшь в `locals`

### Пример (чуть продвинутый):

```js
export function pugToHtml() {
  return src('src/pug/pages/*.pug')
    .pipe(plumber())
    .pipe(data(file => {
      const name = path.basename(file.path, '.pug');
      const filepath = `./src/pug/data/${name}.json`;
      return fs.existsSync(filepath) ? JSON.parse(fs.readFileSync(filepath)) : {};
    }))
    .pipe(pug({ pretty: !isProd }))
    .pipe(dest('dist'))
    .pipe(browserSync.stream());
}
```

(Тут используется `gulp-data` — могу подключить и объяснить)

🔹 **Зачем нужно**

* Каждая страница полностью независима
* Можно использовать разную структуру данных
* Удобно масштабировать — даже на 100 страниц

---

## 💥 Что выбрать?

| Хочешь...                       | Рекомендую                                        |
| ------------------------------- | ------------------------------------------------- |
| Простота + общие переменные     | 1 большой `data.json`                             |
| Уникальные SEO, меню, заголовки | передавать `title`, `meta`, `activePage` в locals |
| Масштаб + многостраничный сайт  | `data/index.json`, `data/about.json` и т.д.       |

---

## 🚀 Готов сделать:

1. Пример шаблона с `title`, `description`, `activePage`
2. Или внедрить `gulp-data` + загрузку JSON по имени файла

Где копнём первым делом? 😎
