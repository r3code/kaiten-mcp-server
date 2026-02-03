# Debug Mode Rules - Kaiten MCP Server

## MCP Inspector для тестирования

- Используйте `npm run inspector` для интерактивного тестирования инструментов
- Inspector показывает все доступные инструменты и их параметры
- Полезно для проверки схем валидации и ответов API

## Logging System

- RFC 5424 уровни: `debug`, `info`, `notice`, `warning`, `error`, `critical`, `alert`, `emergency`
- Все логи пишутся в stderr для совместимости с MCP протоколом
- `safeLog` автоматически маскирует токены через `redactSecrets()`
- Структурированное логирование с pino для парсинга и анализа

## Metrics Collector

- Включается через `KAITEN_LOG_METRICS=true` или `kaiten_set_log_level`
- Собирает статистику по запросам, времени выполнения, ошибкам
- Доступен через `kaiten_get_status` → `metrics` секция
- Полезен для анализа производительности и выявления проблем

## Runtime Log Level Configuration

- Измените уровень логирования на лету через `kaiten_set_log_level` инструмент
- Включайте/выключайте MCP логи, файловые логи, метрики отдельно
- `KAITEN_LOG_REQUESTS` включает полное логирование HTTP запросов/ответов
- Конфигурация доступна через `kaiten_get_status` → `logging` секция
