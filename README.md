[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?logo=docker&logoColor=white)](https://www.docker.com/)

# Навигация

- [Описание](#описание)
- [Настройка](#настройка)
- [Установка](#установка)
    - [Установка на сервере](#установка-на-сервере)
    - [Установка в Swarm mode](#установка-в-swarm-mode)
- [Обновление](#обновление)
    - [Обновление на сервере](#обновление-на-сервере)
    - [Обновление в Swarm mode](#обновление-в-swarm-mode)

# Описание

Конфигурация [Traefik](https://doc.traefik.io/traefik/) для удобного запуска нескольких сервисов на
одном сервере. Поддерживается как обычный режим Docker Compose, так и Docker Swarm mode.

Особенности:

- Настроен автоматический редирект с `http` на `https`
- Через label `traefik.subdomain` можно задавать поддомен для контейнера
- Данные сертификатов хранятся в `lestsencrypt/acme.json`
- Логи доступа хранятся в `logs/access.log`
- Поддержка Docker Swarm mode для высокодоступных развертываний

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

### Установка в Swarm mode

Убедитесь, что Docker Swarm инициализирован:

    docker swarm init

Если вы добавляете новый узел в существующий кластер:

    docker swarm join --token <token> <manager-ip>:2377

Затем выполните развертывание:

    git clone
    cd traefik
    cp .env.template .env

Заполнить файл `.env` и экспортировать переменные окружения:

    export $(cat .env | xargs)
    docker stack deploy -c docker-compose.swarm.yml traefik

**Примечание:** Переменные окружения `SERVER_HOST` и `ACME_EMAIL` должны быть экспортированы перед развертыванием, так как `docker stack deploy` не читает файл `.env` автоматически.

Для проверки статуса:

    docker service ls
    docker service ps traefik_traefik

Для удаления стека:

    docker stack rm traefik

# Обновление

### Обновление на сервере

    git pull
    docker-compose up --build -d

### Обновление в Swarm mode

    git pull
    export $(cat .env | xargs)
    docker stack deploy -c docker-compose.swarm.yml traefik

Для принудительного обновления сервиса:

    docker service update --force traefik_traefik

Для просмотра логов:

    docker service logs traefik_traefik

Для масштабирования сервиса (если нужно несколько реплик):

    docker service scale traefik_traefik=3

export $(cat .env | xargs)
docker stack deploy -c docker-compose.swarm.yml traefik
docker service update --force traefik_traefik
