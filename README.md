# Text Kit

## 1. Назначение программы

`Text Kit` — библиотека модулей и консольная утилита для форматирования текста, работы с датами, расчёта базовой числовой статистики и валидации пользовательских данных (email, телефонов, паролей)[cite: 1].

---

## 2. Принцип разделения функций на модули

Функции распределены по двум директориям (`utils/` и `services/`) на основе их универсальности и предметной области:

* **`app/utils/text.py`** — универсальные утилиты для обработки строк (`clean_spaces`, `shorten`, `initials`, `slug`)[cite: 1, 2]. Не содержат специфичной прикладной логики и могут использоваться в любых сторонних задачах[cite: 2].
* **`app/utils/dates.py`** — независимые вспомогательные функции для парсинга, форматирования, проверки выходных и вычисления разницы дат (`parse_date`, `format_date`, `is_weekend`, `days_between`)[cite: 1, 4].
* **`app/services/statistics.py`** — прикладные расчёты проекта над числовыми выборками (`average`, `median`, `spread`, `count_values`)[cite: 1, 5].
* **`app/services/validation.py`** — правила проверки и нормализации входных данных (`is_email`, `normalize_phone`, `is_phone`, `password_problems`)[cite: 1, 3]. Для предобработки строк функция `is_email` обращается к модулю `app.utils.text`[cite: 1, 2].

---

## 3. Структура проекта

```text
text-kit/
├── README.md                      # Документация, описание структуры, команд запуска и тестов
├── app/
│   ├── __init__.py                # Маркер пакета app
│   ├── main.py                    # Точка входа в программу (константы данных и запуск main())
│   ├── services/                  # Прикладная логика и сервисы проекта
│   │   ├── __init__.py            # Маркер пакета services
│   │   ├── statistics.py          # Модуль статистических расчётов (average, median, spread, count_values)
│   │   └── validation.py          # Модуль валидации (is_email, normalize_phone, is_phone, password_problems)
│   └── utils/                     # Универсальные модули-утилиты
│       ├── __init__.py            # Маркер пакета utils
│       ├── dates.py               # Модуль работы с датами (parse_date, format_date, is_weekend, days_between)
│       └── text.py                # Модуль строковых операций (clean_spaces, shorten, initials, slug)
└── tests/                         # Автоматические тесты (unittest)
    ├── test_dates.py              # Проверка функций модуля dates
    ├── test_statistics.py         # Проверка функций модуля statistics
    ├── test_text.py               # Проверка функций модуля text
    └── test_validation.py         # Проверка функций модуля validation
```

---

## 4. Команды запуска и проверки

Все команды выполняются из корневой папки `text-kit`:

* **Запуск демонстрации программы:**
  ```bash
  python -m app.main
  ```

* **Запуск автоматических тестов:**
  ```bash
  python -m unittest discover -s tests -v
  ```

---

## 5. Ожидаемый вывод программы

При выполнении команды `python -m app.main` в терминал выводится[cite: 1]:

```text
СТРОКИ
Заголовок: Отчёт за март
Коротко: Отчёт за…
Инициалы: П. И. Смирнов
Адрес страницы: otchet-za-mart

ЧИСЛА
Среднее: 4.33
Медиана: 4.5
Разброс: 2
Сколько каких: {'зачёт': 3, 'незачёт': 1}

ПРОВЕРКА ДАННЫХ
Почта 'student@college.ru': подходит
Почта 'не почта': не подходит
Телефон '8 (900) 123-45-67': +79001234567
Телефон '123': не подходит
Пароль 'Qwerty12345': подходит
Пароль 'qwerty': нет ни одной цифры, нет заглавной буквы

ДАТЫ
Начало: 02.03.2026 рабочий день
Конец: 31.03.2026 рабочий день
Дней между ними: 29
```

---

## 6. Результат выполнения автотестов

При выполнении команды `python -m unittest discover -s tests -v` все 19 тестов успешно проходят[cite: 2, 3, 4, 5]:

```text
test_days_between (test_dates.TestDates.test_days_between) ... ok
test_format_date (test_dates.TestDates.test_format_date) ... ok
test_is_weekend (test_dates.TestDates.test_is_weekend) ... ok
test_parse_date (test_dates.TestDates.test_parse_date) ... ok
test_average (test_statistics.TestStatistics.test_average) ... ok
test_average_empty (test_statistics.TestStatistics.test_average_empty) ... ok
test_count_values (test_statistics.TestStatistics.test_count_values) ... ok
test_median (test_statistics.TestStatistics.test_median) ... ok
test_spread (test_statistics.TestStatistics.test_spread) ... ok
test_clean_spaces (test_text.TestText.test_clean_spaces) ... ok
test_initials (test_text.TestText.test_initials) ... ok
test_initials_empty (test_text.TestText.test_initials_empty) ... ok
test_shorten (test_text.TestText.test_shorten) ... ok
test_slug (test_text.TestText.test_slug) ... ok
test_is_email (test_validation.TestValidation.test_is_email) ... ok
test_is_phone (test_validation.TestValidation.test_is_phone) ... ok
test_normalize_phone (test_validation.TestValidation.test_normalize_phone) ... ok
test_normalize_phone_error (test_validation.TestValidation.test_normalize_phone_error) ... ok
test_password_problems (test_validation.TestValidation.test_password_problems) ... ok

----------------------------------------------------------------------
Ran 19 tests in 0.003s

OK
```