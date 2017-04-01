# dockrix

Идея в том, чтобы создать настраиваемое рабочее окружение для разработки под 1С-Битрикс. Настройки окружения задаются в .env-файле:

```env
# Project settings
# Если разработка ведется локально, и хочется обращаться к проекту по доменному имени, а не по айпи - то стоит задать домен
# Не забываем добавлять домен в /etc/hosts. ip-адрес для домена обычно соответствует значению NGINX_IP
PROJECT_DOMAIN=project.dock 
# Путь к директории сайта. Эта директория будет проброшена с помощью volumes в контейнеры. Если директория с сайтом находится в другом месте - можно указать абсолютный путь до нее, например PROJECT_PATH=/home/user/projects/site.ru
PROJECT_PATH=./www
# СОбственно директория с docker-файлами контейнеров. В ней же будет располагаться проброшенная директория с логами nginx и php, а также с базой данных сайта. По умолчанию это .docker в той же директории, что и docker-compose.yml
DOCKRIXDIR=./.docker

# Network settings
# Настройки сети для контейнеров.
# Подсеть, обычно хватает стандартной подсети на 127 ip-адресов, можно сделать больше (указывая вместо 24 например 16 или 8 на конце после слэша) или меньше адресов (указывая например 25 или 27 на конце)
SUBNET=172.172.172.0/24
# Шлюз
GATEWAY=172.172.172.1
# Далее идут ip-адреса контейнеров
NGINX_IP=172.172.172.2
PHP_IP=172.172.172.3
MYSQL_IP=172.172.172.4
MEMCACHED_IP=172.172.172.5

# Services versions
# Тут нужно указывать версии требуемого ПО в контейнерах.
# ВАЖНО: для указанной версии должен существовать соответствующий dockerfile. Если вам нужна какая-то версия, которая еще не включена в репозитории - можете сделать докерфайл по аналогии или взять его с registry.docker.com
# Например, докерфайл для nginx хранится в $DOCKRIXDIR/nginx/nginx-$NGINX_VERSION.dock
NGINX_VERSION=latest
PHP_VERSION=5.6
MYSQL_VERSION=5.6
MEMCACHED_VERSION=latest

# Port forwarding
# Проброс портов на хост-машину. Указывается в формате <SERVICE_HOST_PORT>:<SERVICE_CONTAINER_PORT>.
# Довольно удобно, если вы хотите иметь доступ с комьпьютера напрямую к сайту, не заморачиваясь с ip-адресами. В приведенном примере, можно будет обращаться к сайту по адресу localhost:8172.
# Также это полезно, если вы хотите дать доступ к локальному проекту извне, например показать заказчику. В этом случае вы просто даете клиенту адрес вида <ваш внешний ip хост-машины>:<NGINX_HOST_PORT>
NGINX_PORTS=8172:80
MYSQL_PORTS=3306:3306
MEMCACHED_PORTS=11211:11211

# MYSQL
# Настройки подключения к mysql. При создании контейнера будет создана база данных с указанным названием и правами доступа для указанного пользователя. Дополнительно можно указать пароль, который будет у рута.
MYSQL_DATABASE=project
MYSQL_USER=project
MYSQL_PASSWORD=project
MYSQL_ROOT_PASSWORD=root
```
