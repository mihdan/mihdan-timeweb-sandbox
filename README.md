# mihdan-timeweb-sandbox

## Направим тестовый домен (leto-volovo.ru) на наш VDS

Идём в список своих доменов https://hosting.timeweb.ru/domains 

![](http://screenshot.kobzarev.com/2019-02-21-1550736833.png)

и меняем A-запись у выбранного домена на IP VDS.

![](http://screenshot.kobzarev.com/2019-02-21-1550736875.png)


## Ставим все на VDS

Соединяемся с сервером по SSH:

```
ssh root@188.225.58.119
```

Устанавливаем софт

```
apt install git docker docker-compose make
```

Клонируем докер образ WordPress

```
cd /srv
git clone https://github.com/wodby/docker4wordpress.git
cd docker4wordpress
```

Настраиваем окружение

```
nano .env
```

Меняем домен в `BASE_URL` на leto-volovo.ru

```
PROJECT_BASE_URL=leto-volovo.ru
```

Запускаем 

```
docker-compose up -d
```

## Домены

Сразу после запуска получим работающий сайт и домены

| Service   | Domain                               |
| --------- | ------------------------------------ |
| nginx     | http://leto-volovo.ru:8000           |
| pma	      | http://pma.leto-volovo.ru:8000       |
| adminer	  | http://adminer.leto-volovo.ru:8000   |
| mailhog	  | http://mailhog.leto-volovo.ru:8000   |
| varnish	  | http://varnish.leto-volovo.ru:8000   |
| portainer	| http://portainer.leto-volovo.ru:8000 |
| webgrind  | http://webgrind.leto-volovo.ru:8000  |

## Стэк

WordPress стэк состоит из контейнеров:

| Container       | Versions            | Service name    | Image                              | Default |
| -------------   | ------------------  | ------------    | ---------------------------------- | ------- |
| [Nginx]         | 1.15, 1.14          | `nginx`         | [wodby/nginx]                      | ✓       |
| [Apache]        | 2.4                 | `apache`        | [wodby/apache]                     |         |
| [WordPress]     | 5                   | `php`           | [wodby/wordpress]                  | ✓       |
| [PHP]           | 7.3, 7.2, 7.1, 5.6* | `php`           | [wodby/wordpress-php]              |         |
| [MariaDB]       | 10.3, 10.2, 10.1    | `mariadb`       | [wodby/mariadb]                    | ✓       |
| [PostgreSQL]    | 11, 10, 9.x         | `postgres`      | [wodby/postgres]                   |         |
| [Redis]         | 5, 4                | `redis`         | [wodby/redis]                      |         |
| [Memcached]     | 1                   | `memcached`     | [wodby/memcached]                  |         |
| [Varnish]       | 6.0, 4.1            | `varnish`       | [wodby/varnish]                    |         |
| [Node.js]       | 10, 8, 6            | `node`          | [wodby/node]                       |         |
| [Solr]          | 7.x, 6.6, 5.5       | `solr`          | [wodby/solr]                       |         |
| [Elasticsearch] | 6.x, 5.6, 5.5, 5.4  | `elasticsearch` | [wodby/elasticsearch]              |         |
| [Kibana]        | 6.x, 5.6, 5.5, 5.4  | `kibana`        | [wodby/kibana]                     |         |
| [AthenaPDF]     | 2.10.0              | `athenapdf`     | [arachnysdocker/athenapdf-service] |         |
| [Mailhog]       | latest              | `mailhog`       | [mailhog/mailhog]                  | ✓       |
| [OpenSMTPD]     | 6.0                 | `opensmtpd`     | [wodby/opensmtpd]                  |         |
| [Rsyslog]       | latest              | `rsyslog`       | [wodby/rsyslog]                    |         |
| [Blackfire]     | latest              | `blackfire`     | [blackfire/blackfire]              |         |
| [Webgrind]      | 1.5                 | `webgrind`      | [wodby/webgrind]                   |         |
| [XHProf viewer] | latest              | `xhprof`        | [wodby/xhprof]                     |         |
| Adminer         | 4.6                 | `pma`           | [wodby/adminer]                    |         |
| phpMyAdmin      | latest              | `pma`           | [phpmyadmin/phpmyadmin]            |         |
| Portainer       | latest              | `portainer`     | [portainer/portainer]              | ✓       |
| Traefik         | latest              | `traefik`       | [_/traefik]                        | ✓       |
