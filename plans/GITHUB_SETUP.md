# 🔧 Настройка Git и синхронизация с GitHub

## 📋 Пошаговая инструкция

### Шаг 1: Создать `.gitignore` файл

```bash
cd /Users/tikhomirovevg/Direct_helper_bot
```

Создайте файл `.gitignore` со следующим содержимым:

```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
env/
ENV/
.venv
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Конфигурация и секреты (ВАЖНО!)
config/config.yaml
config/google_credentials.json
config/*.json
.env
*.encrypted
secrets/

# Данные и кэш
data/cache/
data/*.db
data/direct_helper.db
*.db
*.sqlite
*.sqlite3

# Логи
logs/
*.log

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Celery
celerybeat-schedule
celerybeat.pid

# Pytest
.pytest_cache/
.coverage
htmlcov/
.tox/
.nox/

# Документация build
docs/_build/
site/

# Jupyter Notebook
.ipynb_checkpoints

# pyenv
.python-version

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre
.pyre/
```

### Шаг 2: Инициализировать Git репозиторий

```bash
# Убедитесь что находитесь в корне проекта
cd /Users/tikhomirovevg/Direct_helper_bot

# Инициализация Git
git init

# Проверка статуса
git status
```

Вы должны увидеть список файлов для коммита.

### Шаг 3: Создать README.md (если его нет)

```bash
cat > README.md << 'EOF'
# 🤖 Direct Helper Bot

Автоматизированный Telegram-бот для создания рекламных кампаний Яндекс.Директ.

## 🚀 Функционал

- 📊 Сбор семантики из Yandex Wordstat API
- 🔄 Кластеризация ключевых слов с помощью ML
- ✍️ Генерация текстов объявлений через GPT-4
- 📤 Экспорт в Google Sheets для импорта в Директ

## 📋 Требования

- Python 3.10+
- Telegram Bot Token
- Yandex Direct API OAuth Token
- OpenAI API Key
- Google Service Account credentials

## 🛠️ Установка

1. Клонировать репозиторий:
```bash
git clone https://github.com/YOUR_USERNAME/Direct_helper_bot.git
cd Direct_helper_bot
```

2. Создать виртуальное окружение:
```bash
python3 -m venv venv
source venv/bin/activate  # macOS/Linux
# или
venv\Scripts\activate  # Windows
```

3. Установить зависимости:
```bash
pip install -r requirements.txt
```

4. Настроить переменные окружения:
```bash
cp .env.example .env
# Отредактировать .env и заполнить API ключи
```

5. Запустить бота:
```bash
python main.py
```

## 📖 Документация

Подробная документация находится в папке [`plans/`](plans/):
- [Implementation Roadmap](plans/IMPLEMENTATION_ROADMAP.md) - План реализации
- [Project Analysis](plans/PROJECT_ANALYSIS.md) - Анализ проекта
- [Architecture](direct-helper-architecture.md) - Общая архитектура

## 🔑 Получение API ключей

См. [IMPLEMENTATION_ROADMAP.md - ЭТАП 2](plans/IMPLEMENTATION_ROADMAP.md#-этап-2-получение-api-ключей-и-credentials)

## 📊 Текущий статус

⚠️ **Проект в разработке**

- ✅ Архитектурная документация
- ✅ План реализации
- 🚧 Реализация кода (в процессе)

## 📝 Лицензия

MIT

## 👤 Автор

Ваше имя / @your_telegram

## 🤝 Contributing

Pull requests приветствуются!
EOF
```

### Шаг 4: Создать первый коммит

```bash
# Добавить все файлы (gitignore автоматически исключит ненужное)
git add .

# Проверить что будет закоммичено
git status

# Создать первый коммит
git commit -m "Initial commit: Project documentation and architecture"
```

### Шаг 5: Создать репозиторий на GitHub

**Вариант A: Через веб-интерфейс (рекомендуется)**

1. Откройте https://github.com/new
2. Заполните форму:
   - **Repository name:** `Direct_helper_bot`
   - **Description:** "Telegram bot для автоматизации рекламных кампаний Яндекс.Директ"
   - **Visibility:** Private (рекомендуется) или Public
   - ❌ **НЕ** ставьте галочки "Add README", "Add .gitignore" (у нас уже есть)
3. Нажмите "Create repository"
4. **НЕ закрывайте страницу** - понадобятся команды для push

**Вариант B: Через GitHub CLI (если установлен)**

```bash
gh repo create Direct_helper_bot --private --source=. --remote=origin
```

### Шаг 6: Подключить удаленный репозиторий

После создания репозитория на GitHub, выполните:

```bash
# Добавить remote origin (замените YOUR_USERNAME на ваш GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/Direct_helper_bot.git

# Проверить что remote добавлен
git remote -v
```

Вы должны увидеть:
```
origin  https://github.com/YOUR_USERNAME/Direct_helper_bot.git (fetch)
origin  https://github.com/YOUR_USERNAME/Direct_helper_bot.git (push)
```

### Шаг 7: Создать и переключиться на main ветку

```bash
# Переименовать master в main (современная практика)
git branch -M main
```

### Шаг 8: Push на GitHub

```bash
# Первый push с установкой upstream
git push -u origin main
```

**Если требуется аутентификация:**

1. **GitHub Personal Access Token (рекомендуется):**
   - Перейдите на https://github.com/settings/tokens
   - "Generate new token" → "Generate new token (classic)"
   - Отметьте `repo` scope
   - Сгенерируйте и скопируйте токен
   - При push используйте токен вместо пароля

2. **SSH ключ (альтернатива):**
   ```bash
   # Генерация SSH ключа (если нет)
   ssh-keygen -t ed25519 -C "your_email@example.com"
   
   # Добавить в ssh-agent
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   
   # Скопировать публичный ключ
   cat ~/.ssh/id_ed25519.pub
   
   # Добавить на GitHub: Settings → SSH keys → New SSH key
   
   # Изменить remote на SSH
   git remote set-url origin git@github.com:YOUR_USERNAME/Direct_helper_bot.git
   
   # Push
   git push -u origin main
   ```

### Шаг 9: Проверка

Откройте https://github.com/YOUR_USERNAME/Direct_helper_bot

Вы должны увидеть:
- ✅ Файлы документации (`*.md`)
- ✅ Папку `plans/`
- ✅ README.md отображается на главной странице
- ❌ НЕТ файлов `.env`, `config/config.yaml`, `*.db` (защищены `.gitignore`)

---

## 🔄 Дальнейшая работа с Git

### Ежедневный workflow:

```bash
# 1. Проверить статус
git status

# 2. Добавить измененные файлы
git add .
# или конкретные файлы
git add src/bot/app.py src/core/pipeline.py

# 3. Закоммитить
git commit -m "Add feature: Telegram bot base structure"

# 4. Push на GitHub
git push
```

### Хорошие практики коммитов:

```bash
# Понятные сообщения
git commit -m "feat: Add Yandex Wordstat API client"
git commit -m "fix: Fix rate limiting bug in API client"
git commit -m "docs: Update installation instructions"
git commit -m "refactor: Improve keyword clustering algorithm"
git commit -m "test: Add unit tests for ad generator"
```

**Префиксы:**
- `feat:` - новый функционал
- `fix:` - исправление бага
- `docs:` - документация
- `refactor:` - рефакторинг без изменения функционала
- `test:` - тесты
- `chore:` - техническая работа (зависимости и т.д.)

### Работа с ветками (для больших фич):

```bash
# Создать новую ветку
git checkout -b feature/telegram-bot-handlers

# Работаем, коммитим...
git add .
git commit -m "feat: Add campaign creation handler"

# Push ветки на GitHub
git push -u origin feature/telegram-bot-handlers

# На GitHub: создать Pull Request

# После мержа: переключиться обратно на main
git checkout main
git pull origin main

# Удалить локальную ветку
git branch -d feature/telegram-bot-handlers
```

---

## ⚠️ Важные предупреждения

### 🚨 НИКОГДА не коммитьте:

```bash
# Эти файлы должны быть в .gitignore
.env                           # Переменные окружения
config/config.yaml             # Конфигурация с токенами
config/google_credentials.json # Google API credentials
data/*.db                      # Базы данных
logs/*.log                     # Логи
*.sqlite                       # SQLite файлы
```

### 🔐 Если случайно закоммитили секреты:

```bash
# НЕМЕДЛЕННО:
# 1. Удалить из истории Git (сложно)
# 2. Сменить все токены/ключи на новые
# 3. Добавить файл в .gitignore
# 4. Закоммитить изменения

# Лучше предотвратить, чем лечить!
```

### Проверка перед коммитом:

```bash
# Всегда проверяйте что коммитите
git diff

# Проверьте список файлов
git status

# Если что-то не так - уберите из staging
git reset HEAD file_to_remove.txt
```

---

## 📝 Примеры команд для вашего проекта

### Первоначальная настройка (выполнить один раз):

```bash
cd /Users/tikhomirovevg/Direct_helper_bot

# 1. Создать .gitignore (скопируйте содержимое выше)
nano .gitignore  # или любой другой редактор

# 2. Инициализировать Git
git init
git add .
git commit -m "Initial commit: Project documentation and architecture"

# 3. Создать репозиторий на GitHub (через веб-интерфейс)
# https://github.com/new

# 4. Подключить remote (замените YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/Direct_helper_bot.git
git branch -M main
git push -u origin main
```

### После добавления кода:

```bash
# Когда создадите src/, requirements.txt, main.py и т.д.
git add src/ requirements.txt main.py .env.example
git commit -m "feat: Add project structure and base files"
git push
```

---

## 🎯 Следующие шаги после push на GitHub

1. ✅ **Настроить GitHub Actions** (CI/CD) - опционально
2. ✅ **Добавить Shields badges** в README (статус build, версия)
3. ✅ **Создать GitHub Projects** для управления задачами
4. ✅ **Включить GitHub Issues** для трекинга багов
5. ✅ **Написать CONTRIBUTING.md** если планируете открыть проект

---

## 🆘 Troubleshooting

### Проблема: "Permission denied (publickey)"

**Решение:** Используйте HTTPS вместо SSH или настройте SSH ключи (см. Шаг 8)

### Проблема: "fatal: remote origin already exists"

**Решение:**
```bash
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/Direct_helper_bot.git
```

### Проблема: "Updates were rejected"

**Решение:**
```bash
# Сначала получите изменения с GitHub
git pull origin main --rebase
# Потом push
git push
```

### Проблема: Случайно закоммитил большой файл

**Решение:**
```bash
# Отменить последний коммит (НЕ push!)
git reset HEAD~1

# Добавить файл в .gitignore
echo "large_file.db" >> .gitignore

# Заново добавить файлы
git add .
git commit -m "Add files (excluding large file)"
```

---

## 📚 Полезные ресурсы

- [Git Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Conventional Commits](https://www.conventionalcommits.org/)

---

## ✅ Checklist

После выполнения всех шагов, проверьте:

- [ ] Git репозиторий инициализирован
- [ ] `.gitignore` создан и настроен
- [ ] Первый коммит создан
- [ ] Репозиторий создан на GitHub
- [ ] Remote origin подключен
- [ ] Код загружен на GitHub (`git push`)
- [ ] Проверил что секреты НЕ загружены (.env, config.yaml)
- [ ] README.md отображается корректно

**Готово! Ваш проект теперь на GitHub! 🎉**
