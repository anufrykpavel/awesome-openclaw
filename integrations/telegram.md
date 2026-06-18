# Telegram Integration

OpenClaw нативно работает с Telegram: rich text, кнопки, медиа, реакции.

## Настройка

```yaml
# gateway config
agents:
  defaults:
    messaging:
      telegram:
        enabled: true
        token: "${TELEGRAM_BOT_TOKEN}"
```

## Rich Text

OpenClaw поддерживает Bot API 10.1:

### Headings

```markdown
# Заголовок 1
## Заголовок 2
```

### Collapsible Blocks

```html
<details>
<summary>Показать детали</summary>
Скрытый контент здесь
</details>
```

### Tables

```markdown
| Команда | Описание |
|---------|----------|
| /status | Статус |
| /plan   | План |
```

### Code Blocks

```markdown
```bash
gh auth login
```
```

### Highlighting

```markdown
Важно: <mark>это важно</mark>
```

### Spoilers

```markdown||скрытый текст||
```

## Media

```markdown
MEDIA:/path/to/image.png
```

## Inline Buttons

```markdown
[[button:Нажми меня|callback:data]]
```

## Best Practices

### Форматирование

- Используй **жирный** для выделения
- `код` для команд
- Списки вместо таблиц (если сложно)

### Discord-совместимость

```markdown
<-- Wrap links to suppress embeds
<https://example.com>
```

### Emoji

- Используй естественно
- Максимум 1 реакция на сообщение
- React когда: подтверждение, юмор, интерес

## Example

```markdown
## Статус ✅

**Проект:** awesome-openclaw
**Коммит:** 6c5d108

<details>
<summary>Что нового</summary>

- Добавлен раздел Memory & Context
- Обновлён README

</details>

MEDIA:./screenshot.png
```

## Resources

- [Telegram Bot API](https://core.telegram.org/bots/api)
- [OpenClaw Docs](https://docs.openclaw.ai)
