# Telegram Bot для мониторинга торговли токенами

## Обзор
Этот Python-скрипт реализует Telegram-бота, который отслеживает торговые операции с токенами в блокчейне TON с использованием API `geckoterminal.com` и `tonapi.io`. Бот отправляет уведомления о сделках в реальном времени в указанную Telegram-группу, включая детали такие как объем транзакции, адрес кошелька, рыночная капитализация и количество держателей. Также поддерживаются inline-кнопки для взаимодействия с пользователями.

## Функциональность
- Мониторинг торговых операций для указанного пула в блокчейне TON
- Отправка форматированных уведомлений о сделках в Telegram-группу с деталями:
  - Тип транзакции (покупка/продажа)
  - Значение в TON и USD
  - Количество токенов
  - Адрес кошелька (сокращенный, со ссылкой на `tonviewer.com`)
  - Хэш транзакции (со ссылкой на `tonviewer.com`)
  - Текущая цена и рыночная капитализация
  - Количество держателей и статус нового держателя
- Включает inline-кнопки для покупки токена, вступления в чат или приглашения друзей
- Фильтрация транзакций ниже указанного значения (опционально, по умолчанию: 10 TON)
- Логирование активности и ошибок для отладки
- Работает непрерывно с настраиваемой частотой запросов

## Требования
- Python 3.11+
- Необходимые библиотеки:
  - `requests`
  - `pyTelegramBotAPI` (telebot)
  - `threading`
  - `logging`
  - `time`

Установите необходимые библиотеки:
```bash
pip install -r requirements.txt
```

## Конфигурация
Перед запуском скрипта настройте следующие параметры в коде:
- `BOT_TOKEN`: Токен вашего Telegram-бота (полученный от BotFather)
- `GROUP_CHAT_ID`: ID чата Telegram-группы (без кавычек)
- `TOKEN_NAME`: Название отслеживаемого токена
- `TOKEN_ADDRESS`: Адрес токена в блокчейне TON
- `POOL_ID`: ID пула с `geckoterminal.com`
- `BASE_URL`: Базовый URL API для `geckoterminal.com` (по умолчанию: `https://api.geckoterminal.com/api/v2`)
- `IS_CHECKING`: Включение/отключение фильтрации транзакций ниже `CHECKING_VALUE` (True/False)
- `CHECKING_VALUE`: Минимальное значение TON для отчетности о транзакциях (по умолчанию: 10)
- `TIMEOUT`: Частота запросов в секундах (минимум: 1 секунда)
- URL-адреса для inline-кнопок:
  - `button1`: URL для кнопки "КУПИТЬ $RRMONEY"
  - `button2`: URL для кнопки "НАШ ЧАТ"
  - `button3`: URL для кнопки "ПРИГЛАСИТЬ ДРУЗЕЙ"

## Использование
1. Настройте параметры как описано выше
2. Запустите скрипт:
```bash
python bot.py
```

Бот начнет мониторинг торговых операций и отправку уведомлений в указанную Telegram-группу.

## Выходные данные
Бот отправляет сообщения в Telegram-группу в формате Markdown, включая:
- Название токена со ссылкой на пул на `geckoterminal.com`
- Детали транзакции (значение в TON/USD, количество токенов, кошелек, ссылка на транзакцию)
- Цену, рыночную капитализацию, количество держателей и статус нового держателя
- Inline-кнопки для взаимодействия с пользователем

### Пример вывода
```
[НАЗВАНИЕ_ТОКЕНА](https://www.geckoterminal.com/ru/ton/pools/POOL_ID) ПОКУПКА
TON: 15.23 ($50.45)
Токены: 1,000 НАЗВАНИЕ_ТОКЕНА
Кошелек: [abcd...wxyz](https://tonviewer.com/wallet_address) | [ТРАНЗАКЦИЯ](https://tonviewer.com/transaction/tx_hash)
Цена: $0.05
Рыночная капитализация: $1,234,567
Держатели: 5,678
Новый держатель: Да
```

## Обработка ошибок
- Логирование сетевых ошибок при запросах к API
- Обработка неверных ответов API
- Перехват и логирование ошибок polling Telegram
- Пропуск транзакций ниже `CHECKING_VALUE`, если `IS_CHECKING` включен

## Примечания
- Требуется активное интернет-соединение для доступа к API `geckoterminal.com` и `tonapi.io`
- `TIMEOUT` не должен быть установлен ниже 1 секунды во избежание ограничений API
- Убедитесь, что `BOT_TOKEN`, `GROUP_CHAT_ID`, `TOKEN_ADDRESS` и `POOL_ID` действительны
- Бот работает в отдельном потоке для мониторинга торговли одновременно с polling сообщений Telegram

## Лицензия
Проект распространяется под MIT License.  
---
# Telegram Bot for Token Trade Monitoring

## Overview
This Python script implements a Telegram bot that monitors token trades on the TON blockchain using the `geckoterminal.com` and `tonapi.io` APIs. The bot sends real-time trade notifications to a specified Telegram group, including details like transaction volume, wallet address, market cap, and holder count. It also supports inline buttons for user interaction.

## Features
- Monitors token trades for a specified pool on the TON blockchain.
- Sends formatted trade notifications to a Telegram group with details such as:
  - Transaction type (buy/sell)
  - TON and USD value
  - Token amount
  - Wallet address (shortened, with a link to `tonviewer.com`)
  - Transaction hash (with a link to `tonviewer.com`)
  - Current price and market cap
  - Holder count and new holder status
- Includes inline buttons for buying the token, joining a chat, or inviting friends.
- Filters transactions below a specified value (optional, default: 10 TON).
- Logs activities and errors for debugging.
- Runs continuously with configurable request frequency.

## Requirements
- Python 3.11+
- Required libraries:
  - `requests`
  - `pyTelegramBotAPI` (telebot)
  - `threading`
  - `logging`
  - `time`

Install the required libraries:
```bash
pip install -r requirements.txt
```

## Configuration
Before running the script, configure the following settings in the code:
- `BOT_TOKEN`: Your Telegram bot token (obtained from BotFather).
- `GROUP_CHAT_ID`: The Telegram group chat ID (without quotes).
- `TOKEN_NAME`: The name of the token being monitored.
- `TOKEN_ADDRESS`: The token's address on the TON blockchain.
- `POOL_ID`: The pool ID from `geckoterminal.com`.
- `BASE_URL`: The base API URL for `geckoterminal.com` (default: `https://api.geckoterminal.com/api/v2`).
- `IS_CHECKING`: Enable/disable filtering of transactions below `CHECKING_VALUE` (True/False).
- `CHECKING_VALUE`: Minimum TON value for transactions to be reported (default: 10).
- `TIMEOUT`: Request frequency in seconds (minimum: 1 second).
- Inline button URLs:
  - `button1`: URL for "BUY $RRMONEY" button.
  - `button2`: URL for "OUR CHAT" button.
  - `button3`: URL for "INVITE FRIENDS" button.

## Usage
1. Configure the settings as described above.
2. Run the script:
```bash
python bot.py
```

The bot will start monitoring trades and sending notifications to the specified Telegram group.

## Output
The bot sends messages to the Telegram group in Markdown format, including:
- Token name with a link to the pool on `geckoterminal.com`.
- Transaction details (TON/USD value, token amount, wallet, transaction link).
- Price, market cap, holder count, and new holder status.
- Inline buttons for user interaction.

### Example Output
```
[TOKEN_NAME](https://www.geckoterminal.com/ru/ton/pools/POOL_ID) BUY
TON: 15.23 ($50.45)
Token: 1,000 TOKEN_NAME
Wallet: [abcd...wxyz](https://tonviewer.com/wallet_address) | [TXN](https://tonviewer.com/transaction/tx_hash)
Price: $0.05
Market Cap: $1,234,567
Holders: 5,678
New Holder: True
```

## Error Handling
- Logs network errors during API requests.
- Handles invalid API responses.
- Catches and logs Telegram polling errors.
- Skips transactions below the `CHECKING_VALUE` if `IS_CHECKING` is enabled.

## Notes
- Requires an active internet connection to access `geckoterminal.com` and `tonapi.io` APIs.
- The `TIMEOUT` should not be set below 1 second to avoid API rate limits.
- Ensure the `BOT_TOKEN`, `GROUP_CHAT_ID`, `TOKEN_ADDRESS`, and `POOL_ID` are valid.
- The bot runs in a separate thread for trade monitoring while polling Telegram messages.

## License
The project extends to MIT License.
