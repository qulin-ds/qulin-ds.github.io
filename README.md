# qulin-ds.github.io

Сайт-портфолио лабораторных работ — [https://qulin-ds.github.io/](https://qulin-ds.github.io/)

## Описание

Статический сайт, созданный с помощью [MkDocs](https://www.mkdocs.org/) в рамках учебного курса.
Содержит главную страницу, информацию об авторе и отчёты по лабораторным работам по Python.

## Обоснование выбора темы

Выбрана тема **[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)** по следующим причинам:

1. **Современный дизайн** — Material Design обеспечивает чистый и профессиональный вид, подходящий для портфолио.
2. **Адаптивность** — корректное отображение на мобильных устройствах и десктопе.
3. **Тёмная тема** — встроенный переключатель светлой/тёмной схемы.
4. **Навигация** — табы, боковое меню и кнопка «наверх» для удобства.
5. **Расширения** — подсветка синтаксиса кода, admonition-блоки, оглавления с якорями.
6. **Популярность** — самая используемая тема для MkDocs с отличной документацией.

## Структура проекта

```
qulin-ds.github.io/
├── docs/                # Собранный сайт (GitHub Pages)
├── src/                 # Исходники MkDocs
│   ├── mkdocs.yml
│   └── docs/
│       ├── index.md
│       ├── about.md
│       └── labs/
│           ├── index.md
│           ├── lab1.md ... lab11.md
├── .gitignore
└── README.md
```

## Локальная разработка

```bash
py -m venv venv
.\venv\Scripts\Activate.ps1
pip install mkdocs mkdocs-material
cd src
mkdocs serve            # http://127.0.0.1:8000
mkdocs build -d ../docs # Сборка для GitHub Pages
```
