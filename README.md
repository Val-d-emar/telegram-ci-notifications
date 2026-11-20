# 🧪 Telegram CI Notifications

[🇷🇺 Русский](#русский) | [🇬🇧 English](#english)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

<a name="english"></a>

## 🇬🇧 English

A simple, fast, and pure-Bash GitHub Action to send Telegram notifications about GitHub Actions CI workflow results. **Zero dependencies, no Node.js required!**

### ✨ Features

- **Zero dependencies:** Runs instantly using pure Bash and `curl`.
- **Full CI Context:** Shows branch or PR link, short commit SHA, commit title, author, and a direct link to CI logs.
- **Flexible Filtering:** Optional `notify_on_success` toggle to alert only on failures if desired.
- **Smart Mentioning:** Maps GitHub usernames to Telegram usernames.
- **Bot Ignoring:** Automatically ignores runs triggered by bots (e.g., `dependabot`).
- **Thread Support:** Can send messages to specific topics in Telegram Supergroups.
- **i18n:** Supports English and Russian notification languages.
- **Safe HTML Escaping:** Escapes `<`, `>`, `&` and uses URL-encoding so special characters in commit messages or PR titles never break Telegram `HTML` parsing.

### 🚀 Supported Outcomes

- `success` — CI checks passed (can be muted via `notify_on_success: "false"`)
- `failure` — CI checks failed
- `cancelled` — CI run was cancelled

### 🛠 Usage

Create or update your CI workflow (e.g., `.github/workflows/ci.yml`). Make sure to add `if: always()` to the notification step so alerts are sent even when tests or builds fail:

```yaml
name: CI Checks

on:
  push:
    branches: [main]
  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      - name: Run tests
        run: npm test

      - name: Notify CI status in Telegram
        if: always()
        uses: Val-d-emar/telegram-ci-notifications@v1
        with:
          bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
          job_status: ${{ job.status }}
          # message_thread_id: "12345" # Optional: for topic groups
          user_map: ${{ secrets.TELEGRAM_USER_MAP }}
          ignore_bots: "true"
          notify_on_success: "true"
          language: "en"
```

### ⚙️ Inputs

| Input               | Required | Default | Description                                           |
| ------------------- | -------- | ------- | ----------------------------------------------------- |
| `bot_token`         | **Yes**  |         | Your Telegram Bot Token.                              |
| `chat_id`           | **Yes**  |         | Target chat ID (Group, Channel, or User).             |
| `job_status`        | **Yes**  |         | Current job status (usually `${{ job.status }}`).     |
| `message_thread_id` | No       |         | Target Thread ID (if your group uses Topics).         |
| `user_map`          | No       |         | Mapping of GitHub usernames to Telegram usernames.    |
| `ignore_bots`       | No       | `true`  | Ignore runs triggered by `[bot]` accounts.            |
| `notify_on_success` | No       | `true`  | Send notification when CI passes (`true` or `false`). |
| `language`          | No       | `ru`    | Notification language (`en` or `ru`).                 |

### 👥 User Mapping (`user_map`)

To ping correct Telegram users, create a GitHub Secret (e.g., `TELEGRAM_USER_MAP`) with the following format:

```text
github_username1=telegram_username1
github_username2=@telegram_username2
```

_Note: The `@` symbol for Telegram usernames is optional, the action will add it automatically if missing._

### License

This project is licensed under the [MIT License](LICENSE).

---

<a name="русский"></a>

## 🇷🇺 Русский

Простой и быстрый GitHub Action на чистом Bash для отправки уведомлений о статусе прохождения CI в Telegram. **Никаких зависимостей и сборок на Node.js!**

### ✨ Особенности

- **Zero dependencies:** Работает мгновенно, использует только Bash и `curl`.
- **Понятный контекст:** Показывает ветку или ссылку на PR, хэш и заголовок коммита, автора и прямую ссылку на логи CI.
- **Гибкость:** Параметр `notify_on_success` позволяет при желании отключить уведомления об успешных сборках и слать только падения.
- **Умные упоминания:** Связывает никнеймы GitHub с аккаунтами в Telegram.
- **Игнорирование ботов:** Не спамит в чат от лица `dependabot` и других `[bot]`-аккаунтов.
- **Поддержка топиков (Threads):** Умеет отправлять сообщения в конкретные ветки супергрупп.
- **i18n:** Поддерживает русский и английский языки.
- **Безопасность:** Экранирует `<`, `>`, `&` и использует URL-encoding, поэтому спецсимволы и кавычки в коммитах или заголовках PR не сломают отправку в Telegram.

### 🚀 Поддерживаемые статусы

- `success` — все проверки CI прошли успешно (можно отключить через `notify_on_success: "false"`)
- `failure` — одна из проверок CI упала
- `cancelled` — запуск CI был отменён

### 🛠 Использование

Добавьте шаг в ваш CI workflow (например, `.github/workflows/ci.yml`). Обязательно укажите `if: always()`, чтобы уведомление отправлялось и при падении проверок:

```yaml
name: CI Checks

on:
  push:
    branches: [main]
  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      - name: Run tests
        run: npm test

      - name: Notify CI status in Telegram
        if: always()
        uses: Val-d-emar/telegram-ci-notifications@v1
        with:
          bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
          job_status: ${{ job.status }}
          # message_thread_id: "12345" # Опционально: ID топика в форумах
          user_map: ${{ secrets.TELEGRAM_USER_MAP }}
          ignore_bots: "true"
          notify_on_success: "true"
          language: "ru"
```

### ⚙️ Параметры (Inputs)

| Параметр            | Обязательный | По умолчанию | Описание                                                   |
| ------------------- | ------------ | ------------ | ---------------------------------------------------------- |
| `bot_token`         | **Да**       |              | Токен вашего Telegram бота.                                |
| `chat_id`           | **Да**       |              | ID чата (группа, канал или ЛС).                            |
| `job_status`        | **Да**       |              | Статус текущей job (обычно `${{ job.status }}`).           |
| `message_thread_id` | Нет          |              | ID топика (ветки), если группа в режиме форума.            |
| `user_map`          | Нет          |              | Маппинг никнеймов GitHub -> Telegram.                      |
| `ignore_bots`       | Нет          | `true`       | Не отправлять уведомления, если действие совершил бот.     |
| `notify_on_success` | Нет          | `true`       | Отправлять ли уведомление при успехе (`true` или `false`). |
| `language`          | Нет          | `ru`         | Язык уведомлений (`en` или `ru`).                          |

### 👥 Маппинг пользователей (`user_map`)

Чтобы бот тегал нужных людей в Telegram, создайте секрет в GitHub (например, `TELEGRAM_USER_MAP`) в следующем формате:

```text
github_username1=telegram_username1
github_username2=@telegram_username2
```

_Примечание: Символ `@` перед ником в Telegram писать не обязательно, скрипт добавит его автоматически._

### Лицензия

Этот проект распространяется под лицензией [MIT License](LICENSE).
