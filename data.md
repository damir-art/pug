# Данные и 
# 🧠 Шаг 7: Комментарии, переменные сайта и данные (`data.pug`, JSON)

---

## 🔹Переменные сайта: `data/siteData.pug`
Обычно ты делаешь один файл, где задаются общие переменные.

### Пример:

```pug
//- data/siteData.pug
- var site = {
    title: 'Доставка из Китая',
    tagline: 'Надёжно, как китайская сталь',
    phone: '+7 900 000 00 00',
    year: 2025
  }
```

---

### Подключение в `default.pug`:

```pug
include ../data/siteData.pug
```

### Использование:

```pug
title= site.title
p Телефон: #{site.phone}
```

---

## 🔹Данные из JSON-файла

### ❗ Pug сам по себе **не умеет** читать `.json`, но в Gulp ты можешь это настроить через `require()`.

Пример с использованием JSON (если в `data/site.json`):

```js
// gulpfile.js — например, используешь pug plugin с locals
const site = require('./src/pug/data/site.json');

pug({
  locals: {
    site
  }
})
```

И тогда в Pug-файле ты просто используешь `site.title`, как обычно.

---

### ✅ Пример JSON-файла

```json
{
  "title": "Доставка с Aliexpress",
  "tagline": "Без растаможки не высылаем",
  "year": 2025
}
```

---

## 🧪 Советы по структуре `data/`:

Ты можешь разделить:

```
data/
├── siteData.pug      # Заголовки, слоган, контакты
├── content.pug       # Текстовые блоки, описание, преимущества
├── team.pug          # Участники команды (массив объектов)
├── menu.pug          # Меню навигации
```

И потом подключать их в нужные шаблоны — это очень удобно для масштабируемых проектов.

---

Вопросы:
- "include ../data/siteData.pug" подключаем в default.pug или base.pag
- "в Gulp ты можешь JSON настроить через `require()`" может через import по современному?
- тут pug({locals: { site }}), как подключить два файла JSON?
- как выглядит код в siteData.pug

Ответы:
Ооо ты заряжен, как контейнер на китайском складе перед отправкой! 🚢
Разберём всё по пунктам — уверенно, чётко, без лапши.

---

## ✅ 1. Где подключать `include ../data/siteData.pug` — в `base.pug` или `default.pug`?

👉 **Лучше в `default.pug`**, потому что именно он используется для конкретных страниц и может тянуть **разные данные для разных макетов**.

> Если ты подключишь `siteData` в `base.pug`, он будет грузиться **всегда и везде**, даже если не нужен.

📌 **Итог**:

* `base.pug` — только HTML-структура: `html`, `head`, `body`, `block content`
* `default.pug` — уже что-то подключает: данные, head, header, footer и т.д.

---

## ✅ 2. Можно ли использовать `import` вместо `require()` в Gulp?

👎 К сожалению, **нет напрямую**, потому что `import` работает только в `type: "module"` или через Babel/ESM-сборки.

👍 А вот `require()` работает **сразу** в `gulpfile.js` (если ты не на ESM).
Так что остаёмся пока на `require()`:

```js
const site = require('./src/pug/data/site.json');
const content = require('./src/pug/data/content.json');
```

---

## ✅ 3. Как подключить два файла JSON?

Легко!

```js
const site = require('./src/pug/data/site.json');
const content = require('./src/pug/data/content.json');

pug({
  locals: {
    site,
    content
  }
})
```

Теперь в любом `.pug` ты можешь использовать:

```pug
title= site.title
p= content.heroText
```

---

## ✅ 4. Как выглядит `siteData.pug` (если без JSON)

```pug
//- siteData.pug
- var site = {
    title: 'Доставка из Китая',
    tagline: 'Быстро. Дёшево. Гарантия.',
    phone: '+7 900 000 00 00',
    email: 'info@kitay.ru',
    year: 2025
  }
```

> 📌 Подключаешь в `default.pug`:

```pug
include ../data/siteData.pug
```

И дальше используешь:

```pug
title= site.title
h1= site.tagline
p Свяжитесь с нами: #{site.phone}
```

---

## ⚠️ Краткий итог:

| Что                         | Где                                                       |
| --------------------------- | --------------------------------------------------------- |
| `siteData.pug`              | Подключается в `default.pug`                              |
| JSON                        | Подключается в `gulpfile.js` через `require()`            |
| `locals: { site, content }` | Даёт тебе доступ к `site.title` и т.д.                    |
| `import`                    | Только если на `type: "module"` или ESM — для продвинутых |

Вопросы:
- У меня современный Gulp где в package.json стоит type: module

Сейчас у тебя Pug компилируется без `locals`, поэтому он не видит переменные из `site.json` или `siteData.pug`.

---

## 🔧 Давай улучшим твой `compilePug`, чтобы он:

* Загружал JSON-данные из `data/`
* Передавал их как `locals` в `.pug`
* Работал с `import`, потому что у тебя `type: module`

---

## ✅ Новый `compilePug` с импортом JSON:

```js
// ✅ Вверх файла:
import fs from 'fs/promises';

// 🧱 compilePug c данными
export async function compilePug() {
  // Загружаем JSON-файлы
  const site = JSON.parse(await fs.readFile('./src/pug/data/site.json', 'utf-8'));
  const content = JSON.parse(await fs.readFile('./src/pug/data/content.json', 'utf-8'));

  return src(paths.pug.pages)
    .pipe(plumber())
    .pipe(pug({
      pretty: !isProd,
      locals: {
        site,
        content
      }
    }))
    .pipe(dest(paths.pug.dest))
    .pipe(browserSync.stream());
}
```

---

## ✅ Теперь в любом `.pug` можно писать:

```pug
h1= site.title
p= site.tagline
h2= content.heroTitle
p= content.heroText
```

---

## 💡 Убедись, что у тебя структура такая:

```
src/
└── pug/
    ├── data/
    │   ├── site.json
    │   └── content.json
    ├── pages/
    │   └── index.pug
    └── main.pug
```

---

## 🔥 Бонус: как делать динамическую загрузку всех JSON-файлов (если файлов много)

```js
import fs from 'fs/promises';
import path from 'path';

const loadData = async () => {
  const dataDir = './src/pug/data';
  const files = await fs.readdir(dataDir);
  const result = {};

  for (const file of files) {
    if (file.endsWith('.json')) {
      const key = path.basename(file, '.json'); // site, content и т.д.
      const content = await fs.readFile(path.join(dataDir, file), 'utf-8');
      result[key] = JSON.parse(content);
    }
  }

  return result;
};

export async function compilePug() {
  const locals = await loadData();

  return src(paths.pug.pages)
    .pipe(plumber())
    .pipe(pug({ pretty: !isProd, locals }))
    .pipe(dest(paths.pug.dest))
    .pipe(browserSync.stream());
}
```

Теперь можешь просто добавлять JSON-файлы — они все сами подключатся как `site`, `content`, `team` и т.д.

---

Скажи, если хочешь такую же автоматизацию, но для `.pug`-данных, например: `data/siteData.pug` или массивов через `- var` — тоже сделаем!
