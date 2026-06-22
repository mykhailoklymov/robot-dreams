# Технічна документація: Моніторинг macOS за допомогою Prometheus та Node Exporter

Цей проект налаштовує систему моніторингу для локальної операційної системи macOS з використанням Prometheus (у Docker) та Node Exporter (на хостовій ОС).

---

## 1. Набір команд для встановлення та запуску

Оскільки Node Exporter має збирати метрики реального заліза macOS, його встановлено безпосередньо на ОС за допомогою менеджера пакетів Homebrew, а Prometheus запущено в ізольованому Docker-контейнері.

```bash
# Крок 1: Встановлення та запуск Node Exporter на macOS через Homebrew
brew install node_exporter
brew services start node_exporter

# Крок 2: Перевірка, що Node Exporter віддає метрики (має повернути текст з метриками)
curl http://localhost:9100/metrics

# Крок 3: Запуск Prometheus за допомогою Docker Compose
# (Переконайтеся, що ви знаходитесь у папці з файлами docker-compose.yml та prometheus.yml)
docker-compose up -d

# Крок 4: Перевірка статусу контейнера
docker-compose ps