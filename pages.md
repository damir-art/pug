# about.pug, contacts.pug
💣 Протестируем маршруты — проверим, как `about.pug` и `contacts.pug` компилируются через наш шаблон `default.pug`. Погнали 🚀

---

## 📁 Структура `pages/`

```
📁 pages/
├── index.pug
├── about.pug
└── contacts.pug
```

---

## 📄 `about.pug`

```pug
extends ../layouts/default.pug

block page
  h1 О нас
  p Мы команда энтузиастов, создающая крутые сайты на Pug и Gulp 😎
```

---

## 📄 `contacts.pug`

```pug
extends ../layouts/default.pug

block page
  h1 Контакты
  p Свяжитесь с нами через email: 
  a(href="mailto:contact@example.com") contact@example.com
```

---

## ✅ Что должно получиться после сборки:

| Файл           | URL/Путь в `dist/`   | Заголовок на странице |
| -------------- | -------------------- | --------------------- |
| `index.pug`    | `dist/index.html`    | Привет!               |
| `about.pug`    | `dist/about.html`    | О нас                 |
| `contacts.pug` | `dist/contacts.html` | Контакты              |

---
