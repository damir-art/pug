# Как использовать JavaScript в Pug?

---

## 🧠 Как использовать JavaScript в Pug

В Pug можно использовать **чистый JS прямо в шаблоне**, особенно в верхней части или внутри блоков, почти как в EJS:

---

### 🔸 Объявление переменной (как ты уже видел):

```pug
- var name = 'Даян'
- var items = ['картошка', 'грузовик', 'Pug']
```

---

### 🔸 Условия (`if`, `else`, `unless`)

```pug
- var isLoggedIn = true

if isLoggedIn
  p Добро пожаловать!
else
  p Пожалуйста, войдите
```

`unless condition` равносильно `if !condition`

---

### 🔸 Циклы (`each`)

```pug
- var items = ['картошка', 'грузовик', 'Pug']

ul
  each item in items
    li= item
```

👉 Это даст:

```html
<ul>
  <li>картошка</li>
  <li>грузовик</li>
  <li>Pug</li>
</ul>
```

---

### 🔸 Вызов функции

```pug
- var upper = (str) => str.toUpperCase()
h2= upper('pug рулит')
```

👉 Получишь:

```html
<h2>PUG РУЛИТ</h2>
```

---

### 🔸 Комментарии в коде:

```pug
// Это HTML-комментарий (виден в браузере)
//- Это Pug-комментарий (не попадает в HTML)
```

---

## 🧪 Мини-практика

Добавь в `block page`:

```pug
- var user = 'Даян'
- var loggedIn = true

h2 Привет, #{user}!

if loggedIn
  p Вы вошли в систему
else
  p Авторизуйтесь, чтобы продолжить

- var tech = ['Pug', 'SCSS', 'Gulp']

ul
  each item in tech
    li= item
```
