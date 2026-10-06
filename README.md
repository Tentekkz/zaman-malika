# 💍 Свадебный сайт-приглашение: Заман & Малика

Премиальный одностраничный свадебный лендинг в стиле «Тихий люкс» с мобильной оптимизацией (Mobile-first) и мгновенной записью гостей в Google Таблицу через Google Apps Script.

- **Молодожены**: Заман & Малика
- **Дата и время**: 7 ноября 2026, 17:00
- **Место проведения**: Ресторанный комплекс «Grand Sultan»
- **Адрес**: Алматинская обл., Көктөбе 1, ул. Тәуелсіздік, 1а
- **Языки**: Двуязычный сайт (Қазақша / Русский) с окном выбора при входе и переключателем в шапке
- **Слайдер**: Автоматическое слайд-шоу из 3 фотографий (свайп для смартфонов + стрелки + точки)

---

## 🚀 Быстрый старт: Интеграция с Google Таблицей

### Шаг 1. Создание таблицы и скрипта (2 минуты)
1. Откройте [Google Sheets](https://sheets.new) и создайте новую таблицу (назовите её: `Свадьба Заман & Малика — Список гостей`).
2. В первой строке (`A1:F1`) укажите названия колонок:
   - **A1**: `Дата и время`
   - **B1**: `Имя гостя`
   - **C1**: `Придет?`
   - **D1**: `С кем будет`
   - **E1**: `Напитки`
   - **F1**: `Пожелания`
3. В верхнем меню нажмите: **Расширения (Extensions) → Apps Script**.
4. Удалите стандартный код и вставьте следующий скрипт:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    sheet.appendRow([
      new Date().toLocaleString("ru-RU", { timeZone: "Asia/Almaty" }),
      data.name || "",
      data.attendance || "",
      data.partner || "Один",
      data.drinks || "Не выбрано",
      data.comment || ""
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({ result: "success" }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({ result: "error", error: error.message }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

5. Справа вверху нажмите синюю кнопку **Развернуть (Deploy) → Новое развертывание (New deployment)**.
6. Выберите тип: **Веб-приложение (Web app)**:
   - **Описание**: `RSVP Form`
   - **Выполнять от имени**: `Меня (Me)`
   - **У кого есть доступ**: `Все (Anyone)` *(Критически важно: именно «Anyone», чтобы форма могла сохранять ответы гостей без входа в аккаунт Google!)*
7. Нажмите **Развернуть (Deploy)**, разрешите доступ к аккаунту и скопируйте **URL веб-приложения** (вида `https://script.google.com/macros/s/.../exec`).

---

### Шаг 2. Подключение URL в `index.html`
1. Откройте файл `index.html`.
2. Найдите строку:
   ```javascript
   const SCRIPT_URL = 'ТВОЙ_GOOGLE_APPS_SCRIPT_URL';
   ```
3. Вставьте туда ваш скопированный URL вместо `'ТВОЙ_GOOGLE_APPS_SCRIPT_URL'`.

---

## 🌐 Публикация в интернет (Бесплатно за 1 минуту)

### Вариант 1: GitHub Pages
1. Создайте репозиторий на GitHub (например, `zaman-malika-wedding`).
2. Загрузите файлы `index.html` и папку `assets/`.
3. В настройках репозитория (**Settings → Pages**) выберите ветку `main` / `root` и нажмите Save.
4. Через минуту ваш сайт будет доступен по ссылке вида `https://ваш-логин.github.io/zaman-malika-wedding/`.

### Вариант 2: Netlify Drop или Vercel
- Просто перетащите папку с сайтом на [app.netlify.com/drop](https://app.netlify.com/drop) — сайт мгновенно заработает в сети.

---

## 📱 Рассылка гостям
Сократите полученную ссылку через [Dub.co](https://dub.co) или [TinyURL](https://tinyurl.com), сделайте красивое название (например, `dub.sh/zaman-malika`) и отправляйте в WhatsApp и Telegram!
