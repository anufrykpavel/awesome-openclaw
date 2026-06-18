# Memory & Context

Как OpenClaw помнит информацию между сессиями.

## Core Files

| Файл | Назначение | Когда обновлять |
|------|------------|-----------------|
| `MEMORY.md` | Долгосрочная память | Важные события, решения, уроки |
| `memory/YYYY-MM-DD.md` | Ежедневные заметки | Каждый день |
| `AGENTS.md` | Конфигурация агента | При смене настроек |
| `SOUL.md` | Личность агента | При изменении характера |
| `USER.md` | Информация о пользователе | При смене контекста |
| `TOOLS.md` | Локальные заметки | При добавлении инструментов |

## Memory Workflow

### 1. Daily Notes

```bash
# Автоматически создаётся при первом обращении
memory/2026-06-18.md
```

**Что писать:**
- Что делали сегодня
- Какие решения приняли
- Что узнали
- Ошибки и уроки

### 2. Memory Search

```bash
# Поиск по памяти
memory_search query="project deadline"

# Поиск по сессиям
memory_search query="telegram setup" corpus=sessions
```

### 3. MEMORY.md — Curated Memory

Раз в несколько дней просматривай daily notes и переноси важное в `MEMORY.md`:

```markdown
## Project Alpha
- Started: 2026-06-15
- Stack: OpenClaw + Telegram + GitHub
- Key decisions:
  - Use subagents for file processing
  - Heartbeat for daily checks

## Lessons Learned
- Cron лучше heartbeat для точного времени
- Всегда использовать taskName для subagents
```

## Memory Search vs Get

| Команда | Когда использовать |
|---------|-------------------|
| `memory_search` | Не помнишь точно, где искать |
| `memory_get` | Знаешь путь, нужен конкретный фрагмент |

## Best Practices

### ✓ Делай так

- Пиши concrete updates, не placeholder
- Используй memory_search перед вопросами о прошлом
- Цитируй источник: `Source: MEMORY.md#line 42`

### ✗ Не делай так

- Не пиши "запомнить позже" — пиши сразу
- Не храни секреты в memory без необходимости
- Не полагайся на "mental notes"

## Example: Daily Workflow

```markdown
# memory/2026-06-18.md

## Что сделали
- Настроили GitHub CLI авторизацию
- Создали репозиторий awesome-openclaw
- Добавили начальную структуру (README, skills, integrations)

## Уроки
- Для git push нужен PAT с scope `repo`, не только `gh` токен
- GitHub CLI и git используют разные токены

## Завтра
- Дописать раздел про memory
- Добавить примеры subagents
```

## Tools

| Tool | Описание |
|------|----------|
| `memory_search` | Семантический поиск по памяти |
| `memory_get` | Чтение конкретного файла/фрагмента |

## Resources

- [AGENTS.md](/AGENTS.md) — стандартный шаблон
- [SOUL.md](/SOUL.md) — определение личности
