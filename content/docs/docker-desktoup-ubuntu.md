---
title: "Установка Docker Desktop на Ubuntu"
date: 2024-08-02T18:40:56+03:00
type: docs
---

## Установка DEB пакета Docker Desktop

### Требования

Для успешной установки Docker Desktop необходимо:

1. Соответствовать системным требованиям.
2. Иметь 64-битную версию Ubuntu Jammy Jellyfish 22.04 LTS или текущую non-LTS версию. Docker Desktop поддерживается на архитектуре x86_64 (или amd64).

**Примечание:** Последняя версия Ubuntu 24.04 LTS пока не поддерживается. Docker Desktop не сможет запуститься. Из-за изменения политики безопасности в Ubuntu, необходимо выполнить команду `sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0` хотя бы один раз. Подробнее можно прочитать в блоге Ubuntu.

Для не-Gnome окружений требуется установка gnome-terminal:

```bash
sudo apt install gnome-terminal
```

### Установка Docker Desktop

Рекомендуемый способ установки Docker Desktop на Ubuntu:

1. Настройте пакетный репозиторий Docker, следуя инструкциям из [документации](https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository).
2. Скачайте последний DEB пакет.
3. Установите пакет с помощью apt:

```bash
sudo apt-get update
sudo apt-get install ./docker-desktop-<arch>.deb
```

**Примечание:** В конце процесса установки apt может возникнуть ошибка из-за установки скачанного пакета. Эту ошибку можно игнорировать:

```
N: Download is performed unsandboxed as root, as file '/home/user/Downloads/docker-desktop.deb' couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)
```

После установки будет выполнено несколько пост-установочных настроек:

- Установка прав для Docker Desktop для работы с привилегированными портами и настройки ограничений ресурсов.
- Добавление DNS имени для Kubernetes в /etc/hosts.
- Создание символической ссылки от /usr/local/bin/com.docker.cli к /usr/bin/docker для обеспечения работы Docker CLI с облачной интеграцией.

### Запуск Docker Desktop

Для запуска Docker Desktop можно использовать меню приложений или команду в терминале:

```bash
systemctl —user start docker-desktop
```

При запуске Docker Desktop создается специальный контекст, который используется Docker CLI, чтобы избежать конфликта с локальным Docker Engine. При выключении Docker Desktop контекст восстанавливается до предыдущего состояния.

Docker Desktop устанавливает Docker Compose версии 2 и предоставляет возможность настроить его как docker-compose через панель настроек. Новая версия Docker CLI с облачными возможностями устанавливается в /usr/local/bin/com.docker.cli с созданием символической ссылки на классический Docker CLI.

Проверить версии установленных компонентов можно с помощью команд:

```bash
docker compose version
docker —version
docker version
```

Для автоматического запуска Docker Desktop при входе в систему:

1. Откройте настройки Docker и выберите "General" > "Start Docker Desktop when you sign in to your computer".
2. Или выполните команду:

```bash
systemctl —user enable docker-desktop
```

Для остановки Docker Desktop:

1. Выберите иконку Docker в меню и нажмите "Quit Docker Desktop".
2. Или выполните команду:

```bash
systemctl —user stop docker-desktop
```

### Обновление Docker Desktop

При выходе новой версии Docker Desktop в интерфейсе Docker появится уведомление. Для обновления необходимо скачать новый пакет и выполнить команду:

```bash
sudo apt-get install ./docker-desktop-<arch>.deb
```

**Заключение:** Установка Docker Desktop на Ubuntu проста и удобна, но требует выполнения определенных шагов для корректной работы. Надеемся, эта статья поможет вам в установке и использовании Docker Desktop.

