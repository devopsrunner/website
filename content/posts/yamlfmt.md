---
title: "Yamlfmt: Форматирование YAML файлов"
date: 2024-08-02T20:59:15+03:00
type: docs
---

## Введение

**Yamlfmt** — это утилита командной строки для форматирования YAML-файлов. Она упрощает соблюдение единых правил оформления, делая файлы более читаемыми и поддерживаемыми. В данной статье рассмотрим основные способы использования yamlfmt, различные режимы работы и доступные флаги конфигурации.

## Основное использование

Наиболее простое использование `yamlfmt` заключается в применении стандартной конфигурации и указании пути к файлу или директории для форматирования.

### Форматирование одного файла

Для форматирования одного файла используйте команду:

```bash
yamlfmt x.yaml
```

### Форматирование директории

Для рекурсивного поиска и форматирования всех файлов с расширением `.yml` или `.yaml` в указанной директории:

```bash
yamlfmt .
```

### Использование Doublestar

Для более динамичного поиска файлов можно использовать Doublestar. Например:

```bash
yamlfmt -dstar "**/*.yaml"
```

## Три режима работы

Yamlfmt поддерживает три режима работы: форматирование, проверка (Lint) и тестовый запуск (Dry Run).

### Форматирование

Это основной режим работы по умолчанию. Yamlfmt находит все файлы, соответствующие указанным шаблонам, и форматирует их, перезаписывая исходные файлы.

### Форматирование из stdin

Этот режим считывает данные из стандартного ввода и выводит отформатированные данные в стандартный вывод. Для его использования передайте `-` или `/dev/stdin`, либо используйте флаг `-in`:

```bash
cat x.yaml | yamlfmt -
cat x.yaml | yamlfmt /dev/stdin
cat x.yaml | yamlfmt -in
```

### Тестовый запуск (Dry Run)

Этот режим активируется с помощью флага `-dry`. Он выводит результат форматирования в консоль без изменения файлов. Полезен для предварительного просмотра изменений:

```bash
yamlfmt -dry "путь/к/файлам/*.yaml"
```

### Проверка (Lint)

Этот режим активируется флагом `-lint`. Yamlfmt проверяет файлы на наличие различий в формате и завершает работу с кодом 1, если такие различия найдены.

```bash
yamlfmt -lint .
```

## Флаги командной строки

### Операционные флаги

| Имя | Флаг | Описание |
| - | - | - |
| Помощь | `-help` | Печатает информацию о команде. |
| Версия | `-version` | Печатает версию yamlfmt. |
| Конфигурация | `-print_conf` | Печатает используемую конфигурацию. |
| Тестовый запуск | `-dry` | Выполняет тестовый запуск. |
| Проверка | `-lint` | Выполняет проверку формата. |
| Тихий режим | `-quiet` | Использует тихий режим (влияет на Dry Run и Lint). |
| Ввод | `-in` | Считывает данные из stdin и выводит в stdout. |

### Конфигурационные флаги

| Имя | Флаг | Тип | Описание |
| - | - | - | - |
| Файл конфигурации | `-conf` | string | Путь к файлу конфигурации. |
| Глобальная конфигурация | `-global_conf` | bool | Использовать конфигурацию из системного каталога. |
| Отключить глобальную конфигурацию | `-no_global_conf` | bool | Отключить использование конфигурации из системного каталога. |
| Doublestar | `-dstar` | bool | Использовать Doublestar для включения и исключения путей. |
| Исключить | `-exclude` | []string | Паттерны для исключения путей. |
| Gitignore | `-gitignore_excludes` | bool | Использовать gitignore для исключения путей. |
| Путь к gitignore | `-gitignore_path` | string | Путь к файлу gitignore (по умолчанию `.gitignore`). |
| Расширения | `-extensions` | []string | Расширения файлов для поиска. |
| Конфигурация форматтера | `-formatter` | []string | Параметры для настройки форматтера. |
| Отладка | `-debug` | []string | Включить отладочные сообщения. |
| Формат вывода | `-output_format` | string | Формат вывода (по умолчанию "default"). |

## Файл конфигурации

Yamlfmt автоматически ищет файлы конфигурации с определенными именами (`.yamlfmt`, `yamlfmt.yml`, и т.д.). Вы также можете указать путь к конфигурационному файлу с помощью флага `-conf`.

### Настройка форматтера

Пример конфигурации с использованием базового форматтера:

```yaml
formatter:
type: basic
indent: 2
include_document_start: true
```

## Пример использования в pre-commit

Для использования yamlfmt в качестве хука pre-commit добавьте следующий блок в ваш `.pre-commit-config.yaml`:

```yaml
- repo: https://github.com/google/yamlfmt
rev: v0.10.0
hooks:
- id: yamlfmt
```

## Заключение

Yamlfmt — это мощный инструмент для обеспечения единообразного форматирования YAML-файлов. Он поддерживает различные режимы работы, гибкие настройки и легко интегрируется в существующие процессы CI/CD.

Для получения дополнительной информации и установки, посетите [страницу проекта Yamlfmt на GitHub](https://github.com/google/yamlfmt).
