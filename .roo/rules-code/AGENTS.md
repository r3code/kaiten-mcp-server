# Code Mode Rules - Kaiten MCP Server

## MCP I/O Protocol

- **stdout**: Только JSON-RPC ответы, никаких console.log
- **stderr**: Все логи через `safeLog.info/warn/error/debug` из `config.ts`
- Никогда не используйте `console.log` - это нарушает MCP протокол

## Логирование

- Используйте `safeLog.info/warn/error/debug` вместо console
- Все методы автоматически маскируют токены через `redactSecrets()`
- Логи всегда пишутся в stderr для совместимости с MCP

## Verbosity Control

- Поддерживаются уровни: `minimal`, `normal` (default), `detailed`
- Используйте `applyCardVerbosity()`, `applyUserVerbosity()`, `applyBoardVerbosity()` из `utils.ts`
- `truncateResponse()` ограничивает ответы до 100k символов (~25k токенов)

## Idempotency Keys

- Авто-генерация для: `createCard`, `updateCard`, `createComment`, `updateComment`
- Формат: `mcp-${timestamp}-${randomHex}`
- Передавайте через заголовок `Idempotency-Key`

## AbortSignal Support

- Передавайте `signal?: AbortSignal` во все async методы клиента
- Проверяйте `signal?.aborted` перед выполнением запроса
- Используйте `queuedRequest()` для поддержки отмены через p-queue

## Zod Validation

- Все входы инструментов валидируются через схемы в `schemas.ts`
- Используйте `Schema.parse(args)` в handler'ах
- Обрабатывайте `z.ZodError` с форматированием ошибок

## Helper Functions Pattern

- Используйте `simplifyUser()`, `simplifySpace()`, `simplifyCard()`, `simplifyComment()` из `src/index.ts`
- Функции уменьшают размер ответа на 92-96% (удаляют base64 аватары, права доступа, UI метаданные)
- `simplifyCard()` добавляет человеко-читаемые поля: `board_title`, `column_title`, `owner_name`, `members`
- Применяйте упрощение для всех list/get операций перед возвратом данных клиенту
