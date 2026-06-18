# Security

Best practices для работы с токенами, секретами и доступом в OpenClaw.

## Токены

### GitHub Token

**Типы:**
- **GitHub CLI token** (`gh_*`) — для `gh` команд
- **PAT** (`ghp_*`) — для git operations

**Настройка:**

```bash
# В ~/.bashrc (не коммить!)
export GH_TOKEN="ghp_***"

# Или keyring (рекомендуется)
gh auth login  # сохраняет в системный keyring
```

**Проверка:**

```bash
gh auth status
```

### Telegram Bot Token

```yaml
# gateway config
agents:
  defaults:
    messaging:
      telegram:
        token: "${TELEGRAM_BOT_TOKEN}"
```

**Environment variable:**

```bash
export TELEGRAM_BOT_TOKEN="***"
```

## Keyring

Рекомендуется использовать системный keyring вместо env vars:

```bash
# GitHub CLI сохраняет туда автоматически
gh auth login

# Проверить
gh auth status
# Token: gho_************************************ (keyring)
```

## .gitignore

```gitignore
# Tokens
*.token
*.key
.env
.env.local

# Configs with secrets
config.yaml
config.yml
settings.json

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
```

## Best Practices

### ✅ Делай так

- Храни токены в `~/.bashrc` или keyring
- Используй разные токены для разных целей
- Регулярно ротируй токены (90 дней)
- Проверяй `git status` перед коммитом
- Используй `export` вместо hardcode

### ❌ Не делай так

```bash
# Плохо — hardcode
gh auth login --with-token <<< "ghp_abc123"

# Плохо — в коде
curl -H "Authorization: token ghp_abc123" ...

# Плохо — в README
export TOKEN="ghp_abc123"
```

### ✅ Делай так

```bash
# Хорошо — env var
export GH_TOKEN="${GH_TOKEN}"
gh auth login

# Хорошо — keyring
gh auth login  # интерактивно
```

## Проверка перед коммитом

```bash
# Проверить, что не коммитишь секреты
git diff --cached | grep -E "(ghp_|gho_|token|secret|password)"

# Или используй gitleaks
gitleaks detect --source .
```

## Leak Response

Если токен попал в git:

1. **Отозвать токен немедленно**
   - GitHub → Settings → Developer settings → Personal access tokens → Delete

2. **Удалить из истории git**
   ```bash
   git filter-branch --force --index-filter \
     'git rm --cached --ignore-unmatch файл-с-токеном' \
     --prune-empty --tag-name-filter cat -- --all
   ```

3. **Force push**
   ```bash
   git push origin --force --all
   ```

4. **Создать новый токен**

## Scope минимум

Давай токенам минимально необходимые права:

| Задача | Scope |
|--------|-------|
| Чтение репо | `repo:read` или `public_repo` |
| Push в репо | `repo` |
| Issues/PR | `repo` |
| Actions | `repo`, `workflow` |
| Admin | `repo`, `admin:repo_hook` |

## Resources

- [GitHub Token docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
- [gitleaks](https://github.com/gitleaks/gitleaks) — сканер секретов
- [git-secrets](https://github.com/awslabs/git-secrets) — pre-commit hook
