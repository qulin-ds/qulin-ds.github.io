# ЛР 2. Бинарный поиск

**Тема:** Реализация алгоритма бинарного поиска

## Цель работы

Реализовать алгоритм бинарного поиска числа в отсортированном массиве с подсчётом количества шагов.

## Задание

- Написать функцию `guess_num(num, values)`, возвращающую найденное число и количество шагов.
- Если число не найдено — вернуть `(None, steps)`.
- Написать unit-тесты.

## Код

### Основная функция

```python
from typing import List, Optional, Tuple


def guess_num(num: int, values: List[int]) -> Tuple[Optional[int], int]:
    low = 0
    high = len(values) - 1
    steps = 0

    while low <= high:
        steps += 1
        mid = (low + high) // 2
        guess = values[mid]

        if guess == num:
            return guess, steps
        elif guess > num:
            high = mid - 1
        else:
            low = mid + 1

    return None, steps


if __name__ == "__main__":
    num = int(input("Введите загаданное число: "))
    low = int(input("Введите нижнюю границу: "))
    high = int(input("Введите верхнюю границу: "))
    values = list(range(low, high + 1))

    result, steps = guess_num(num, values)
    if result is not None:
        print(f"Число найдено: {result} (за {steps} шагов)")
    else:
        print(f"Число не найдено (проверено {steps} шагов)")
```

### Тесты

```python
from lab_2 import guess_num
import unittest


class NumTestCase(unittest.TestCase):
    def test_found_number(self):
        self.assertEqual(guess_num(5, [1, 3, 5, 7, 9]), (5, 1))
        self.assertEqual(guess_num(42, [42]), (42, 1))

    def test_not_found_number(self):
        self.assertEqual(guess_num(4, [1, 3, 5, 7, 9]), (None, 3))
        self.assertEqual(guess_num(10, []), (None, 0))

if __name__ == '__main__':
    unittest.main()
```

## Выводы

Реализован алгоритм бинарного поиска со сложностью O(log n). Функция корректно обрабатывает случаи пустого массива и отсутствия элемента. Подсчёт шагов наглядно демонстрирует эффективность алгоритма.
