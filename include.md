# include, extends, block
Это уже почти как **наследование классов в ООП**, только в HTML-шаблонах! 💻
Переходим к **инклюдам и экстендам** — это фундамент твоей архитектуры Pug, без него всё будет как лапша без соли.

---

# 🧠 Инклюды и экстенды (include, extends, block)

---

## 🧩 `include` — Вставка других Pug-файлов

Ты вставляешь код другого файла прямо внутрь текущего.

### ✅ Используется для:

* компонентов (`components/*.pug`)
* секций (`sections/*.pug`)
* миксинов, данных

```pug
include ../sections/header.pug
include ../sections/footer.pug
```

> 📌 `include` — **копирует содержимое** файла как есть в то место, где он вызван.

---

## 🧬 `extends` — Наследование шаблона

Ты говоришь: "Эта страница будет основана на другом шаблоне (обычно `layouts/default.pug`)".

```pug
extends ../layouts/default.pug
```

> 📌 `extends` — **определяет родительский шаблон**, от которого наследуется текущий файл.

---

## 🔳 `block` — Места для замены в родительском шаблоне

В родительском шаблоне (`default.pug`) ты создаёшь **"пустые блоки"**, которые потом заполняешь в дочерних файлах.

### 📄 `layouts/default.pug`

```pug
extends base.pug

block content
  // сюда попадёт то, что определено в pages/*.pug
```

---

### 📄 `pages/index.pug`

```pug
extends ../layouts/default.pug

block content
  h1 Главная страница
  p Добро пожаловать!
```

👉 В итоге: `base.pug` → `default.pug` → `index.pug` — как **матрёшка** 🪆

---

## ⚡ Комбинируем всё вместе:

### 📄 `default.pug`

```pug
extends base.pug

include ../sections/header.pug

block content

include ../sections/footer.pug
```

### 📄 `index.pug`

```pug
extends ../layouts/default.pug

block content
  h1= site.title
  p= site.tagline
```

---

## 🧪 Мини-практика

1. Проверь, что в `default.pug` есть:

```pug
extends base.pug

include ../sections/header.pug

block content

include ../sections/footer.pug
```

2. В `pages/index.pug`:

```pug
extends ../layouts/default.pug

block content
  h1 Добро пожаловать!
  p Это тестовый вывод
```

---

Можно сделать кастомный `include` из `components/card.pug` с параметрами через `data/`.

# Более понлый пример
Доведём систему до полной **боевой готовности**, чтобы `site.title` и `site.tagline` не вызывали вопросов, и разложим всё по местам.

---

## 💾 Полная связка: `base.pug`, `default.pug`, `index.pug` + `siteData.pug`

---

### ✅ `layouts/base.pug` — база всего сайта (html + head + body)

```pug
doctype html
html(lang="ru")
  head
    include ../helpers/head.pug
    block head
  body
    block content
    include ../helpers/scripts.pug
```

---

### ✅ `layouts/default.pug` — шаблон страницы, расширяющий `base.pug`

```pug
extends base.pug

include ../data/siteData.pug
include ../sections/header.pug

block content

include ../sections/footer.pug
```

---

### ✅ `data/siteData.pug` — переменные сайта

```pug
- var site = {
    title: 'Сайт доставки из Китая',
    tagline: 'Быстро, надёжно, с кэшбэком в юанях',
    year: 2025
  }
```

> 📦 Ты можешь сюда добавить любые переменные: ссылки, соцсети, меню, партнёров и т.д.

---

### ✅ `pages/index.pug` — сама страница

```pug
extends ../layouts/default.pug

block content
  h1= site.title
  p= site.tagline
```

---

## 🔄 А в `helpers/head.pug` можно положить что-нибудь такое:

```pug
meta(charset="UTF-8")
meta(name="viewport", content="width=device-width, initial-scale=1.0")
title= site.title
link(rel="stylesheet", href="/style.css")
```

---

## 🧪 Что получится в HTML:

```html
<html lang="ru">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Сайт доставки из Китая</title>
    <link rel="stylesheet" href="/style.css">
  </head>
  <body>
    <header>...</header>
    <h1>Сайт доставки из Китая</h1>
    <p>Быстро, надёжно, с кэшбэком в юанях</p>
    <footer>...</footer>
    <script src="/script.js"></script>
  </body>
</html>
```
