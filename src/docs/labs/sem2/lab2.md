# ЛР 2. Численные вычисления и анализ данных с NumPy

**Тема:** Численные вычисления, матричные операции, статистический анализ и визуализация данных с использованием библиотеки NumPy

## Цель работы

- Освоить основы работы с массивами и матрицами в NumPy.
- Научиться выполнять векторные и матричные операции (сложение, умножение, определитель, обратная матрица, решение СЛАУ).
- Изучить инструменты статистического анализа: среднее, медиана, стандартное отклонение, перцентили.
- Реализовать Min-Max нормализацию данных.
- Построить визуализации (гистограмма, тепловая карта, линейный график) с помощью Matplotlib и Seaborn.
- Обеспечить соответствие кода стандартам DATS: тесты, аннотации типов (PEP 484), документация (PEP 257), стиль (PEP 8).

## Задание

1. Реализовать функции создания и преобразования массивов (`create_vector`, `create_matrix`, `reshape_vector`, `transpose_matrix`).
2. Реализовать векторные операции (`vector_add`, `scalar_multiply`, `elementwise_multiply`, `dot_product`).
3. Реализовать матричные операции (`matrix_multiply`, `matrix_determinant`, `matrix_inverse`, `solve_linear_system`).
4. Реализовать статистический анализ (`load_dataset`, `statistical_analysis`, `normalize_data`).
5. Реализовать визуализацию данных (`plot_histogram`, `plot_heatmap`, `plot_line`).
6. Все функции должны проходить предоставленные тесты (17 тестов), содержать аннотации типов и документацию.

## Код

Исходный код: [github.com/qulin-ds/pythonf/tree/main/lab_12](https://github.com/qulin-ds/pythonf/tree/main/lab_12)

### Создание и обработка массивов

```python
def create_vector() -> np.ndarray:
    """Создать одномерный массив целых чисел от 0 до 9."""
    return np.arange(10)


def create_matrix() -> np.ndarray:
    """Создать матрицу 5x5 со случайными числами из [0, 1)."""
    return np.random.rand(5, 5)


def reshape_vector(vec: np.ndarray) -> np.ndarray:
    """Преобразовать одномерный массив формы (10,) в матрицу (2, 5)."""
    return vec.reshape(2, 5)


def transpose_matrix(mat: np.ndarray) -> np.ndarray:
    """Транспонировать матрицу."""
    return mat.T
```

**Нюансы:**

- `np.arange(10)` создаёт массив типа `int64` — это важно для дальнейших операций, где типы могут неявно меняться.
- `np.random.rand` генерирует числа из равномерного распределения на полуинтервале `[0, 1)`.
- `reshape` не копирует данные — возвращает *view* на тот же буфер памяти. Изменение элемента в результате изменит оригинал.

### Векторные операции

```python
def vector_add(a: np.ndarray, b: np.ndarray) -> np.ndarray:
    """Выполнить поэлементное сложение двух векторов."""
    return a + b


def scalar_multiply(vec: np.ndarray, scalar: Union[int, float]) -> np.ndarray:
    """Умножить вектор на скаляр."""
    return vec * scalar


def elementwise_multiply(a: np.ndarray, b: np.ndarray) -> np.ndarray:
    """Выполнить поэлементное умножение двух массивов."""
    return a * b


def dot_product(a: np.ndarray, b: np.ndarray) -> float:
    """Вычислить скалярное произведение двух векторов."""
    return np.dot(a, b)
```

**Нюансы:**

- Оператор `*` для `ndarray` выполняет **поэлементное** умножение (Hadamard product), а не матричное.
- `np.dot` для одномерных массивов вычисляет скалярное произведение, но для двумерных — матричное умножение. Для ясности при работе с матрицами лучше использовать оператор `@`.
- Broadcasting: если формы массивов не совпадают, NumPy автоматически «расширяет» размерности. Для скалярного умножения скаляр broadcast'ится до формы вектора.

### Матричные операции

```python
def matrix_multiply(a: np.ndarray, b: np.ndarray) -> np.ndarray:
    """Выполнить матричное умножение двух матриц."""
    return a @ b


def matrix_determinant(a: np.ndarray) -> float:
    """Вычислить определитель квадратной матрицы."""
    return np.linalg.det(a)


def matrix_inverse(a: np.ndarray) -> np.ndarray:
    """Вычислить обратную матрицу."""
    return np.linalg.inv(a)


def solve_linear_system(a: np.ndarray, b: np.ndarray) -> np.ndarray:
    """Решить систему линейных уравнений Ax = b."""
    return np.linalg.solve(a, b)
```

**Нюансы:**

- Оператор `@` (PEP 465) — рекомендуемый способ матричного умножения в Python 3.5+.
- `np.linalg.det` использует LU-разложение; для больших матриц возможна потеря точности из-за чисел с плавающей запятой.
- `np.linalg.inv` бросит `LinAlgError`, если матрица вырождена (определитель ≈ 0).
- `np.linalg.solve` численно устойчивее, чем вычисление `inv(A) @ b`, так как не требует явного нахождения обратной матрицы.

### Статистический анализ

```python
def load_dataset(path: str = "data/students_scores.csv") -> np.ndarray:
    """Загрузить CSV-файл и вернуть данные как NumPy-массив."""
    return pd.read_csv(path).to_numpy()


def statistical_analysis(data: np.ndarray) -> Dict[str, float]:
    """Вычислить основные статистические показатели массива."""
    return {
        "mean": np.mean(data),
        "median": np.median(data),
        "std": np.std(data),
        "min": np.min(data),
        "max": np.max(data),
        "percentile_25": np.percentile(data, 25),
        "percentile_75": np.percentile(data, 75),
    }


def normalize_data(data: np.ndarray) -> np.ndarray:
    """Выполнить Min-Max нормализацию массива в диапазон [0, 1]."""
    data_min = np.min(data)
    data_max = np.max(data)
    return (data - data_min) / (data_max - data_min)
```

**Нюансы:**

- `pd.read_csv().to_numpy()` — удобный способ получить массив из CSV, но теряется информация о заголовках столбцов.
- `np.std` по умолчанию использует `ddof=0` (смещённая оценка). Для несмещённой оценки следует передать `ddof=1`.
- При нормализации необходимо учитывать случай `max == min` — деление на ноль. В данной реализации предполагается, что данные содержат хотя бы два различных значения.

### Визуализация

```python
def plot_histogram(data: np.ndarray) -> None:
    """Построить и сохранить гистограмму распределения оценок."""
    os.makedirs("plots", exist_ok=True)
    plt.figure()
    plt.hist(data, bins=10, edgecolor="black", alpha=0.7)
    plt.title("Распределение оценок по математике")
    plt.xlabel("Оценка")
    plt.ylabel("Частота")
    plt.savefig("plots/histogram.png", dpi=150, bbox_inches="tight")
    plt.close()


def plot_heatmap(matrix: np.ndarray) -> None:
    """Построить и сохранить тепловую карту корреляции."""
    os.makedirs("plots", exist_ok=True)
    plt.figure()
    sns.heatmap(matrix, annot=True, cmap="coolwarm", fmt=".2f")
    plt.title("Корреляционная матрица предметов")
    plt.savefig("plots/heatmap.png", dpi=150, bbox_inches="tight")
    plt.close()


def plot_line(x: np.ndarray, y: np.ndarray) -> None:
    """Построить и сохранить линейный график оценок студентов."""
    os.makedirs("plots", exist_ok=True)
    plt.figure()
    plt.plot(x, y, marker="o", linestyle="-", color="steelblue")
    plt.title("Оценки студентов по математике")
    plt.xlabel("Номер студента")
    plt.ylabel("Оценка")
    plt.grid(True, alpha=0.3)
    plt.savefig("plots/line_plot.png", dpi=150, bbox_inches="tight")
    plt.close()
```

**Нюансы:**

- `matplotlib.use("Agg")` устанавливается в начале модуля для работы без графического интерфейса (важно для серверов и CI).
- `plt.close()` обязателен после `savefig`, иначе фигуры накапливаются в памяти.
- `bbox_inches="tight"` автоматически обрезает пустые поля вокруг графика.

## Тестирование

Все 17 тестов проходят успешно:

```
test.py::test_create_vector PASSED
test.py::test_create_matrix PASSED
test.py::test_reshape_vector PASSED
test.py::test_vector_add PASSED
test.py::test_scalar_multiply PASSED
test.py::test_elementwise_multiply PASSED
test.py::test_dot_product PASSED
test.py::test_matrix_multiply PASSED
test.py::test_matrix_determinant PASSED
test.py::test_matrix_inverse PASSED
test.py::test_solve_linear_system PASSED
test.py::test_load_dataset PASSED
test.py::test_statistical_analysis PASSED
test.py::test_normalization PASSED
test.py::test_plot_histogram PASSED
test.py::test_plot_heatmap PASSED
test.py::test_plot_line PASSED

============================= 17 passed =============================
```

## Соответствие DATS

| Критерий              | Стандарт   | Статус     |
|-----------------------|------------|------------|
| **Тесты**            | pytest     | 17/17 passed |
| **Аннотации типов**  | PEP 484    | Все функции аннотированы (`-> np.ndarray`, `-> float`, `-> Dict[str, float]`, `-> None`) |
| **Документация**     | PEP 257    | Все функции содержат docstring с описанием, `Args` и `Returns` |
| **Стиль кода**       | PEP 8      | Соблюдён (проверено линтером) |

## Выводы

В ходе выполнения лабораторной работы были получены следующие навыки:

1. **Работа с массивами NumPy** — создание, изменение формы, транспонирование. Понимание разницы между `view` и `copy`.
2. **Векторные операции** — поэлементные операции, broadcasting, скалярное произведение.
3. **Матричные операции** — умножение (`@`), определитель, обратная матрица, решение СЛАУ. Понимание численной устойчивости `linalg.solve` vs `inv`.
4. **Статистический анализ** — вычисление описательных статистик, Min-Max нормализация. Осознание особенностей `ddof` в `np.std`.
5. **Визуализация** — построение графиков с Matplotlib и Seaborn, сохранение в файлы с настройкой DPI и backend.
6. **Качество кода** — соблюдение стандартов DATS: тесты, аннотации типов, документация, PEP 8.
