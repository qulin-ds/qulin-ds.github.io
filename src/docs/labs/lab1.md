# ЛР 1. Two Sum

**Тема:** Поиск двух чисел в массиве, дающих заданную сумму

## Цель работы

Реализовать функцию `two_sum`, которая находит индексы двух элементов массива, сумма которых равна заданному значению. Освоить написание unit-тестов.

## Задание

- Написать функцию `two_sum(nums, target)`, возвращающую список из двух индексов.
- Обработать случаи: отрицательные числа, нули, отсутствие решения, пустой массив.
- Реализовать проверку типов (выбрасывать `TypeError` для нецелочисленных данных).
- Написать тесты с использованием `unittest`.

## Код

### Основная функция

```python
nums = [3, 2]
target = 5

def two_sum(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] == str(nums[i]) or nums[j] == str(nums[j]) or target == str(target):
                raise TypeError('неверный тип данных')
            if nums[i] != int(nums[i]) or nums[j] != int(nums[j]) or target != int(target):
                raise TypeError('неверный тип данных')
            if nums[i] + nums[j] == target:
                res = [i, j]
                return res
    return None

print(two_sum(nums, target))
```

### Тесты

```python
import unittest
from main import two_sum

class TestTwoSum(unittest.TestCase):
    def test_1(self):
        self.assertEqual(two_sum([3, 2], 5), [0, 1])

    def test_2(self):
        self.assertEqual(two_sum([3, 3], 6), [0, 1])

    def test_3(self):
        self.assertEqual(two_sum([-2, -3, -4, -5], -7), [0, 3])
        self.assertEqual(two_sum([-1, 2, 5, -3], 1), [0, 1])

    def test_4(self):
        self.assertEqual(two_sum([0, 4, 3, 0], 0), [0, 3])

    def test_5(self):
        self.assertIsNone(two_sum([1, 2, 3], 10))
        self.assertIsNone(two_sum([], 0))

    def test_6(self):
        self.assertIsNone(two_sum([], 5))

    def test_7(self):
        self.assertIsNone(two_sum([5], 5))

    def test_8(self):
        with self.assertRaises(TypeError):
            two_sum([2, 'a', 4, 5], 7)

    def test_9(self):
        with self.assertRaises(TypeError):
            two_sum([21.4, 7, 45, 3], 29)

    def test_10(self):
        with self.assertRaises(TypeError):
            two_sum([2, 3, 4], '10')

    def test_11(self):
        with self.assertRaises(TypeError):
            two_sum([2, 3, 4], 10.0)

    def test_12(self):
        with self.assertRaises(TypeError):
            two_sum([2, 3, 4], 5.5)

if __name__ == '__main__':
    unittest.main()
```

## Выводы

В ходе выполнения лабораторной работы реализована функция поиска двух чисел с заданной суммой методом перебора (O(n²)). Написаны 12 unit-тестов, покрывающих основные сценарии: положительные и отрицательные числа, нули, пустой массив, проверку типов. Освоены базовые приёмы тестирования с `unittest`.
