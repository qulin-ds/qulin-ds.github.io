# ЛР 7. Декоратор логирования

**Тема:** Параметризуемый декоратор логирования, работа с API ЦБ РФ

## Цель работы

Создать универсальный параметризуемый декоратор `@logger`, поддерживающий вывод в `stdout`, файл и `logging.Logger`. Применить его для логирования запросов к API курсов валют ЦБ РФ.

## Задание

- Реализовать декоратор `@logger(handle=...)` с поддержкой разных типов вывода.
- Написать функцию `get_currencies` для получения курсов валют через API ЦБ РФ.
- Реализовать логирование в файл (`currency.log`).
- Добавить демонстрацию уровней логирования на примере решения квадратного уравнения.

## Код

### Декоратор

```python
import functools
import sys
import logging


def logger(func=None, *, handle=sys.stdout):
    def decorator(f):
        is_logging_logger = isinstance(handle, logging.Logger)

        @functools.wraps(f)
        def wrapper(*args, **kwargs):
            if is_logging_logger:
                log_info = handle.info
                log_error = handle.error
            else:
                def log_info(msg):
                    handle.write(f"INFO: {msg}\n")
                def log_error(msg):
                    handle.write(f"ERROR: {msg}\n")

            log_info(f"Calling {f.__name__} with args={args}, kwargs={kwargs}")

            try:
                result = f(*args, **kwargs)
                log_info(f"{f.__name__} returned {result!r}")
                return result
            except Exception as e:
                log_error(f"{f.__name__} raised {type(e).__name__}: {e}")
                raise

        return wrapper

    if func is None:
        return decorator
    else:
        return decorator(func)
```

### Получение курсов валют

```python
import requests

@logger(handle=sys.stdout)
def get_currencies(currency_codes, url="https://www.cbr-xml-daily.ru/daily_json.js"):
    try:
        response = requests.get(url, timeout=5)
        response.raise_for_status()
    except requests.exceptions.RequestException as e:
        raise ConnectionError(f"API is not available: {e}")

    try:
        data = response.json()
    except ValueError as e:
        raise ValueError(f"Invalid JSON: {e}")

    if "Valute" not in data:
        raise KeyError("Key 'Valute' not found in response")

    valute = data["Valute"]
    result = {}
    for code in currency_codes:
        if code not in valute:
            raise KeyError(f"Currency {code} not found in data")
        value = valute[code].get("Value")
        if not isinstance(value, (int, float)):
            raise TypeError(f"Rate for {code} has invalid type: {type(value)}")
        result[code] = float(value)
    return result
```

### Решение квадратного уравнения с уровнями логирования

```python
import math

@logger(handle=quad_logger)
def solve_quadratic(a, b, c):
    if a == 0 and b == 0:
        quad_logger.critical("Both a and b are zero; not an equation")
        raise ValueError("Both a and b are zero")
    try:
        a, b, c = float(a), float(b), float(c)
    except (TypeError, ValueError) as e:
        quad_logger.error(f"Invalid coefficients: {e}")
        raise

    d = b * b - 4 * a * c
    if d < 0:
        quad_logger.warning(f"Negative discriminant: {d}")
        return None
    if d == 0:
        return (-b / (2 * a),)
    sqrt_d = math.sqrt(d)
    return (-b - sqrt_d) / (2 * a), (-b + sqrt_d) / (2 * a)
```

## Выводы

Создан гибкий параметризуемый декоратор `@logger`, поддерживающий вывод в `stdout`, файл и `logging.Logger`. Продемонстрирована работа с API ЦБ РФ с обработкой ошибок сети, парсинга JSON и отсутствия данных. Реализованы все уровни логирования: INFO, WARNING, ERROR, CRITICAL.
