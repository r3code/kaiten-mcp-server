# Ask Mode Rules - Kaiten MCP Server

## Russian Search Quirks

- Kaiten API использует только корневые слова для русского поиска
- Используйте корень: "болгар" вместо "Болгарии/Болгария/болгарский"
- Используйте корень: "валют" вместо "валюты/валютный"
- Это критично для поиска - точные формы могут не давать результатов

## LATIN-only User Names

- Kaiten хранит имена пользователей ТОЛЬКО в латинице
- Ищите "Saranyuk", а не "Саранюк"
- Транслитерация: Владимир → Vladimir/Vlad, Алексей → Alex/Aleksey
- Используйте `kaiten_list_users(query="latin_name")` для поиска

## Space/Board API Limitations

- `/boards` endpoint требует `space_id` параметр
- Используйте `/spaces/{id}/boards` для получения досок пространства
- `/boards` без space_id не поддерживается (API limitation)
- Сначала получите space_id через `kaiten_list_spaces`

## Response Truncation

- Ответы ограничены 100k символов (~25k токенов)
- Используйте `verbosity: 'minimal'` для больших списков
- Добавляйте фильтры: `board_id`, `space_id`, `column_id`
- Уменьшайте `limit` параметр для избежания обрезки
