# Troubleshooting

Типичные ошибки и решения при работе с OpenClaw.

## GitHub

### "Invalid username or token"

**Причина:** Используешь GitHub CLI token для git operations.

**Решение:**
1. Создай PAT (Personal Access Token) с scope `repo`
2. Используй его для git push: `https://token@github.com/...`
3. Или настрой SSH ключи

### "ssh_askpass: exec(/usr/bin/ssh-askpass): No such file"

**Причина:** Нет SSH key или SSH agent.

**Решение:**
```bash
# HTTPS с токеном проще
git remote set-url origin https://TOKEN@github.com/user/repo.git
```

## Subagents

### Subagent не завершается

**Причина:** Ждёшь в цикле.

**Решение:**
```bash
# Плохо
while true; do subagents list; done

# Хорошо
sessions_yield  # жди completion events
```

### Не могу обратиться к subagent

**Причина:** Не задан taskName.

**Решение:**
```bash
# Плохо
sessions_spawn task="Analyze"

# Хорошо
sessions_spawn task="Analyze" taskName=log-analyzer
```

## Cron

### Cron job не срабатывает

**Проверь:**
1. Правильный формат expr: `0 9 * * *` (не `9:00`)
2. Часовой пояс: `"tz": "Europe/Moscow"`
3. sessionTarget для payload kind

## Memory

### Memory search не находит

**Причина:** Ищешь в сессиях, а нужно в memory.

**Решение:**
```bash
# Поиск в памяти
memory_search query="project"

# Поиск в сессиях
memory_search query="project" corpus=sessions
```

## Telegram

### Нет rich text

**Причина:** Используешь старый формат.

**Решение:** OpenClaw поддерживает Bot API 10.1:
```markdown
# Heading
<details><summary>Spoiler</summary>Content</details>
<mark>highlight</mark>
```

## General

### "No active session found"

**Причина:** Процесс уже завершился или неверный sessionId.

**Решение:** Проверь `process list` или запусти заново.

### Команда не работает в sandbox

**Ошибка:** `exec host=sandbox requires a sandbox runtime`

**Решение:**
```bash
# Используй host=auto или host=gateway
exec host=gateway command="..."
```

## Resources

- [OpenClaw Docs](https://docs.openclaw.ai)
- [GitHub CLI Troubleshooting](https://cli.github.com/manual/troubleshooting)
