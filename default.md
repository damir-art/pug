# default.pug
## 🪄 Теперь создаём `layouts/default.pug`

Этот файл будет **наследовать `base.pug`**, но добавлять:

* `include ../sections/header.pug`
* `block page` — чтобы каждая страница добавляла только содержимое
* `include ../sections/footer.pug`

---

### 📄 `layouts/default.pug`

```pug
extends ./base.pug

block content
  include ../sections/header.pug

  block page

  include ../sections/footer.pug
```

---

### 📄 А теперь `pages/index.pug` должен выглядеть так:

```pug
extends ../layouts/default.pug

block page
  h1 Добро пожаловать!
  p Это мой сайт, собранный на Pug и Gulp 😎
```

---

### 🎯 Как это работает:

* `index.pug` → наследует `default.pug`
* `default.pug` → наследует `base.pug`
* В `base.pug` есть `block content`
* В `default.pug` этот `block content` заполняется шапкой + футером + `block page`
* А `block page` уже наполняется в каждой странице по-разному

---
