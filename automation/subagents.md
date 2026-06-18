# Subagents

Делегирование задач в изолированные сессии.

## Создание Subagent

```bash
sessions_spawn task="Analyze logs" taskName=log-analyzer
```

## Параметры

| Параметр | Описание | Значения |
|----------|----------|----------|
| `task` | Задача для subagent | Описание работы |
| `taskName` | Стабильное имя | log-analyzer, doc-writer |
| `context` | Контекст | isolated (по умолч.), fork |
| `mode` | Режим | run (one-shot), chat |
| `runtime` | Тип | subagent (по умолч.) |

## Context Types

### Isolated (рекомендуется)

```bash
sessions_spawn task="Process files" taskName=processor
```

Чистая сессия без истории. Быстрее, дешевле.

### Fork

```bash
sessions_spawn task="Analyze this" context="fork" taskName=analyzer
```

Subagent видит текущий транскрипт. Полезно для контекста.

## Best Practices

### Используй taskName

```bash
# Плохо
sessions_spawn task="Analyze logs"

# Хорошо
sessions_spawn task="Analyze logs" taskName=log-analyzer
```

Позволяет обращаться к subagent по имени.

### Не полль в цикле

```bash
# Плохо
while true; do subagents list; done

# Хорошо
sessions_yield
```

Используй `sessions_yield` для ожидания completion events.

### Batch задачи

```bash
# Плохо — много мелких subagents
for file in *.txt; do sessions_spawn task="Process $file"; done

# Хорошо — один subagent обрабатывает всё
sessions_spawn task="Process all files in /data" taskName=batch-processor
```

## Monitoring

```bash
# Список active subagents
subagents list

# История сессий
sessions_list --active-minutes 60

# Отправить сообщение subagent
sessions_send taskName=log-analyzer message="Status?"
```

## Use Cases

| Сценарий | Подход |
|----------|--------|
| Batch file processing | Один subagent, обработка всех файлов |
| Параллельный анализ | Несколько subagents, разные задачи |
| Long-running task | background mode + cron check |
| Interactive помощник | sessions_spawn с mode=chat |
