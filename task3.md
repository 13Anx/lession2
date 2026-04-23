# Инструкция по выполнению задания

##1. Подготовка окружения и создание скрипта
Сначала необходимо создать директорию и сам скрипт.

## Создаем директорию
sudo mkdir -p /opt/app
sudo chown $USER:$USER /opt/app

# Создаем файл скрипта
nano /opt/app/logger.sh

## Вставьте в /opt/app/logger.sh следующее содержимое:
#!/bin/bash

# Создаем файл лога, если его нет
touch /opt/app/log.txt

while true
do
  # Генерируем случайную строку длиной до 20 символов (буквы и цифры)
  RANDOM_STR=$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w $((1 + RANDOM % 20)) | head -n 1)
  
  # Записываем в файл
  echo "$RANDOM_STR" >> /opt/app/log.txt
  
  # Ожидаем 17 секунд
  sleep 17
done

Сделайте скрипт исполняемым:

chmod +x /opt/app/logger.sh

## 2. Настройка автозагрузки (Systemd)

Чтобы скрипт запускался автоматически, создадим системный сервис.
sudo nano /etc/systemd/system/log-gen.service

### Содержимое файла:
Ini/TOML

[Unit]
Description=Random String Logger Service
After=network.target

[Service]
ExecStart=/opt/app/logger.sh
Restart=always
User=root

[Install]
WantedBy=multi-user.target

### Активируйте и запустите сервис:

sudo systemctl daemon-reload
sudo systemctl enable log-gen.service
sudo systemctl start log-gen.service

3. Настройка ротации логов (logrotate)
Чтобы файл log.txt не занимал все свободное место, настроим logrotate.

sudo nano /etc/logrotate.d/app-logger

### Содержимое файла:

/opt/app/log.txt {
    daily
    rotate 7
    compress
    missingok
    notifempty
    copytruncate
}

#### Примечание: copytruncate важен, так как наш скрипт постоянно держит файл открытым для записи.

## 4. Проверка работы
Проверить статус работы скрипта:

sudo systemctl status log-gen.service

### Проверить наполнение файла:
tail -f /opt/app/log.txt
