# Kaiten MCP Server - Правила проекта

## Основные принципы

- **ES Modules + strict mode**: Все `.ts` файлы используют ES imports, `"use strict"` неявно
- **MCP I/O Protocol**: stdout = только JSON-RPC, stderr = логи (никогда не использовать `console.log`)
- **safeLog wrapper**: Используйте `safeLog.info/warn/error/debug` из `config.ts` вместо console
- **Idempotency keys**: Авто-генерация для мутаций (`createCard`, `updateCard`, `createComment`, `updateComment`)
- **AbortSignal support**: Передавайте `signal` через все async методы клиента для отмены
- **Zod strict validation**: Все входы инструментов валидируются через схемы в `schemas.ts`

## Конфигурация

- Переменные окружения валидируются через Zod схему в `config.ts`
- Обязательно: `KAITEN_API_URL` (должен заканчиваться на `/api/latest`), `KAITEN_API_TOKEN` (мин 20 символов)
- Опционально: `KAITEN_DEFAULT_SPACE_ID`, `KAITEN_MAX_CONCURRENT_REQUESTS` (1-20), `KAITEN_CACHE_TTL_SECONDS`
- Логирование: `KAITEN_LOG_LEVEL`, `KAITEN_LOG_MCP_ENABLED`, `KAITEN_LOG_FILE_ENABLED`, `KAITEN_LOG_METRICS`

## Зависимости

- **axios-retry**: 3 повтора, exponential backoff + jitter, учитывает заголовок Retry-After
- **p-queue**: Контроль параллельности (по умолчанию: 5 запросов), лимит 1s
- **lru-cache**: TTL-based кэш (по умолчанию: 300s), без сброса при чтении
- **pino**: RFC 5424 уровни логирования, структурированное логирование с маскировкой
- **zod**: Runtime валидация типов в strict mode

## Особенности Kaiten API

- **Русский поиск**: Используйте только корневые слова (Болгарии/Болгария → "болгар", валюты/валютный → "валют")
- **Имена пользователей**: ТОЛЬКО LATIN - ищите "Saranyuk", а не "Саранюк"
- **Space/Board API**: `/boards` требует space_id, используйте `/spaces/{id}/boards`
- **Users endpoint**: Максимум 100 пользователей за запрос, используйте query для фильтрации
