# ЛР 11. Django Notes App

**Тема:** Разработка веб-приложения для управления заметками на Django (хакатон)

**Репозиторий:** [github.com/l1234-tech/1_sem_hacaton_mast_syr](https://github.com/l1234-tech/1_sem_hacaton_mast_syr)

## Цель работы

Разработать полнофункциональное веб-приложение для управления заметками с аутентификацией пользователей, используя фреймворк Django.

## Задание

- Реализовать регистрацию и аутентификацию пользователей.
- CRUD-операции для заметок (создание, просмотр, редактирование, удаление).
- Поиск и фильтрация заметок по тегам.
- Адаптивный интерфейс на Bootstrap 5.
- Защита от CSRF-атак.
- Подготовка к развёртыванию на PythonAnywhere.

## Технологический стек

| Компонент    | Технология                    |
|-------------|-------------------------------|
| Backend     | Django 4.2                    |
| Frontend    | HTML5, CSS3, JS, Bootstrap 5  |
| База данных | SQLite3                       |
| Сервер      | Gunicorn + WhiteNoise         |
| Хостинг     | PythonAnywhere                |

## Архитектура (MVT)

**Модели:** `Note` (заголовок, содержание, даты, автор), `Tag` — связь Many-to-One с пользователем.

**Представления:** `NoteListView`, `NoteDetailView`, `NoteCreateView`, `NoteUpdateView`, `NoteDeleteView`, `CustomLoginView`, `note_search()`.

**Шаблоны:** `base.html`, `note_list.html`, `note_detail.html`, `note_form.html`, `note_confirm_delete.html`, `login.html`, `register.html`.

## Структура проекта

```
project/
├── config/          # Настройки Django
├── notes/           # Основное приложение
├── static/          # CSS, JS
├── templates/       # HTML-шаблоны
├── manage.py
├── db.sqlite3
└── requirements.txt
```

## Работа в команде

Проект разрабатывался в команде из двух человек (Мастеров, Сырчин) с использованием **GitHub Flow**:

- Каждая функция — в отдельной ветке
- Pull Request с ревью перед слиянием
- Issues для отслеживания задач
- Семантические коммиты

**Распределение:**

- **Сырчин:** templates, manage.py, models, hosts
- **Мастеров:** views, forms, urls, static, admin

## Выводы

В ходе хакатона разработано полнофункциональное веб-приложение на Django с полным циклом: от проектирования моделей до развёртывания. Реализованы аутентификация, CRUD-операции, поиск, адаптивный UI. Освоена совместная работа через GitHub Flow с code review и issues.
