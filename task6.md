1. Dockerfile для Backend (репозиторий dapi)
Создайте файл Dockerfile в корне репозитория с бэкендом:
Dockerfile

FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8081
# Предполагаем, что приложение слушает порт, заданный в коде или конфиге
CMD ["npm", "start"]

2. Dockerfile для Frontend (репозиторий dwUI)
Создайте файл Dockerfile в корне репозитория с фронтендом:
Dockerfile

FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD ["npm", "start"]

3. Docker Compose (основной репозиторий, папка task6/docker-compose.yml)

В вашем основном репозитории создайте структуру task6/docker-compose.yml:
YAML

version: '3.8'

services:
  backend:
    build:
      context: ../dapi # Укажите путь к локальной папке склонированного бэкенда
    ports:
      - "8081:8081"
    networks:
      - app-network

  frontend:
    build:
      context: ../dwUI # Укажите путь к локальной папке склонированного фронтенда
    ports:
      - "8080:8080"
    networks:
      - app-network
    depends_on:
      - backend

networks:
  app-network:
    driver: bridge
