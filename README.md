[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?logo=docker&logoColor=white)](https://www.docker.com/)

# Навигация

- [Описание](#описание)
- [Настройка](#настройка)
- [Установка](#установка)
    - [Установка на сервере](#установка-на-сервере)
- [Обновление](#обновление)
    - [Обновление на сервере](#обновление-на-сервере)

# Описание

Конфигурация [Traefik](https://doc.traefik.io/traefik/) для удобного запуска нескольких сервисов на
одном сервере.

Особенности:

- Настроен автоматический редирект с `http` на `https`
- Через label `traefik.subdomain` можно задавать поддомен для контейнера
- Данные сертификатов хранятся в `lestsencrypt/acme.json`
- Логи доступа хранятся в `logs/access.log`

# Настройка

Настройка осуществляется через файл `.env`, доступны следующие параметры:

- `SERVER_HOST` - Хост сервера, значение указанное в `trarfik.subdomain` будет поддоменом этого
  хоста
- `ACME_EMAIL` - Почта на которую будут приходить уведомления от `letsencrypt`

# Установка

### Установка на сервере

    git clone
    cd traefik
    cp .env.template .env

Заполнить файл `.env`

    docker network create traefik
    docker-compose up --build -d 

# Обновление

### Обновление на сервере

    git pull
    docker-compose up --build -d

