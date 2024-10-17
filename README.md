# Docker Prune Systemctl Timer

This repository contains a `systemd` timer configuration to automatically clean up unused Docker resources (volumes, networks, images, and stopped containers) on a regular basis using the `docker system prune` command. The goal is to help manage Docker environments by preventing excessive disk space usage.

## Features
- Automatic execution of `docker system prune` on a schedule.
- Timer-based execution via systemd.
- Easy installation and setup.
- Customizable schedule.

## Prerequisites
- Linux-based system with `systemd`.
- Docker installed on your machine.

## Installation 

1. **Clone the repository:**
   ```bash
   git clone https://github.com/1thenikita/docker-prune-systemctl-timer.git
   cd docker-prune-systemctl-timer
   ```

2. **Copy the systemd service and timer files to the system:**
   ```bash
   sudo cp docker-prune.service /etc/systemd/system/
   sudo cp docker-prune.timer /etc/systemd/system/
   ```

3. **Enable and start the timer:**
   ```bash
   sudo systemctl enable docker-prune.timer
   sudo systemctl start docker-prune.timer
   ```

4. **Check the status:**
   To confirm that the timer is active:
   ```bash
   sudo systemctl status docker-prune.timer
   ```

5. **Logs:**
   You can check the logs to see when `docker system prune` was last executed:
   ```bash
   journalctl -u docker-prune.service
   ```

## Customization

You can modify the schedule by editing the `docker-prune.timer` file. The schedule follows the systemd time syntax, so feel free to adjust it according to your needs.

Example: To run the prune every day at 2 AM, you can adjust the `OnCalendar` value:
```ini
[Timer]
OnCalendar=*-*-* 02:00:00
```

or, as the author uses prune every day and persistant:
```ini
[Timer]
OnCalendar=daily
Persistent=true
```

After modifying the timer, reload systemd and restart the timer:
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker-prune.timer
```

## Uninstallation

To remove the timer and service:
```bash
sudo systemctl stop docker-prune.timer
sudo systemctl disable docker-prune.timer
sudo rm /etc/systemd/system/docker-prune.service
sudo rm /etc/systemd/system/docker-prune.timer
sudo systemctl daemon-reload
```

---

### Перевод на русский

# Таймер Systemctl для очистки Docker

Этот репозиторий содержит конфигурацию таймера systemd для автоматической очистки неиспользуемых ресурсов Docker (томов, сетей, образов и остановленных контейнеров) на регулярной основе с помощью команды `docker system prune`. Цель - помочь управлять средой Docker, предотвращая чрезмерное использование дискового пространства.

## Возможности
- Автоматическое выполнение `docker system prune` по расписанию.
- Выполнение через таймер systemd.
- Простая установка и настройка.
- Настраиваемое расписание.

## Требования
- Система на базе Linux с `systemd`.
- Установленный Docker.

## Установка

1. **Клонируйте репозиторий:**
   ```bash
   git clone https://github.com/1thenikita/docker-prune-systemctl-timer.git
   cd docker-prune-systemctl-timer
   ```

2. **Скопируйте файлы таймера и сервиса systemd в систему:**
   ```bash
   sudo cp docker-prune.service /etc/systemd/system/
   sudo cp docker-prune.timer /etc/systemd/system/
   ```

3. **Включите и запустите таймер:**
   ```bash
   sudo systemctl enable docker-prune.timer
   sudo systemctl start docker-prune.timer
   ```

4. **Проверьте статус:**
   Чтобы убедиться, что таймер активен:
   ```bash
   sudo systemctl status docker-prune.timer
   ```

5. **Логи:**
   Вы можете проверить логи, чтобы узнать, когда `docker system prune` был выполнен в последний раз:
   ```bash
   journalctl -u docker-prune.service
   ```

## Настройка

Вы можете изменить расписание, отредактировав файл `docker-prune.timer`. Расписание следует синтаксису времени systemd, поэтому смело настраивайте его в соответствии с вашими потребностями.

Пример: чтобы запускать очистку каждый день в 2 часа ночи, измените значение `OnCalendar`:
```ini
[Timer]
OnCalendar=*-*-* 02:00:00
```

или, как использует автор очистку каждый день и постоянно:
```ini
[Timer]
OnCalendar=daily
Persistent=true
```

После изменения таймера перезагрузите systemd и перезапустите таймер:
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker-prune.timer
```

## Удаление

Для удаления таймера и сервиса:
```bash
sudo systemctl stop docker-prune.timer
sudo systemctl disable docker-prune.timer
sudo rm /etc/systemd/system/docker-prune.service
sudo rm /etc/systemd/system/docker-prune.timer
sudo systemctl daemon-reload
```