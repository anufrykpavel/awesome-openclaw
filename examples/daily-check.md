# Daily Check Script

Автоматическая проверка email, calendar, weather через heartbeat.

## HEARTBEAT.md

```markdown
## Daily Tasks

### Email
- Check inbox for urgent messages
- Reply if needed

### Calendar
- Events in next 24h
- Remind 2h before

### Weather
- Today's forecast
- Tomorrow's forecast
- Alert if rain

## State

```json
{
  "lastChecks": {
    "email": 1703275200,
    "calendar": 1703275200,
    "weather": 1703275200
  }
}
```
```

## Usage

1. Сохранить как `HEARTBEAT.md`
2. Heartbeat сработает автоматически
3. Агент выполнит проверки

## Output

```
✅ Email: 0 urgent
📅 Calendar: 1 event tomorrow at 14:00
🌧️ Weather: Rain expected — bring umbrella
```
