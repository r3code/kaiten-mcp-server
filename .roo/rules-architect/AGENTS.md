# Architect Mode Rules - Kaiten MCP Server

## Concurrency Control (p-queue)

- `KAITEN_MAX_CONCURRENT_REQUESTS` ограничивает параллельные запросы (1-20)
- Интервальный лимит: максимум N запросов за 1 секунду
- Используйте `queuedRequest()` для всех API вызовов
- Поддерживает AbortSignal для отмены ожидающих запросов

## Retry Logic

- 3 повтора с exponential backoff + jitter (0-500ms)
- Учитывает заголовок `Retry-After` при rate limiting
- Повторяет на: 429, 5xx, 408, network errors
- Не повторяет на: 4xx (кроме 429, 408), валидационные ошибки

## LRU Cache Configuration

- TTL-based кэш по умолчанию: 300 секунд
- Кэш НЕ сбрасывается при чтении (только по TTL или инвалидации)
- Инвалидация: `kaiten_cache_invalidate_*` инструменты
- Кэшируются: spaces, boards, users (для оптимизации)

## KaitenError Categorization

- `AUTH_ERROR`: 401/403 - проблемы с токеном или правами
- `NOT_FOUND`: 404 - ресурс не найден
- `VALIDATION_ERROR`: 422 - неверные параметры запроса
- `RATE_LIMITED`: 429 - превышен лимит запросов
- `TIMEOUT`: ECONNABORTED/ETIMEDOUT - таймаут запроса
- `NETWORK_ERROR`: нет ответа от сервера
- `API_ERROR`: 5xx - ошибки сервера Kaiten
