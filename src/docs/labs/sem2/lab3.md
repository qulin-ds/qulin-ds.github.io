# ЛР 3. CI/CD для статического сайта в SourceCraft

**Тема:** Настройка CI/CD для автоматической сборки и публикации статического сайта MkDocs на платформе SourceCraft

## Цель работы

- Освоить работу с платформой SourceCraft (авторизация, создание организации и репозитория).
- Научиться создавать персональные токены доступа (PAT) для работы по HTTPS.
- Настроить дополнительный удалённый репозиторий (remote) для синхронизации кода между GitHub и SourceCraft.
- Написать конфигурацию CI/CD-пайплайна (`.sourcecraft/ci.yaml`) для автоматической сборки MkDocs-сайта.
- Развернуть статический сайт с помощью SourceCraft Sites.

## Задание

1. Авторизоваться на [sourcecraft.dev](https://sourcecraft.dev/) с помощью аккаунта Яндекс.
2. Создать публичную организацию.
3. Создать пустой публичный репозиторий.
4. Создать персональный токен доступа (PAT) с правами Maintainer.
5. Добавить удалённый репозиторий SourceCraft в локальный проект.
6. Настроить CI/CD-пайплайн для сборки MkDocs и публикации через SourceCraft Sites.
7. Предоставить ссылки на репозиторий и развёрнутый сайт.

## Ход выполнения

### Шаг 1. Авторизация на SourceCraft

Выполнена авторизация на [sourcecraft.dev](https://sourcecraft.dev/) с помощью аккаунта Яндекс. Аккаунт: `artemmasterov8`.

### Шаг 2. Создание организации

Создана публичная организация `qulin` на платформе SourceCraft.

### Шаг 3. Создание репозитория

Создан публичный репозиторий [artemmasterov8/portfolio](https://sourcecraft.dev/artemmasterov8/portfolio) для размещения исходников статического сайта.

### Шаг 4. Создание персонального токена (PAT)

Создан персональный токен доступа для работы по HTTPS:

- **Права:** Maintainer
- **Срок действия:** 1 год
- **Доступ:** Все репозитории

Документация по созданию токена: [sourcecraft.dev/portal/docs/ru/sourcecraft/security/pat](https://sourcecraft.dev/portal/docs/ru/sourcecraft/security/pat)

### Шаг 5. Добавление удалённого репозитория

Добавлен второй remote `sourcecraft` в локальный Git-проект:

```bash
git remote add sourcecraft https://<имя_аккаунта>:<токен>@git.sourcecraft.dev/artemmasterov8/portfolio.git
```

Проверка списка удалённых репозиториев:

```bash
git remote -v
```

```
origin      https://github.com/qulin-ds/qulin-ds.github.io.git (fetch)
origin      https://github.com/qulin-ds/qulin-ds.github.io.git (push)
sourcecraft https://artemmasterov8:***@git.sourcecraft.dev/artemmasterov8/portfolio.git (fetch)
sourcecraft https://artemmasterov8:***@git.sourcecraft.dev/artemmasterov8/portfolio.git (push)
```

### Шаг 6. Настройка CI/CD

#### Конфигурация пайплайна — `.sourcecraft/ci.yaml`

```yaml
on:
  push:
    - workflows: [build-site]
      filter:
        branches: [main]

workflows:
  build-site:
    tasks:
      - name: build-and-deploy
        cubes:
          - name: build-mkdocs
            image: docker.io/library/python:3.12-slim
            script:
              - |
                pip install mkdocs mkdocs-material
                cd src
                mkdocs build -d ../site

          - name: deploy-to-release
            needs: ["build-mkdocs"]
            script:
              - |
                AUTH_URL=$(echo "$SOURCECRAFT_REPO_URL" | sed "s|git@|sourcecraft-ci:${SOURCECRAFT_TOKEN}@|")
                git config --global user.name "CI Bot"
                git config --global user.email "ci@sourcecraft.dev"
                cd site
                git init
                git checkout -b release
                git add -A
                git commit -m "Deploy from ${SOURCECRAFT_COMMIT_SHORT_SHA}"
                git push -f "$AUTH_URL" release
```

**Описание пайплайна:**

| Куб | Образ | Назначение |
|-----|-------|------------|
| `build-mkdocs` | `python:3.12-slim` | Установка зависимостей и сборка сайта через `mkdocs build` |
| `deploy-to-release` | нативный | Публикация собранных файлов в ветку `release` с использованием `$SOURCECRAFT_TOKEN` |

Пайплайн автоматически запускается при каждом пуше в ветку `main`.

#### Конфигурация хостинга — `.sourcecraft/sites.yaml`

```yaml
site:
  ref: "release"
```

Эта конфигурация указывает SourceCraft Sites использовать ветку `release` (куда CI/CD деплоит собранные файлы) в качестве источника для статического сайта.

### Шаг 7. Пуш в SourceCraft

```bash
git add .sourcecraft/
git commit -m "feat: add SourceCraft CI/CD and Sites configuration"
git push sourcecraft main
```

После пуша в разделе CI/CD репозитория запустился пайплайн `build-site`.

## Результат

- **Репозиторий:** [https://sourcecraft.dev/artemmasterov8/portfolio](https://sourcecraft.dev/artemmasterov8/portfolio)
- **Статический сайт:** [https://artemmasterov8.sourcecraft.site/portfolio/](https://artemmasterov8.sourcecraft.site/portfolio/)

## Схема работы CI/CD

```
git push sourcecraft main
        │
        ▼
┌─────────────────────────┐
│   Триггер: on.push      │
│   Ветка: main            │
└────────┬────────────────┘
         ▼
┌─────────────────────────┐
│  build-mkdocs            │
│  Docker: python:3.12     │
│  pip install mkdocs ...  │
│  mkdocs build -d ../site │
└────────┬────────────────┘
         ▼
┌─────────────────────────┐
│  deploy-to-release       │
│  git push -f → release   │
└────────┬────────────────┘
         ▼
┌─────────────────────────┐
│  SourceCraft Sites       │
│  sites.yaml → ref:release│
│  → artemmasterov8        │
│    .sourcecraft.site     │
│    /portfolio/           │
└─────────────────────────┘
```

## Выводы

В ходе выполнения лабораторной работы были получены следующие навыки:

1. **Работа с SourceCraft** — авторизация, создание организации, репозитория и персонального токена доступа.
2. **Управление несколькими remote** — настройка параллельной синхронизации с GitHub и SourceCraft через `git remote add`.
3. **Настройка CI/CD** — написание пайплайна `.sourcecraft/ci.yaml` с использованием Docker-кубов для сборки MkDocs.
4. **SourceCraft Sites** — конфигурация `.sourcecraft/sites.yaml` для автоматического хостинга статического сайта из ветки `release`.
5. **Автоматизация деплоя** — при каждом пуше в `main` сайт автоматически пересобирается и публикуется без ручного вмешательства.
