# footer.pug
Подвал сайта.

## 📄 `sections/footer.pug`

```pug
footer.site-footer
  p © #{new Date().getFullYear()} Мой Сайт. Все права защищены.
```

> Фишка: можно сделать автообновление года через JS или mixin позже 😎

---

## 🧪 Появится в `dist/index.html`:

```html
<!DOCTYPE html>
<html lang="ru">
  <head>
    ...
  </head>
  <body>
    <header class="site-header">
      <nav class="navbar">
        <ul class="menu">
          <li><a href="/">Главная</a></li>
          <li><a href="/about.html">О нас</a></li>
          <li><a href="/contacts.html">Контакты</a></li>
        </ul>
      </nav>
    </header>

    <h1>Привет!</h1>
    <p>Это главная страница моего Pug-проекта</p>

    <footer class="site-footer">
      <p>© 2025 Мой Сайт. Все права защищены.</p>
    </footer>
  </body>
</html>
```
