# GitHub Skill

Интеграция с GitHub CLI для работы с репозиториями, issues, PR и CI.

## Setup

```bash
# Установка GH_TOKEN
export GH_TOKEN="ghp_xxx"

# Авторизация через браузер
gh auth login
```

## Common Commands

```bash
# Список репозиториев
gh repo list --json name,visibility,updatedAt

# Создать репозиторий
gh repo create my-repo --public --add-readme

# Issues
gh issue list
gh issue create --title "Bug" --body "Description"

# PR
gh pr create --title "Feature" --body "Changes"
gh pr merge

# CI/CD
gh run list
gh run view <run-id>
```

## Tips

- Используй `--json` для программного вывода
- `gh api` для кастомных запросов к API
- Токен храни в `~/.bashrc`, не коммить в репо
