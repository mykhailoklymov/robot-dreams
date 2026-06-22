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
docker compose up -d

# Крок 4: Перевірка статусу контейнера
docker compose ps


Після цього веб-інтерфейс Prometheus буде доступний за адресою: http://localhost:9090

2. Аналіз конфігураційного файлу prometheus.yml
Який інтервал збору метрик налаштовано?
У блоці global налаштовано параметр scrape_interval: 60s. Це означає, що Prometheus звертається до джерел метрик кожні 60 секунд.

Скільки джерел метрик налаштовано?
Налаштовано 2 джерела метрик (targets) у блоці scrape_configs:

prometheus (localhost:9090) — Prometheus збирає метрики сам із себе.

macos-node-exporter (host.docker.internal:9100) — збір метрик моєї ОС macOS.

3. Метрики використання CPU на macOS
Оскільки архітектура macOS (Darwin) відрізняється від Linux, Node Exporter використовує специфічні метрики для процесора. Ось основні з них, які можна ввести в пошуковий рядок Prometheus (http://localhost:9090):

node_cpu_seconds_total — Загальний час роботи CPU в секундах за різними режимами (user, system, idle). Використовується для розрахунку загального завантаження.

node_load1 — Середнє завантаження системи за останню 1 хвилину (Load Average).

node_load5 — Середнє завантаження системи за останні 5 хвилин.

node_load15 — Середнє завантаження системи за останні 15 хвилин.