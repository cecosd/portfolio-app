# Моето Портфолио — Проектна документация

## Обобщение

Безплатен, офлайн тракер на лични финанси — един HTML файл, който работи в браузъра без сървър, без регистрация, без абонамент. Данните се пазят в `localStorage` на браузъра. Създаден за хора с разпръснати активи (фондове, метали, крипто, кеш) и задължения (кредити), които искат да виждат цялата картина на едно място.

**URL (production):** `https://tsvetanpetrov.com/portfolio/`

**Автор:** Цветан Петров

**PayPal за подкрепа:** `https://paypal.me/sdevelopers`

---

## Файлова структура

| Файл | Описание |
|---|---|
| `portfolio_app.html` | Самото приложение — React 18 + Tailwind + Chart.js, всичко inline |
| `README.md` | Тази документация |

---

## Технически стек

- **React 18** от CDN (production build)
- **Babel Standalone** за JSX transpile в браузъра
- **Tailwind CSS** от CDN (с `darkMode: 'class'`)
- **Chart.js 4.4.1** от CDN (Doughnut + Line charts)
- **localStorage** за съхранение
- **JSON export/import** — архивен формат v2 с данни + история
- **PWA** — Web App Manifest + Service Worker за офлайн и install prompt
- Няма backend, няма build стъпка, няма npm

---

## Storage ключове

| Ключ | Описание |
|---|---|
| `portfolio_list_v1` | Списък на всички портфолиа `[{id, name}]` |
| `portfolio_active` | ID на активното портфолио |
| `portfolio_app_v2` | Данни на "default" портфолиото |
| `portfolio_app_v2_{id}` | Данни на допълнителните портфолиа |
| `portfolio_history_v1` | История на "default" портфолиото |
| `portfolio_history_v1_{id}` | История на допълнителните портфолиа |
| `portfolio_dark` | Dark mode preference |

---

## Data model

```json
{
  "funds": [{
    "id": "abc123", "category": "Vanguard",
    "name": "Vanguard FTSE All-World ETF", "isin": "IE00B3RBWM25",
    "units": 12.5, "avgPrice": 95.20, "invested": 1190,
    "currentNav": 108.40, "currentValue": 1355, "currency": "EUR"
  }],
  "metals": [{
    "id": "dm1", "name": "Златно кюлче 10g",
    "weightGrams": 10, "pricePerGram": 92, "currency": "EUR",
    "type": "gold", "purchaseDate": "2024-03-15"
  }],
  "crypto": [{
    "id": "dc1", "name": "BTC",
    "balance": 0.025, "priceUsd": 104500, "currency": "USD"
  }],
  "loans": [{
    "id": "dl1", "name": "Потребителски кредит",
    "principal": 8500, "monthlyPayment": 195, "interestRate": 5.2,
    "nextPaymentDate": "2026-06-15", "remainingPayments": 48,
    "originalPayments": 60, "currency": "EUR",
    "payments": ["2026-01-15", "2026-02-15"]
  }],
  "cash": 5200,
  "fundCategories": ["Vanguard", "iShares", "Amundi", "NN"],
  "rates": { "USD_EUR": 0.92 }
}
```

## Export archive format (v2)

```json
{
  "version": 2,
  "exportDate": "2026-05-18T10:30:00.000Z",
  "data": { /* portfolio data */ },
  "history": [ /* array of daily snapshots */ ]
}
```

Импортът поддържа и стар формат (само `{funds, metals, ...}` без `version`).

---

## Табове (8 бр.)

| Таб | Описание |
|---|---|
| **Табло** | Hero карта с нетна стойност, 6 категорийни карти, Doughnut chart (по клас / детайлно), резултат на фондовете, активи vs задължения бар |
| **История** | Автоматични дневни снимки + drag-and-drop за стари JSON файлове, 4 chart режима, delta таблица |
| **Фондове** | Динамични категории, групирано показване, CRUD, ↑↓ reorder, P&L индикатор |
| **Метали** | Злато/Сребро/Платина, тегло в грамове, цена/грам, дата на покупка |
| **Крипто** | Име, баланс, USD цена, автоматично USD→EUR конвертиране |
| **Кеш** | Едно EUR поле + бързи бутони ±100/500/1000 |
| **Кредити** | Пълен амортизационен план, mark as paid, прогрес бар |
| **Настройки** | Dark mode, портфолиа, "какво ако" симулатор, категории, валутен курс, reset, PayPal |

---

## Функции

### Onboarding Wizard
При първо отваряне — три опции: празно портфолио, импорт на JSON, демо данни (фиктивен инвеститор).

### Auto-snapshot
При всяка промяна на данните — автоматичен snapshot за днес. Макс 1 на ден, презаписва при нова промяна.

### Множество портфолиа
- Списък в `portfolio_list_v1`, dropdown в header-а при 2+ портфолиа
- Създаване, преименуване, изтриване в Настройки
- Всяко портфолио: собствен storage key и history key
- Default портфолио ползва оригиналните ключове (обратна съвместимост)
- Reset трие само активното

### "Какво ако" симулатор
В Настройки (при наличие на кредити): избери кредит + сума → виж спестена лихва и по-малко месеци.

### Dark mode
Toggle в header-а (🌙/☀️) и switch в Настройки. Tailwind `darkMode: 'class'`. Запомня в localStorage.

### Търсене
Search bar под header-а — видим само на Фондове, Метали, Крипто, Кредити. Филтрира по име, категория, ISIN, тип.

### PWA
Web App Manifest + Service Worker (cache-first). `beforeinstallprompt` лента с "Инсталирай" бутон. Apple meta тагове.

### Reorder на фондове
↑↓ бутони на всеки фонд за преместване в списъка.

### Валути
Базова: EUR. Поддържани: EUR, USD. Курс USD→EUR ръчно в Настройки.

---

## Лого

SVG shield + pie chart + diamond accent.

- Shield: `#6d28d9`
- Pie segments: `#a78bfa` (фондове), `#fbbf24` (злато), `#34d399` (крипто), `#38bdf8` (кеш)
- Center diamond: `#c4b5fd`
- Inline SVG + data URI favicon

---

## Landing page

9 секции: Nav → Hero → Features (9 карти) → Screens showcase (6 интерактивни таба) → Как работи (3 стъпки) → Документация (8 карти) → Privacy banner → CTA (свали + отвори + PayPal) → Footer.

Дизайн: Fraunces + DM Sans, grain texture, `--accent: #6d28d9`, scroll animations.

---

## Персонална страница

LinkedIn: `https://www.linkedin.com/in/tsvetanpetrov/`

---

## Обратна съвместимост

- Default портфолио = оригинални ключове
- Импорт: стар формат (само data) + нов (archive v2)
- Dark mode default = false
- Новите features не променят data model-а

---
