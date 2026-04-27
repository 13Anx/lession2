#!/bin/bash

# Проверяем, передан ли аргумент
if [ -z "$1" ]; then
    echo "Использование: $0 <url>"
    exit 1
fi

TARGET_URL=$1

# Выполняем запрос с помощью curl
# --silent скрывает прогресс-бар
# --head запрашивает только заголовки (быстрее)
# --fail заставляет curl возвращать ненулевой код при ошибках HTTP (4xx, 5xx)
# --max-time 10 ограничивает время ожидания 10 секундами

if curl --silent --head --fail --max-time 10 "$TARGET_URL" > /dev/null; then
    echo "Ресурс $TARGET_URL доступен."
    exit 0
else
    echo "Ошибка: Ресурс $TARGET_URL недоступен."
    exit 1
fi
