# Виртуальное Государство — прототип (React + Tailwind + shadcn/ui)

Этот репозиторий содержит прототип сайта «Цифровое Государство»: игровая и философская песочница с ИИ‑кабинетом министров, экономикой, гражданством и сообществом. В комплект включены: переключение темы (light/dark), мок‑аутентификация, мультиязычность (RU/EN) и скелеты API.

## ✨ Возможности

* ИИ‑кабинет министров (чат‑интерфейс, заглушка для LLM API)
* Экономика (демо‑графики Recharts)
* Гражданство (регистрация, статусы и прогресс)
* Философия/манифест (табы)
* Сообщество (инициативы и голосования)
* Песочница политик (режимы управления)
* Тёмная/светлая тема (Persist в `localStorage`)
* RU/EN переключение языка
* Мок‑аутентификация (Persist в `localStorage`)

## 🧱 Технологии

* React (SPA)
* Tailwind CSS
* shadcn/ui + lucide-react
* framer-motion (анимации)
* recharts (графики)

## 📦 Структура (минимум)

```
.
├─ src/
│  └─ App.(tsx|jsx)          # Главный компонент (из bolt.new)
├─ package.json
├─ index.html                 # если Vite
└─ README.md
```

> Если вы стартовали из bolt.new, просто скопируйте файл `App` из канваса в свой проект (или скачайте ZIP).

---

# ⬆️ Публикация в GitHub

Поскольку Git недоступен в этой среде разработки, вам нужно будет вручную загрузить проект в GitHub:

## Вариант 1: Загрузка через веб-интерфейс GitHub

1. Перейдите в ваш репозиторий: https://github.com/pachugonis/state
2. Нажмите "uploading an existing file" или "Add file" → "Upload files"
3. Скачайте все файлы из этого проекта и загрузите их в репозиторий
4. Добавьте commit message: "Initial commit: Digital State prototype"
5. Нажмите "Commit changes"

## Вариант 2: Клонирование и загрузка локально

Если у вас установлен Git на локальной машине:

```bash
# Скачайте проект как ZIP и распакуйте
# Затем в терминале:
git clone https://github.com/pachugonis/state.git
cd state
# Скопируйте все файлы проекта в эту папку
git add .
git commit -m "Initial commit: Digital State prototype"
git push origin main
```

---

# 🚀 Быстрый старт (после загрузки в GitHub)

## Требования

* Node.js 18+
* npm или pnpm / yarn

## Установка

```bash
# если проект только что распакован
npm install
# или
pnpm install
```

## Разработка

```bash
npm run dev     # старт dev-сервера (Vite/Next — в зависимости от шаблона)
```

Откройте ссылку из терминала (обычно `http://localhost:5173` для Vite или `http://localhost:3000` для Next.js).

## Сборка (прод)

```bash
npm run build
npm run preview  # локальный предпросмотр билда (Vite)
```

---

# 🔌 Интеграции и API

В коде уже есть функции‑заглушки:

* `apiChat(prompt, agentId)` → замените вызовом вашего бэкенда (например, Next.js API route `/api/chat`).
* `apiEconomy()` → подключите реальные временные ряды экономических показателей (`GET /api/economy`).
* `apiVote(pollId, choice)` → запишите голос пользователя (`POST /api/vote`).

### Пример Next.js API route (минимально)

```ts
// app/api/chat/route.ts (Next.js 13+)
import { NextResponse } from 'next/server'
export async function POST(req: Request) {
  const { prompt, agentId } = await req.json()
  // Вызов внешнего LLM API здесь
  return NextResponse.json({ ok: true, text: `LLM ответ на: ${prompt} (agent: ${agentId})` })
}
```

### ENV-переменные

Создайте `.env` и добавьте ключи для вашего LLM/бэкенда:

```
OPENAI_API_KEY=...         # пример
BACKEND_BASE_URL=...
```

В коде обращайтесь к ним через конфиг/роуты.

---

# 🌐 Деплой

## Вариант A: Vercel (рекомендовано)

1. Запушьте проект в GitHub (см. ниже раздел «Публикация в GitHub»).
2. Авторизуйтесь на [https://vercel.com](https://vercel.com) и импортируйте репозиторий.
3. Vercel сам определит фреймворк и соберёт проект.
4. Настройте переменные окружения в разделе **Settings → Environment Variables**.

## Вариант B: GitHub Pages (для Vite SPA)

1. Установите пакет gh-pages:

```bash
npm i -D gh-pages
```

2. В `package.json` добавьте:

```json
{
  "homepage": "https://<user>.github.io/<repo>",
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

3. Выполните деплой:

```bash
npm run deploy
```

> Для Next.js используйте Vercel или другой Node‑хостинг (GitHub Pages не поддерживает SSR/Edge‑функции).

---

# 🔄 GitHub Actions CI (опционально)

Пример простого workflow для Vite‑проекта:

```yml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 18 }
      - run: npm ci
      - run: npm run build
```

Создайте файл `.github/workflows/ci.yml` с этим содержимым для автоматической сборки.

---

# 🗂 Структура проекта (рекомендация)

```
src/
├─ components/            # переиспользуемые UI-компоненты
├─ features/              # доменные блоки (economy, citizenship, gov)
├─ lib/                   # утилиты, клиенты API
├─ i18n/                  # словари и хелперы локализации
└─ App.(tsx|jsx)
```

---

# 🧭 Настройка тематики и языков

* Тема хранится в `localStorage` ключом `ds-theme` и управляется через контекст.
* Язык — через контекст `LangContext` и словари `dictionaries` (RU/EN). Вынесите их в `src/i18n` при масштабировании.

# 🔐 Аутентификация

Текущая версия — мок (без сервера), хранит пользователя в `localStorage` (`ds-user`). Замените на реальную auth‑схему (например, OAuth/OpenID, Clerk/Auth0/Supabase).

# 📈 Данные экономики

Демо‑серии внутри компонента. Для продакшена — подключите ваш источник данных (REST/GraphQL/WebSocket) и обновляйте чарты реактивно.

---

# 🤝 Вклад

PR и issues приветствуются. Предложения по новым министерствам, экономическим моделям, этическим правилам ИИ — особенно.

# 📝 Лицензия

Добавьте файл `LICENSE` (например, MIT) и укажите авторство...

---

## EN (short)

This repo contains the **Digital State** prototype (React + Tailwind + shadcn/ui). It ships with light/dark theme, mock auth, RU/EN i18n and API stubs. Use Vercel for deployment, or GitHub Pages for Vite SPA. Replace API stubs with your backend routes.

### Quick Start

```bash
npm install
npm run dev
```

### Deploy to GitHub Pages (Vite)

```bash
npm i -D gh-pages
# add scripts: predeploy/build and deploy
npm run deploy
```

Contributions are welcome! 🎉

