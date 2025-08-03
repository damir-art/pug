# index.pug
💪 Начинаем с **одного файла `pages/index.pug`**, без `layout`, без миксинов, без include'ов.

В папке `pages/` размещают страницы сайта: index.pug, about.pug, contact.pug и т.п.

---

## 📄 `src/pug/pages/index.pug`

Вот самый простой рабочий пример:

```pug
doctype html
html(lang="ru")
  head
    meta(charset="UTF-8")
    title Мой первый Pug-сайт
    link(rel="stylesheet", href="css/style.css")
  body
    h1 Привет, мир!
    p Это мой первый сайт, собранный на Pug 🎉
    a(href="#") Кликни сюда
```

---

## 📝 Что тут важно:

| Элемент         | Как в HTML                   | Как в Pug                         |
| --------------- | ---------------------------- | --------------------------------- |
| Теги            | `<h1>Текст</h1>`             | `h1 Текст`                        |
| Атрибуты        | `<a href="#">Ссылка</a>`     | `a(href="#") Ссылка`              |
| Вложенность     | `<div><p>Текст</p></div>`    | `div p Текст` (с отступами, p на другой строке сделать отступ табом)   |
| Классы и id     | `<div class="box" id="one">` | `div#one.box`                     |
| Подключение CSS | `<link rel="stylesheet"...`  | `link(rel="stylesheet" href="...` |

---

## 🧪 Как проверить:

1. Этот файл должен быть: `src/pug/pages/index.pug`
2. В `gulpfile.js` у тебя должно быть:

```js
return src('src/pug/pages/*.pug')
```

3. После команды `gulp` → будет создан `dist/index.html`

---

Хочешь — в следующем шаге подключим `layout/base.pug` и разберём `extends + block`
А пока можешь попробовать поменять `title`, `lang`, сделать пару блоков — и увидишь как всё просто 😎

Если хочешь, могу дать тебе 5 мини-заданий по Pug прямо в этом стиле.
