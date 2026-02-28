# ЛР 3. Рекурсивное бинарное дерево

**Тема:** Генерация бинарного дерева с помощью рекурсии

## Цель работы

Реализовать рекурсивную генерацию бинарного дерева заданной высоты с вычисляемыми значениями потомков.

## Задание

- Написать функции `left_leaf(root)` и `right_leaf(root)` для вычисления значений потомков.
- Написать рекурсивную функцию `gen_bin_tree(height, root)`.
- Каждый узел — список `[значение, левое_поддерево, правое_поддерево]`.
- Написать unit-тесты.

## Код

### Основные функции

```python
def right_leaf(root: int) -> int:
    """Правый потомок: right = 2 * root - 1"""
    return 2 * root - 1


def left_leaf(root: int) -> int:
    """Левый потомок: left = 2 * root + 1"""
    return root * 2 + 1


def gen_bin_tree(height: int, root: int):
    """
    Рекурсивно строит бинарное дерево в виде вложенных списков.

    >>> gen_bin_tree(0, 10)
    10
    >>> gen_bin_tree(1, 10)
    [10, 21, 19]
    >>> gen_bin_tree(2, 9)
    [9, [19, 39, 37], [17, 35, 33]]
    """
    if height == 0:
        return root
    return [root,
            gen_bin_tree(height - 1, left_leaf(root)),
            gen_bin_tree(height - 1, right_leaf(root))]
```

### Тесты

```python
import unittest
from lab_3 import left_leaf, right_leaf, gen_bin_tree


class MyTestCase(unittest.TestCase):
    def test_left_leaf(self):
        self.assertEqual(left_leaf(10), 21)
        self.assertEqual(left_leaf(5), 11)

    def test_right_leaf(self):
        self.assertEqual(right_leaf(10), 19)
        self.assertEqual(right_leaf(5), 9)

    def test_gen_bin_tree_height0(self):
        self.assertEqual(gen_bin_tree(0, 10), 10)

    def test_gen_bin_tree_height1(self):
        self.assertEqual(gen_bin_tree(1, 10), [10, 21, 19])

    def test_gen_bin_tree_height2(self):
        self.assertEqual(gen_bin_tree(2, 9), [9, [19, 39, 37], [17, 35, 33]])

if __name__ == '__main__':
    unittest.main()
```

## Выводы

Реализована рекурсивная генерация бинарного дерева. Каждый узел хранится в виде вложенного списка. Рекурсия позволяет лаконично описать древовидную структуру, но имеет ограничение по глубине стека вызовов.
