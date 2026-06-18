# GitHub Automation

Создание репозитория и начальная настройка.

## Скрипт

```bash
# 1. Auth
export GH_TOKEN="***"
gh auth login

# 2. Create repo
gh repo create awesome-project \
  --public \
  --description "My awesome project" \
  --add-readme

# 3. Clone
cd ~/workspace
gh repo clone username/awesome-project

# 4. Initial structure
cd awesome-project
mkdir -p src docs tests
touch README.md LICENSE .gitignore

# 5. Commit
git add .
git commit -m "Initial commit"
git push origin main
```

## Настройка для push

```bash
# HTTPS with token
git remote set-url origin https://TOKEN@github.com/user/repo.git

# Or SSH (recommended for long-term)
git remote set-url origin git@github.com:user/repo.git
```

## Проверка

```bash
gh repo view --json name,description,url
```
