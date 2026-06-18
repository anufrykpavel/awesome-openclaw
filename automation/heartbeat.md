# Heartbeat

Регулярные проверки через `HEARTBEAT.md` — batch-обработка задач без точного расписания.

## Как работает

Когда OpenClaw получает heartbeat-сигнал, читает `HEARTBEAT.md` и выполняет задачи оттуда.

```
HEARTBEAT.md → Агент видит задачи → Выполняет → Отвечает HEARTBEAT_OK или результат
```

## Пустой файл = Skip

```markdown
<!-- Heartbeat template; comments-only content prevents scheduled heartbeat API calls. -->

# Keep this file empty (or with only comments) to skip heartbeat API calls.

# Add tasks below when you want the agent to check something periodically.
```

## Пример с задачами

```markdown
# Heartbeat Checklist

## Email
- Проверить важные письма
- Ответить на срочные

## Calendar
- События в ближайшие 24 часа
- Напомнить за 2 часа

## Weather
- Погода на завтра
- Если дождь — предупредить

## GitHub
- Новые issues
- Новые PR
- CI статус

## Memory
- Обновить MEMORY.md из daily notes
- Удалить устаревшее
```

## Heartbeat vs Cron

| Задача | Инструмент | Почему |
|--------|-----------|--------|
| Точное время (9:00 AM) | Cron | Heartbeat дрейфует |
| Группа проверок | Heartbeat | Batch, меньше API calls |
| Изолированная задача | Cron | Другой model/thinking |
| «Напомни через 20 мин» | Cron | One-shot, точное время |
| Раз в ~30 минут | Heartbeat | Не критичен дрейф |

## Best Practices

### Batch всё вместе

```markdown
# Хорошо — всё в одном heartbeat
- Email + Calendar + Weather + GitHub

# Плохо — разные cron для каждого
```

### Track checks в JSON

```json
// memory/heartbeat-state.json
{
  "lastChecks": {
    "email": 1703275200,
    "calendar": 1703260800,
    "weather": null
  }
}
```

### Когда молчать

- Ночь (23:00-08:00)
- Ничего нового с прошлого раза
- Ты занят

### Когда говорить

- Важное email
- Событие через <2 часа
- Что-то интересное нашёл
- Прошло >8 часов молчания

## Против proactive работы

**Можешь делать без спроса:**
- Читать/организовать memory
- Проверять git status
- Обновлять документацию
- Коммитить и пушить свои изменения
- Обновлять MEMORY.md

**Спроси перед:**
- Отправкой письма/сообщения
- Публикацией в публичный доступ
- Любым действием "наружу"

## Example: Full HEARTBEAT.md

```markdown
<!-- Morning Check — 9:00 AM -->

## Daily Tasks

### 1. Email
- [ ] Проверить inbox
- [ ] Срочные — ответить
- [ ] Остальные — отметить

### 2. Calendar
- [ ] События сегодня
- [ ] События завтра
- [ ] Напоминания за 2 часа

### 3. GitHub
- [ ] Новые issues
- [ ] Новые PR
- [ ] Review requests

### 4. Weather
- [ ] Погода на сегодня
- [ ] Погода на завтра
- [ ] Если дождь — предупредить

### 5. Memory
- [ ] Просмотреть вчерашние notes
- [ ] Обновить MEMORY.md
- [ ] Удалить устаревшее

## State Tracking

```json
{
  "lastChecks": {
    "email": 1703275200,
    "calendar": 1703275200,
    "github": 1703275200,
    "weather": 1703275200,
    "memory": 1703275200
  }
}
```

## Output Format

- Если всё ок: `HEARTBEAT_OK`
- Если нашёл что-то: краткое сообщение
- Не спамить, если ничего нового
```

## Integration с Cron

```bash
# Cron на 9:00 AM — триггер heartbeat
cron: {
  "name": "morning-trigger",
  "schedule": {"kind": "cron", "expr": "0 9 * * *", "tz": "Europe/Moscow"},
  "payload": {"kind": "systemEvent", "text": "Morning heartbeat"}
}

# HEARTBEAT.md содержит список проверок
```

## Resources

- [Cron](./cron.md) — для точного расписания
- [Memory](./context.md) — для отслеживания состояния
