# Awesome OpenClaw 🦾

> Curated list of OpenClaw best practices, skills, integrations and tricks

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

## Contents

- [Getting Started](#getting-started)
- [Core Concepts](#core-concepts)
- [Skills](#skills)
- [Integrations](#integrations)
- [Automation](#automation)
- [Subagents](#subagents)
- [Memory & Context](#memory--context)
- [Tips & Tricks](#tips--tricks)
- [Resources](#resources)

---

## Getting Started

OpenClaw — это платформа для запуска AI-агентов с доступом к инструментам, памяти и внешним интеграциям.

### Installation

```bash
# Install via npm
npm install -g openclaw

# Or use npx
npx openclaw
```

### First Run

```bash
openclaw init
openclaw start
```

---

## Core Concepts

### Session Types

- **Main session** — основной чат с агентом
- **Subagents** — изолированные сессии для делегирования задач
- **Cron jobs** — запланированные задачи

### Memory System

- **MEMORY.md** — долгосрочная память агента
- **memory/YYYY-MM-DD.md** — ежедневные заметки
- **AGENTS.md** — конфигурация агента
- **SOUL.md** — личность агента
- **USER.md** — информация о пользователе
- **TOOLS.md** — локальные заметки об инструментах

---

## Skills

### Available Skills

| Skill | Description | Use Case |
|-------|-------------|----------|
| [browser-automation](./skills/browser-automation.md) | Web page control, login flows, tab management | Scraping, automation |
| [github](./skills/github.md) | GitHub CLI for issues, PRs, CI logs | Dev workflows |
| [notion](./skills/notion.md) | Notion API for pages, content, databases | Knowledge management |
| [weather](./skills/weather.md) | Weather and forecasts | Travel planning |
| [diagram-maker](./skills/diagram-maker.md) | SVG/HTML/Excalidraw diagrams | Documentation |
| [meme-maker](./skills/meme-maker.md) | Meme generation | Fun |
| [node-inspect-debugger](./skills/node-inspect-debugger.md) | Node.js debugging | Development |
| [healthcheck](./skills/healthcheck.md) | Security auditing | Sysadmin |

### Creating Custom Skills

```bash
skill_workshop action=create name=my-skill description="My custom skill"
```

---

## Integrations

### Telegram

```json
{
  "channel": "telegram",
  "provider": "telegram"
}
```

### GitHub CLI

```bash
# Auth
export GH_TOKEN="ghp_xxx"
gh auth login

# Usage
gh repo list
gh issue list
```

### Cron Jobs

```json
{
  "name": "daily-check",
  "schedule": {"kind": "cron", "expr": "0 9 * * *"},
  "payload": {"kind": "systemEvent", "text": "Morning check"}
}
```

---

## Automation

### Heartbeat Checks

Use `HEARTBEAT.md` для регулярных проверок:

```markdown
# Check email
# Check calendar
# Check weather
```

### Cron vs Heartbeat

| Use Case | Tool | Reason |
|----------|------|--------|
| Exact timing | Cron | 9:00 AM sharp |
| Batch checks | Heartbeat | Email + calendar together |
| Isolated task | Cron | Different model/thinking |
| Quick reminder | Cron | "Remind me in 20 min" |

---

## Subagents

### Spawning

```bash
sessions_spawn task="Analyze logs" taskName=log-analyzer
```

### Best Practices

- Используй `context="fork"` только когда нужен текущий транскрипт
- Обычно `context="isolated"` — чище и дешевле
- Используй `taskName` для стабильного обращения

---

## Memory & Context

### Memory Search

```bash
memory_search query="project deadline"
```

### Daily Notes

```bash
# Create daily note
echo "## $(date +%Y-%m-%d)" >> memory/$(date +%Y-%m-%d).md
```

---

## Tips & Tricks

### Security

- Никогда не коммить токены в репозиторий
- Используй `~/.bashrc` для переменных окружения
- Проверяй `GH_TOKEN` перед использованием

### Efficiency

- Batch external API calls
- Используй `sessions_yield` вместо поллинга
- Читай SKILL.md перед использованием нового skill

---

## Resources

- [OpenClaw Docs](https://docs.openclaw.ai)
- [GitHub Repository](https://github.com/openclaw/openclaw)
- [Discord Community](https://discord.gg/openclaw)

---

## Contributing

PR welcome! Формат:

```markdown
## Tip Name

**Category:** integration/automation/tip

**Description:** Краткое описание

**Example:**
```code
# пример кода
```
```

---

*Made with 🤖 by [anufrykpavel](https://github.com/anufrykpavel)*
