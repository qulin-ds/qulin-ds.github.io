# ЛР 6. Сравнение производительности

**Тема:** Бенчмарк рекурсивного и итеративного построения дерева с визуализацией

## Цель работы

Сравнить время построения бинарного дерева рекурсивным и итеративным методами. Построить графики с помощью `matplotlib`.

## Задание

- Реализовать рекурсивную и итеративную функции построения дерева.
- Написать функцию бенчмарка с использованием `timeit`.
- Построить график зависимости времени от высоты дерева.

## Код

```python
import timeit
import matplotlib.pyplot as plt
from typing import Callable, List


def build_tree_recursive(height, root,
                         left_branch=lambda x: x * 2 + 1,
                         right_branch=lambda x: 2 * x - 1):
    if height <= 0:
        return []
    return [
        root,
        build_tree_recursive(height - 1, left_branch(root), left_branch, right_branch),
        build_tree_recursive(height - 1, right_branch(root), left_branch, right_branch)
    ]


def build_tree_iterative(height, root,
                         left_branch=lambda x: x * 2 + 1,
                         right_branch=lambda x: 2 * x - 1):
    if height < 1:
        return []
    tree = [root, [], []]
    current_level = [tree]
    level = 1
    while level < height:
        next_level = []
        for node in current_level:
            left_node = [left_branch(node[0]), [], []]
            right_node = [right_branch(node[0]), [], []]
            node[1] = left_node
            node[2] = right_node
            next_level.extend([left_node, right_node])
        current_level = next_level
        level += 1
    return tree


def benchmark(func, heights, root=9, number=10, repeat=3):
    results = []
    for h in heights:
        times = timeit.repeat(lambda: func(h, root), number=number, repeat=repeat)
        results.append(min(times))
    return results


def main():
    ROOT = 9
    HEIGHTS = list(range(1, 7))

    recursive_times = benchmark(build_tree_recursive, HEIGHTS, root=ROOT)
    iterative_times = benchmark(build_tree_iterative, HEIGHTS, root=ROOT)

    for h, r, i in zip(HEIGHTS, recursive_times, iterative_times):
        print(f"Высота {h}: рекурсивно = {r:.6f}s, итеративно = {i:.6f}s")

    plt.plot(HEIGHTS, recursive_times, label="Рекурсивная")
    plt.plot(HEIGHTS, iterative_times, label="Итеративная")
    plt.xlabel("Высота дерева")
    plt.ylabel("Время построения (сек)")
    plt.title("Сравнение рекурсивного и итеративного построения дерева")
    plt.legend()
    plt.grid(True)
    plt.show()

if __name__ == "__main__":
    main()
```

## Выводы

При небольших высотах дерева (1–3) разница между рекурсией и итерацией минимальна. С увеличением высоты итеративный метод показывает преимущество за счёт отсутствия накладных расходов на стек вызовов. Визуализация с `matplotlib` наглядно подтверждает эту тенденцию.
