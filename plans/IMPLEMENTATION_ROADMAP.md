# 🚀 Direct Helper Bot - Пошаговый план реализации

## 📊 Текущее состояние проекта

### ✅ Что уже есть:
- ✅ Архитектурная документация ([`direct-helper-architecture.md`](../direct-helper-architecture.md))
- ✅ План интеграции с Google Sheets ([`google-sheets-integration.md`](../google-sheets-integration.md))
- ✅ Руководство по Yandex Wordstat API ([`wordstat-api-guide.md`](../wordstat-api-guide.md))
- ✅ Telegram Bot архитектура ([`telegram-bot-architecture.md`](../telegram-bot-architecture.md))
- ✅ Сервисная архитектура v2 ([`service-architecture-v2.md`](../service-architecture-v2.md))

### ❌ Что отсутствует:
- ❌ Реальный код проекта (нет папок `src`, `config`, `tests`)
- ❌ Файлы конфигурации (`requirements.txt`, `config.yaml`, `.env`)
- ❌ Главный файл приложения (`main.py`)
- ❌ Docker-конфигурация
- ❌ База данных и ORM модели
- ❌ Telegram Bot реализация
- ❌ Core модули (semantic, clustering, generation, export)
- ❌ API клиенты (Yandex, OpenAI, Google Sheets)

---

## 🎯 ЭТАП 1: Настройка базовой инфраструктуры проекта

### Цель: Создать структуру проекта и настроить окружение разработки

### Задачи:

#### 1.1. Инициализация Git репозитория
```bash
cd /Users/tikhomirovevg/Direct_helper_bot
git init
git add .
git commit -m "Initial commit with architecture documentation"
```

#### 1.2. Создание структуры директорий
```bash
# Создаем основные директории
mkdir -p src/{bot,core,api,models,database,tasks,services,utils}
mkdir -p src/bot/{handlers,middlewares,keyboards,states,utils,filters}
mkdir -p src/core/{semantic,clustering,generation,export}
mkdir -p src/database/{models,repositories,migrations}
mkdir -p config data/{cache,templates} tests/{unit,integration} logs plans
```

#### 1.3. Создание файла `.gitignore`
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

# Конфигурация и секреты
config/config.yaml
config/google_credentials.json
config/*.json
.env
*.encrypted

# Данные и кэш
data/cache/
data/*.db
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

# OS
.DS_Store
Thumbs.db

# Celery
celerybeat-schedule
celerybeat.pid

# Pytest
.pytest_cache/
.coverage
htmlcov/

# Документация build
docs/_build/
```

#### 1.4. Создание `requirements.txt`
```txt
# Telegram Bot
python-telegram-bot==20.7

# Yandex Direct API
requests==2.31.0

# OpenAI / LLM
openai==1.6.1

# Google Sheets
google-auth==2.25.2
google-auth-oauthlib==1.2.0
google-auth-httplib2==0.2.0
google-api-python-client==2.110.0
gspread==5.12.3

# Data processing
pandas==2.1.4
openpyxl==3.1.2

# ML & NLP
scikit-learn==1.3.2
sentence-transformers==2.2.2

# Database
SQLAlchemy==2.0.23
alembic==1.13.1
psycopg2-binary==2.9.9  # PostgreSQL
# или для SQLite (уже включен в Python)

# Celery & Redis
celery==5.3.4
redis==5.0.1

# Configuration & Environment
pyyaml==6.0.1
python-dotenv==1.0.0

# Security
cryptography==41.0.7

# Logging & Monitoring
sentry-sdk==1.39.1

# Testing
pytest==7.4.3
pytest-asyncio==0.21.1
pytest-cov==4.1.0

# Development tools
black==23.12.1
flake8==7.0.0
mypy==1.7.1
```

#### 1.5. Создание виртуального окружения
```bash
python3 -m venv venv
source venv/bin/activate  # macOS/Linux
# или
venv\Scripts\activate  # Windows

pip install --upgrade pip
pip install -r requirements.txt
```

#### 1.6. Создание `.env.example`
```bash
# Telegram
TELEGRAM_BOT_TOKEN=your_bot_token_here

# Yandex Direct API
YANDEX_CLIENT_ID=your_client_id
YANDEX_CLIENT_SECRET=your_client_secret
YANDEX_OAUTH_TOKEN=your_oauth_token

# OpenAI
OPENAI_API_KEY=your_openai_key_here

# Google Sheets
GOOGLE_CREDENTIALS_PATH=config/google_credentials.json

# Database
DATABASE_URL=sqlite:///data/direct_helper.db
# DATABASE_URL=postgresql://user:password@localhost:5432/direct_helper

# Redis & Celery
REDIS_URL=redis://localhost:6379/0
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/0

# Security
ENCRYPTION_KEY=generate_with_fernet_key
SECRET_KEY=random_secret_key_here

# Logging
LOG_LEVEL=INFO
SENTRY_DSN=your_sentry_dsn_if_used

# Admin
ADMIN_TELEGRAM_IDS=123456789,987654321
```

#### 1.7. Создание `README.md`
```markdown
# 🤖 Direct Helper Bot

Автоматизированный Telegram-бот для создания рекламных кампаний Яндекс.Директ.

## 🚀 Функционал

- 📊 Сбор семантики из Yandex Wordstat API
- 🔄 Кластеризация ключевых слов с помощью ML
- ✍️ Генерация текстов объявлений через GPT-4
- 📤 Экспорт в Google Sheets

## 📋 Установка

1. Клонировать репозиторий
2. Создать виртуальное окружение: `python3 -m venv venv`
3. Активировать: `source venv/bin/activate`
4. Установить зависимости: `pip install -r requirements.txt`
5. Скопировать `.env.example` в `.env` и заполнить credentials
6. Запустить: `python main.py`

## 📖 Документация

См. папку `plans/` для подробной документации.
```

---

## 🔑 ЭТАП 2: Получение API-ключей и credentials

### Цель: Получить все необходимые API ключи для работы сервиса

### 2.1. Telegram Bot Token

**Шаги:**
1. Открыть Telegram и найти [@BotFather](https://t.me/BotFather)
2. Отправить команду `/newbot`
3. Ввести название бота: `Direct Helper Bot`
4. Ввести username: `direct_helper_your_suffix_bot`
5. Получить токен и сохранить в `.env`:
   ```
   TELEGRAM_BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
   ```

### 2.2. Yandex Direct API OAuth Token

**Шаги:**
1. Зарегистрировать приложение на https://oauth.yandex.ru/
   - Название: Direct Helper
   - Платформы: Веб-сервисы
   - Redirect URI: `http://localhost:8080/callback`
   - Права: `Яндекс.Директ` → `Управление рекламными кампаниями`
   
2. Получить Client ID и Client Secret

3. Получить OAuth токен (см. [`wordstat-api-guide.md`](../wordstat-api-guide.md))

4. Сохранить в `.env`:
   ```
   YANDEX_CLIENT_ID=your_client_id
   YANDEX_CLIENT_SECRET=your_secret
   YANDEX_OAUTH_TOKEN=your_token
   ```

### 2.3. OpenAI API Key

**Шаги:**
1. Зарегистрироваться на https://platform.openai.com/
2. Перейти в API Keys
3. Создать новый ключ
4. Сохранить в `.env`:
   ```
   OPENAI_API_KEY=sk-proj-...
   ```

### 2.4. Google Sheets API Credentials

**Шаги:**
1. Создать проект в Google Cloud Console
2. Включить Google Sheets API и Google Drive API
3. Создать Service Account
4. Скачать JSON credentials
5. Сохранить как `config/google_credentials.json`

Подробная инструкция в [`google-sheets-integration.md`](../google-sheets-integration.md)

### 2.5. Encryption Key для безопасности

**Генерация:**
```python
from cryptography.fernet import Fernet
key = Fernet.generate_key()
print(key.decode())
```

Сохранить в `.env`:
```
ENCRYPTION_KEY=generated_key_here
```

---

## 🏗️ ЭТАП 3: Создание базовой структуры приложения

### Цель: Создать базовые файлы конфигурации и точку входа

### 3.1. Создание `src/__init__.py`
```python
"""
Direct Helper Bot - автоматизация рекламных кампаний Яндекс.Директ
"""

__version__ = "0.1.0"
__author__ = "Your Name"
```

### 3.2. Создание `src/utils/config.py`
```python
"""Управление конфигурацией приложения"""
import os
from pathlib import Path
from typing import Any, Optional
import yaml
from dotenv import load_dotenv

# Загружаем переменные окружения
load_dotenv()

class Config:
    """Класс конфигурации приложения"""
    
    def __init__(self, config_path: Optional[str] = None):
        self.config_path = config_path or os.getenv('CONFIG_PATH', 'config/config.yaml')
        self._config = self._load_config()
    
    def _load_config(self) -> dict:
        """Загрузить конфигурацию из YAML файла"""
        config_file = Path(self.config_path)
        
        if config_file.exists():
            with open(config_file, 'r', encoding='utf-8') as f:
                return yaml.safe_load(f) or {}
        return {}
    
    def get(self, key: str, default: Any = None) -> Any:
        """
        Получить значение конфигурации
        Поддерживает вложенные ключи через точку: 'telegram.bot_token'
        Приоритет: переменные окружения > config.yaml > default
        """
        # Пробуем получить из переменных окружения
        env_key = key.upper().replace('.', '_')
        env_value = os.getenv(env_key)
        if env_value is not None:
            return env_value
        
        # Пробуем получить из config.yaml
        keys = key.split('.')
        value = self._config
        
        for k in keys:
            if isinstance(value, dict) and k in value:
                value = value[k]
            else:
                return default
        
        return value if value is not None else default
    
    # Удобные shortcuts для частых параметров
    @property
    def telegram_token(self) -> str:
        return self.get('TELEGRAM_BOT_TOKEN', '')
    
    @property
    def yandex_token(self) -> str:
        return self.get('YANDEX_OAUTH_TOKEN', '')
    
    @property
    def openai_key(self) -> str:
        return self.get('OPENAI_API_KEY', '')
    
    @property
    def database_url(self) -> str:
        return self.get('DATABASE_URL', 'sqlite:///data/direct_helper.db')
    
    @property
    def redis_url(self) -> str:
        return self.get('REDIS_URL', 'redis://localhost:6379/0')

# Глобальный экземпляр конфигурации
_config = None

def get_config() -> Config:
    """Получить глобальный экземпляр конфигурации"""
    global _config
    if _config is None:
        _config = Config()
    return _config
```

### 3.3. Создание `src/utils/logger.py`
```python
"""Настройка логирования"""
import logging
import sys
from pathlib import Path
from datetime import datetime
from typing import Optional

def setup_logger(
    name: str = "direct_helper",
    level: str = "INFO",
    log_file: Optional[str] = None
) -> logging.Logger:
    """
    Настроить логгер для приложения
    
    Args:
        name: Имя логгера
        level: Уровень логирования (DEBUG, INFO, WARNING, ERROR)
        log_file: Путь к файлу логов (опционально)
    
    Returns:
        Настроенный logger
    """
    logger = logging.getLogger(name)
    logger.setLevel(getattr(logging, level.upper()))
    
    # Форматирование
    formatter = logging.Formatter(
        '%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        datefmt='%Y-%m-%d %H:%M:%S'
    )
    
    # Консольный handler
    console_handler = logging.StreamHandler(sys.stdout)
    console_handler.setLevel(logging.INFO)
    console_handler.setFormatter(formatter)
    logger.addHandler(console_handler)
    
    # Файловый handler
    if log_file:
        # Создаем директорию для логов если нужно
        log_path = Path(log_file)
        log_path.parent.mkdir(parents=True, exist_ok=True)
        
        file_handler = logging.FileHandler(log_file, encoding='utf-8')
        file_handler.setLevel(logging.DEBUG)
        file_handler.setFormatter(formatter)
        logger.addHandler(file_handler)
    else:
        # По умолчанию создаем файл с датой
        log_dir = Path('logs')
        log_dir.mkdir(exist_ok=True)
        log_file = log_dir / f"direct_helper_{datetime.now():%Y%m%d}.log"
        
        file_handler = logging.FileHandler(log_file, encoding='utf-8')
        file_handler.setLevel(logging.DEBUG)
        file_handler.setFormatter(formatter)
        logger.addHandler(file_handler)
    
    return logger

# Глобальный логгер
logger = setup_logger()
```

### 3.4. Создание `config/config.example.yaml`
```yaml
# Пример конфигурации Direct Helper Bot
# Скопируйте в config.yaml и заполните своими значениями

telegram:
  bot_token: "${TELEGRAM_BOT_TOKEN}"
  admin_ids:
    - 123456789  # Ваш Telegram ID

yandex:
  client_id: "${YANDEX_CLIENT_ID}"
  client_secret: "${YANDEX_CLIENT_SECRET}"
  oauth_token: "${YANDEX_OAUTH_TOKEN}"
  rate_limit:
    max_requests: 10
    time_window: 1  # секунды

openai:
  api_key: "${OPENAI_API_KEY}"
  model: "gpt-4-turbo"
  temperature: 0.7
  max_tokens: 200

google_sheets:
  credentials_path: "config/google_credentials.json"
  auto_share: true
  default_permission: "writer"

database:
  url: "${DATABASE_URL}"
  echo: false  # SQL логи
  pool_size: 5
  max_overflow: 10

celery:
  broker_url: "${CELERY_BROKER_URL}"
  result_backend: "${CELERY_RESULT_BACKEND}"
  task_time_limit: 3600  # 1 час
  task_soft_time_limit: 3000  # 50 минут

cache:
  enabled: true
  ttl_days: 7
  directory: "data/cache"

logging:
  level: "INFO"
  file: "logs/direct_helper.log"

security:
  encryption_key: "${ENCRYPTION_KEY}"
  secret_key: "${SECRET_KEY}"

features:
  max_campaigns_per_user: 10
  max_keywords_per_campaign: 500
  ads_per_cluster: 2
```

### 3.5. Создание главного файла `main.py`
```python
"""
Главный файл приложения Direct Helper Bot
"""
import asyncio
import sys
from pathlib import Path

# Добавляем src в путь
sys.path.insert(0, str(Path(__file__).parent / 'src'))

from src.utils.logger import setup_logger
from src.utils.config import get_config

logger = setup_logger()

async def main():
    """Главная функция приложения"""
    logger.info("🚀 Запуск Direct Helper Bot...")
    
    # Загружаем конфигурацию
    config = get_config()
    
    # Проверяем наличие обязательных параметров
    if not config.telegram_token:
        logger.error("❌ Отсутствует TELEGRAM_BOT_TOKEN")
        logger.error("Создайте файл .env и заполните необходимые параметры")
        return
    
    logger.info("✅ Конфигурация загружена")
    logger.info(f"📊 Database: {config.database_url}")
    logger.info(f"📱 Telegram Bot готов к запуску")
    
    # TODO: Инициализация компонентов
    # TODO: Запуск Telegram бота
    
    logger.info("⏸️  Telegram Bot реализация в процессе...")
    logger.info("📋 Следующие шаги:")
    logger.info("   1. Реализовать core модули")
    logger.info("   2. Создать Telegram bot handlers")
    logger.info("   3. Настроить базу данных")
    logger.info("   4. Интегрировать Celery для фоновых задач")

if __name__ == "__main__":
    try:
        asyncio.run(main())
    except KeyboardInterrupt:
        logger.info("👋 Остановка приложения...")
    except Exception as e:
        logger.error(f"❌ Критическая ошибка: {e}", exc_info=True)
        sys.exit(1)
```

---

## 💻 ЭТАП 4: Реализация core-модулей бизнес-логики

### Цель: Создать основные модули для работы с семантикой, кластеризацией и генерацией

### 4.1. Yandex Wordstat API Client

**Файл:** `src/api/yandex_client.py`

**Что реализовать:**
- Класс `YandexWordstatClient` с методами:
  - `create_report(phrases, geo_ids)` - создание отчета
  - `get_report(report_id)` - получение данных
  - `delete_report(report_id)` - удаление отчета
- Rate limiting (10 req/sec)
- Retry logic с экспоненциальной задержкой
- Обработка ошибок API

**Пример кода:** См. [`wordstat-api-guide.md`](../wordstat-api-guide.md), раздел "Шаг 2"

### 4.2. Keyword Collector (сбор семантики)

**Файл:** `src/core/semantic/keyword_collector.py`

**Что реализовать:**
- Класс `KeywordCollector`:
  - Сбор базовых ключевых слов
  - Расширение связанными запросами
  - Фильтрация по частоте
  - Дедупликация
- Класс `Keyword` (dataclass) для хранения данных
- Кэширование результатов

### 4.3. Keyword Clusterer (кластеризация)

**Файл:** `src/core/clustering/keyword_clusterer.py`

**Что реализовать:**
- Класс `KeywordClusterer`:
  - ML-кластеризация (K-means или DBSCAN)
  - Векторизация с помощью TF-IDF или sentence-transformers
  - Автоматическое определение количества кластеров
  - Именование кластеров
- Метрики качества кластеризации

### 4.4. Ad Generator (генерация объявлений)

**Файл:** `src/core/generation/ad_generator.py`

**Что реализовать:**
- Класс `AdGenerator`:
  - Интеграция с OpenAI GPT-4
  - Промпты для генерации текстов
  - Валидация на соответствие требованиям Яндекс.Директ:
    - Заголовок: макс 35 символов
    - Текст: макс 81 символ
  - Генерация нескольких вариантов
- Класс `AdValidator` для проверки требований

### 4.5. Google Sheets Exporter

**Файл:** `src/core/export/google_sheets_exporter.py`

**Что реализовать:**
- Класс `GoogleSheetsClient`:
  - Аутентификация через Service Account
  - Создание таблиц
  - Запись данных
  - Форматирование (заголовки, ширина столбцов)
  - Предоставление доступа по email
- Класс `DirectSheetFormatter`:
  - Форматирование данных кампании в формат Яндекс.Директ
  - Структура: Campaign | AdGroup | Keyword | AdTitle | AdText | URL

**Пример кода:** См. [`google-sheets-integration.md`](../google-sheets-integration.md)

### 4.6. Campaign Pipeline (оркестратор)

**Файл:** `src/core/pipeline.py`

**Что реализовать:**
- Класс `CampaignPipeline`:
  - Метод `create_campaign()` - полный цикл создания
  - Этапы:
    1. Сбор семантики
    2. Кластеризация
    3. Генерация объявлений
    4. Экспорт в Google Sheets
  - Progress callbacks для уведомлений
  - Обработка ошибок на каждом этапе
  - Сохранение промежуточных результатов

---

## 🤖 ЭТАП 5: Реализация Telegram бота

### Цель: Создать интерфейс пользователя через Telegram

### 5.1. Базовая структура бота

**Файл:** `src/bot/app.py`

**Что реализовать:**
- Инициализация `Application`
- Регистрация handlers
- Регистрация middlewares
- Обработка ошибок
- Graceful shutdown

### 5.2. Command Handlers

**Файлы:**
- `src/bot/handlers/start.py` - `/start`, `/help`
- `src/bot/handlers/campaign.py` - создание кампаний
- `src/bot/handlers/history.py` - история кампаний
- `src/bot/handlers/settings.py` - настройки пользователя
- `src/bot/handlers/admin.py` - админ-панель

### 5.3. Conversation Flow (FSM)

**Файл:** `src/bot/states/campaign.py`

**Состояния:**
1. `ENTER_NAME` - ввод названия кампании
2. `ENTER_PRODUCT` - описание продукта
3. `ENTER_KEYWORDS` - базовые ключевые слова
4. `SELECT_REGION` - выбор региона
5. `ENTER_URL` - целевой URL
6. `ENTER_BUDGET` - бюджет (опционально)
7. `CONFIRMATION` - подтверждение
8. `PROCESSING` - обработка (запуск Celery task)
9. `COMPLETED` - завершено

### 5.4. Keyboards

**Файлы:**
- `src/bot/keyboards/inline.py` - Inline клавиатуры
- `src/bot/keyboards/reply.py` - Reply клавиатуры

**Основные клавиатуры:**
- Главное меню
- Выбор региона
- Подтверждение кампании
- Действия с кампанией

### 5.5. Middlewares

**Файлы:**
- `src/bot/middlewares/auth.py` - проверка пользователя
- `src/bot/middlewares/rate_limit.py` - ограничение запросов
- `src/bot/middlewares/logging.py` - логирование действий

---

## 💾 ЭТАП 6: Интеграция с базой данных

### Цель: Хранение данных пользователей и кампаний

### 6.1. ORM Models

**Файлы:**
- `src/database/models/user.py` - модель пользователя
- `src/database/models/campaign.py` - модель кампании
- `src/database/models/task.py` - модель задачи

**Что реализовать:**
- SQLAlchemy ORM модели
- Relationships между моделями
- Индексы для производительности

### 6.2. Repositories

**Файлы:**
- `src/database/repositories/user.py`
- `src/database/repositories/campaign.py`
- `src/database/repositories/task.py`

**Что реализовать:**
- CRUD операции
- Бизнес-логика запросов
- Транзакции

### 6.3. Database Connection

**Файл:** `src/database/connection.py`

**Что реализовать:**
- Инициализация SQLAlchemy engine
- Session management
- Connection pooling

### 6.4. Alembic Migrations

**Команды:**
```bash
alembic init src/database/migrations
alembic revision --autogenerate -m "Initial migration"
alembic upgrade head
```

---

## ⚙️ ЭТАП 7: Асинхронная обработка (Celery)

### Цель: Фоновая обработка создания кампаний

### 7.1. Celery App

**Файл:** `src/tasks/celery_app.py`

**Что реализовать:**
- Инициализация Celery
- Конфигурация broker (Redis)
- Конфигурация result backend

### 7.2. Campaign Creation Task

**Файл:** `src/tasks/campaign_tasks.py`

**Что реализовать:**
- Task `create_campaign_task`:
  - Получение данных из БД
  - Запуск pipeline
  - Обновление progress в Redis
  - Отправка уведомлений в Telegram
  - Сохранение результата в БД
- Error handling и retry logic

### 7.3. Progress Notifications

**Файл:** `src/bot/utils/notifications.py`

**Что реализовать:**
- Функция `send_progress_update()`:
  - Обновление сообщения с прогресс-баром
  - Этапы: Wordstat (0-25%), Clustering (25-50%), Generation (50-75%), Export (75-100%)

### 7.4. Celery Worker запуск

**Команды:**
```bash
# Запуск worker
celery -A src.tasks.celery_app worker --loglevel=info

# Запуск beat (для периодических задач)
celery -A src.tasks.celery_app beat --loglevel=info
```

---

## 🧪 ЭТАП 8: Тестирование и отладка

### Цель: Обеспечить качество кода

### 8.1. Unit Tests

**Что тестировать:**
- Core модули (semantic, clustering, generation)
- API клиенты (mock responses)
- Validators
- Formatters

### 8.2. Integration Tests

**Что тестировать:**
- Полный pipeline создания кампании
- Интеграция с реальными API (с test credentials)
- Database операции

### 8.3. Bot Tests

**Что тестировать:**
- Handlers
- Conversation flow
- Keyboards

### 8.4. Запуск тестов

```bash
# Все тесты
pytest

# С покрытием
pytest --cov=src --cov-report=html

# Конкретный модуль
pytest tests/unit/test_keyword_collector.py
```

---

## 📚 ЭТАП 9: Документация и деплой

### Цель: Подготовить к production

### 9.1. Docker Configuration

**Файл:** `Dockerfile`

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "main.py"]
```

**Файл:** `docker-compose.yml`

```yaml
version: '3.8'

services:
  bot:
    build: .
    env_file: .env
    volumes:
      - ./data:/app/data
      - ./logs:/app/logs
    depends_on:
      - redis
      - postgres
  
  celery_worker:
    build: .
    command: celery -A src.tasks.celery_app worker --loglevel=info
    env_file: .env
    depends_on:
      - redis
      - postgres
  
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
  
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: direct_helper
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### 9.2. Production Checklist

- [ ] Все API ключи в переменных окружения
- [ ] Логирование настроено (Sentry)
- [ ] Database backup strategy
- [ ] Monitoring (health checks)
- [ ] Rate limiting настроен
- [ ] Error handling везде
- [ ] Documentation актуальная

### 9.3. Deployment

```bash
# Local development
python main.py

# Docker
docker-compose up -d

# Production (example: VPS)
git pull
docker-compose -f docker-compose.prod.yml up -d --build
```

---

## 🎯 ЭТАП 10: Production-готовность и мониторинг

### Цель: Обеспечить стабильную работу

### 10.1. Logging

- Настроить уровни логирования
- Ротация логов
- Интеграция с Sentry для ошибок

### 10.2. Monitoring

- Health check endpoints
- Метрики Celery (задачи, очередь)
- Метрики бота (пользователи, кампании)

### 10.3. Backup

- Регулярный backup БД
- Backup конфигурации
- Backup кэша (опционально)

### 10.4. Security

- Шифрование токенов в БД
- HTTPS для всех API запросов
- Rate limiting для пользователей
- Валидация всех входных данных

---

## 📊 Приоритеты реализации

### 🔴 Критически важно (MVP):
1. ✅ ЭТАП 1 - Базовая инфраструктура
2. ✅ ЭТАП 2 - API ключи
3. ✅ ЭТАП 3 - Структура приложения
4. ⚠️ ЭТАП 4 - Core модули (без ML кластеризации сначала)
5. ⚠️ ЭТАП 5 - Базовый Telegram bot (основной flow)

### 🟡 Важно (v1.0):
6. ЭТАП 6 - База данных
7. ЭТАП 7 - Celery для фоновых задач
8. ЭТАП 4 (продолжение) - ML кластеризация

### 🟢 Желательно (v1.1+):
9. ЭТАП 8 - Тестирование
10. ЭТАП 9 - Docker и деплой
11. ЭТАП 10 - Мониторинг

---

## 🚀 С чего начать ПРЯМО СЕЙЧАС

### Шаг 1: Создать структуру проекта (15 мин)
```bash
# Выполнить команды из раздела 1.2
mkdir -p src/{bot,core,api,models,database,tasks,services,utils}
# ... и т.д.
```

### Шаг 2: Установить зависимости (5 мин)
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Шаг 3: Настроить .env (10 мин)
```bash
cp .env.example .env
# Заполнить токены
```

### Шаг 4: Создать базовые файлы (20 мин)
- `src/utils/config.py`
- `src/utils/logger.py`
- `main.py`

### Шаг 5: Запустить и проверить (5 мин)
```bash
python main.py
```

**Ожидаемый результат:** Лог сообщение о запуске и проверке конфигурации

---

## 📞 Следующие действия

После завершения Этапов 1-3:

1. **Получить API ключи** (ЭТАП 2)
2. **Реализовать Yandex API Client** (самый критичный модуль)
3. **Создать простейший Telegram bot** (команды /start, /help)
4. **Интегрировать первый end-to-end flow:**
   - Пользователь вводит ключевые слова
   - Собираем из Wordstat
   - Показываем результат

После этого итеративно добавлять функционал согласно этапам.

---

## ⚠️ Критические моменты

1. **API Лимиты:**
   - Yandex Wordstat: 10 req/sec, 100k units/day
   - OpenAI: Pay-as-you-go, следить за стоимостью
   - Google Sheets: Квоты на запись

2. **Безопасность:**
   - НЕ коммитить `.env` и `config.yaml` в git
   - Шифровать токены в БД
   - Валидировать все входные данные

3. **Производительность:**
   - Кэшировать Wordstat запросы (TTL 7 дней)
   - Celery для длительных операций
   - Connection pooling для БД

4. **User Experience:**
   - Progress notifications обязательны (операции занимают 2-5 минут)
   - Понятные сообщения об ошибках
   - Возможность отменить операцию

---

## 📖 Справочные материалы

Вся детальная информация в документации:

1. [`direct-helper-architecture.md`](../direct-helper-architecture.md) - Общая архитектура
2. [`telegram-bot-architecture.md`](../telegram-bot-architecture.md) - Telegram Bot детали
3. [`wordstat-api-guide.md`](../wordstat-api-guide.md) - Yandex Wordstat API
4. [`google-sheets-integration.md`](../google-sheets-integration.md) - Google Sheets
5. [`service-architecture-v2.md`](../service-architecture-v2.md) - Сервисная архитектура

---

**Готовы начать? Давайте создавать! 🚀**
