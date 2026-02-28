# ЛР 5. Итеративное бинарное дерево

**Тема:** Генерация бинарного дерева без рекурсии

## Цель работы

Реализовать итеративную версию генерации бинарного дерева с использованием цикла вместо рекурсии.

## Задание

- Переписать рекурсивную функцию `gen_bin_tree` из ЛР 3 в итеративном стиле.
- Поддержать параметризацию функций вычисления потомков через `lambda`.
- Написать unit-тесты.

## Код

### Основная функция

```python
def gen_bin_tree(
    height: int,
    root: int,
    left_branch=lambda x: x * 2 + 1,
    right_branch=lambda x: 2 * x - 1
) -> list:
    """
    Создаёт бинарное дерево заданной высоты без рекурсии.
    Пустые ветви обозначаются пустыми списками [].

    >>> gen_bin_tree(3, 9)
    [9, [19, [39, [], []], [37, [], []]], [17, [35, [], []], [33, [], []]]]
    """
    if height < 1:
        return []

    tree = [root, [], []]
    current_level = [tree]
    level = 1

    while level < height:
        next_level = []
        for node in current_level:
            left_value = left_branch(node[0])
            right_value = right_branch(node[0])

            left_node = [left_value, [], []]
            right_node = [right_value, [], []]

            node[1] = left_node
            node[2] = right_node

            next_level.append(left_node)
            next_level.append(right_node)

        current_level = next_level
        level += 1

    return tree
```

### Тесты

```python
import unittest
from lab_5 import gen_bin_tree


class TestGenBinTree(unittest.TestCase):
    def test_basic_tree(self):
        tree = gen_bin_tree(3, 9)
        expected = [
            9,
            [19, [39, [], []], [37, [], []]],
            [17, [35, [], []], [33, [], []]]
        ]
        self.assertEqual(tree, expected)

    def test_height_one(self):
        tree = gen_bin_tree(1, 7)
        expected = [7, [], []]
        self.assertEqual(tree, expected)

    def test_height_zero(self):
        tree = gen_bin_tree(0, 9)
        expected = []
        self.assertEqual(tree, expected)

    def test_different_root(self):
        tree = gen_bin_tree(2, 3)
        self.assertEqual(tree[0], 3)

if __name__ == "__main__":
    unittest.main()
```

## Выводы

Итеративная версия генерации бинарного дерева использует очередь уровней вместо стека вызовов. Это устраняет ограничение по глубине рекурсии и делает алгоритм более предсказуемым по потреблению памяти. Функции вычисления потомков вынесены в параметры `lambda`, что обеспечивает гибкость.
