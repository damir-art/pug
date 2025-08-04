# 🧠 Условия, циклы, массивы

---

## 🔹 Что разберём:
### ✅ `if`, `else`
### ✅ `unless` — “если НЕ”
### ✅ `each` — перебор массива
### ✅ `while` — цикл по условию (редко, но бывает)

---

## 🟢 1. `if`, `else`

```pug
- var isLoggedIn = true

if isLoggedIn
  p Добро пожаловать, командир!
else
  p Введите пароль, товарищ!
```

👉 Вывод:

```html
<p>Добро пожаловать, командир!</p>
```

---

## 🟢 2. `unless` (обратный `if` — “если НЕ”)

```pug
- var isAdmin = false

unless isAdmin
  p У вас нет доступа к секретным данным
```

---

## 🟢 3. `each` — перебор массива

```pug
- var users = ['Андрей', 'Катя', 'Даян']

ul
  each user in users
    li= user
```

👉 Результат:

```html
<ul>
  <li>Андрей</li>
  <li>Катя</li>
  <li>Даян</li>
</ul>
```

🧠 Можно добавить `index`:

```pug
each user, i in users
  p #{i + 1}) Привет, #{user}!
```

---

## 🟢 4. `while` — как в JS, но редко нужен

```pug
- var i = 0

ul
  while i < 3
    li Пункт #{i + 1}
    - i++
```

---

## 📄 Пример с внешним массивом

Создай `data/content.pug`:

```pug
- var features = [
    { icon: '🚀', title: 'Быстро', desc: 'Работает мгновенно' },
    { icon: '🔒', title: 'Безопасно', desc: 'Никаких вирусов' },
    { icon: '🎨', title: 'Красиво', desc: 'Выглядит отпадно' }
  ]
```

📄 В `pages/index.pug`:

```pug
include ../data/content.pug

block page
  h2 Наши преимущества

  ul.features
    each feat in features
      li
        h3 #{feat.icon} #{feat.title}
        p= feat.desc
```

Можно сделать что-то сложнее — с условием внутри `each`, например: не показывать пустые `desc` или фильтровать по типу.
