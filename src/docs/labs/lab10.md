# ЛР 10. Численное интегрирование

**Тема:** Численное интегрирование методом левых прямоугольников, оптимизация через Cython и многопоточность

## Цель работы

Реализовать численное интегрирование методом левых прямоугольников. Изучить способы оптимизации: Cython-компиляция и многопоточное выполнение.

## Задание

- Написать функцию `integrate(f, a, b, n_iter)` на чистом Python.
- Написать Cython-версию для ускорения вычислений.
- Реализовать параллельное вычисление с `concurrent.futures`.
- Провести бенчмарк всех вариантов.

## Код

### Базовая реализация (Python)

```python
import math


def integrate(f, a: float, b: float, *, n_iter: int = 100_000) -> float:
    """
    Численное вычисление интеграла по формуле левых прямоугольников.

    >>> round(integrate(math.cos, 0.0, math.pi, n_iter=10_000), 3)
    0.0
    >>> round(integrate(lambda x: x**2, 0.0, 1.0, n_iter=10_000), 3)
    0.333
    """
    if n_iter <= 0:
        raise ValueError("n_iter must be a positive integer")

    acc = 0.0
    step = (b - a) / n_iter
    for i in range(n_iter):
        acc += f(a + i * step) * step
    return acc
```

### Структура проекта

```
lab_10/
├── integrate.py             # Базовая реализация
├── cy_integrate.pyx         # Cython-версия
├── concurrent_integrate.py  # Многопоточная версия
├── bench_timeit.py          # Бенчмарк timeit
├── bench_concurrent.py      # Бенчмарк concurrent
├── cy_bench.py              # Бенчмарк Cython
├── setup_cy.py              # Сборка Cython-модуля
└── test_integrate.py        # Тесты
```

## Выводы

Метод левых прямоугольников прост в реализации, но требует большого числа итераций для высокой точности. Cython-компиляция даёт значительное ускорение за счёт статической типизации и компиляции в C. Многопоточность через `concurrent.futures` эффективна при разделении отрезка на независимые подзадачи.
