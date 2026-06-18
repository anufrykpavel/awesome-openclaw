# Cron Jobs

Запланированные задачи в OpenClaw.

## Schedule Types

### At — одноразово

```json
{
  "kind": "at",
  "at": "2026-06-19T09:00:00Z"
}
```

### Every — интервал

```json
{
  "kind": "every",
  "everyMs": 3600000
}
```

### Cron — по расписанию

```json
{
  "kind": "cron",
  "expr": "0 9 * * *",
  "tz": "Europe/Moscow"
}
```

## Payload Types

### System Event

```json
{
  "kind": "systemEvent",
  "text": "Daily reminder"
}
```

### Agent Turn

```json
{
  "kind": "agentTurn",
  "message": "Check email and calendar"
}
```

## Examples

### Morning Check

```json
{
  "name": "morning-check",
  "schedule": {"kind": "cron", "expr": "0 9 * * *", "tz": "Europe/Moscow"},
  "payload": {"kind": "systemEvent", "text": "Check: email, calendar, weather"}
}
```

### Reminder

```json
{
  "name": "reminder",
  "schedule": {"kind": "at", "at": "2026-06-19T15:00:00Z"},
  "payload": {"kind": "systemEvent", "text": "Call reminder"}
}
```

## Heartbeat vs Cron

| Задача | Что использовать | Почему |
|--------|-----------------|--------|
| Точное время | Cron | 9:00 sharp |
| Группа проверок | Heartbeat | Email + calendar + weather |
| Изолированная задача | Cron | Другая модель/уровень thinking |
| Быстрый reminder | Cron | "Напомни через 20 мин" |
