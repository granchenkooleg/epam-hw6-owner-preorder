# Домашнє завдання #6 — екран Owner групового pre-order

**Виконано:** практичне, рівень 1, рівень 2 (на моках). Рівень 3 і бонус (синхронізація кожні 15 с, Remove Guest) — реалізовано, тести проходять, але зі справжнім API не перевірено.

## Де код

| Шлях (в `frontend/restaurant-frontend-app/src/app/`) | Що там |
|---|---|
| `pages/group-order-page/` | сторінка Owner: стани loading / error / 404 / empty, консолідоване замовлення, Submit, синхронізація 15 с, Remove Guest |
| `components/group-orders/participant-card/` | картка учасника |
| `services/group-orders/group-orders.service.ts` | запити до API (5 ендпоінтів) |
| `auth/token.interceptor.ts` (+ spec) | виправлення interceptor, див. нижче |
| `app.routes.ts` | маршрут `reservations/:reservationId/group-order` |

## Перевірка

```bash
cd frontend/restaurant-frontend-app
nvm use && npm ci
npm run test:hw      # 67/67
npm start            # http://localhost:4200/reservations/demo-reservation/group-order?demo
```

Тести: `npm run test:hw` — 67/67 у headless Chromium 141 (Linux). `tsc` (app і spec) — без помилок, `npm run build` — «Bundle check passed».

## Відхилення і виправлення

- **tokenInterceptor:** `SKIP_ERROR_TOAST` тепер працює і для `/guest/` запитів — фонова синхронізація не показує toast кожні 15 с. Додатково Owner-токен додається до `/guest/groups/…` (ці маршрути захищені Cognito). Є тести, один з них падає без фіксу.
- **Макет 02 і тест рівня 2 суперечать одне одному:** макет показує сіру «Submit Pre-Order», а тест вимагає, щоб такої кнопки не було. Зроблено неактивну «Pre-Order Sent».
- **Рівень 3:** перевірено на моках і фейковому HTTP-сервері; зі справжнім API не перевірено.

---

# Green & Tasty — Frontend · домашка IT Marathon 6.0

Фронтенд застосунку Green & Tasty (Angular 22) — стартовий код для домашнього завдання.
Бекенду в цьому архіві немає: для рівнів 1–2 він не потрібен.

## Старт

Потрібен Node.js 22.22+ або 24.x (див. `frontend/restaurant-frontend-app/.nvmrc`).

```bash
cd frontend/restaurant-frontend-app
npm ci
npm start
```

Відкрий **http://localhost:4200/reservations/demo-reservation/group-order?demo**
(`?demo` — мок-логін: акаунт і бекенд не потрібні). Сторінку з цієї адреси ти зробиш на практичному, а поки тебе перекине на головну. Там будуть помилки завантаження меню й локацій — бекенду в архіві немає, для домашки це нормально.

## Що читати

| Файл | Що там |
|---|---|
| `practice-frontend.md` | практичне на ~15 хв: картка учасника і сторінка — з цього починається домашка |
| `homework-frontend.md` | домашка: 3 рівні, що зробити, умови, тести |
| `homework-design/` | дизайн з Figma (PNG) + що з макета входить у домашку. Оригінал: [Figma](https://www.figma.com/design/P9rkBNLJZ3WabXSQX9b5DO/EPAM-IT-Marathon-6.0?node-id=62645-20592) |
| `frontend/restaurant-frontend-app/homework-tests/` | **готові тести** до практичного і кожного рівня — копіюєш у `src/app/`, запускаєш `npm run test:hw` |
| `homework-backend.md` | контракт API (ті самі ендпоінти, що відповідає мок) |
| `frontend/restaurant-frontend-app/CLAUDE.md` | правила проєкту: куди класти файли, стилі, тести (зручно давати AI-асистенту) |

## Перевірки перед здачею

```bash
cd frontend/restaurant-frontend-app
npx tsc -p tsconfig.app.json  --noEmit
npx tsc -p tsconfig.spec.json --noEmit
BACKEND_URL=https://example.com npm run build
npm run test:hw
```
