# Mihdan: Timeweb Sandbox

Песочница для тестирования своей сборки на основе [Wodby](https://wodby.com/) под WordPress на VDS от [Timeweb](https://www.kobzarev.com/r/timeweb/).

![](./assets/77fgBUhHuF0.jpg)

## Исходные данные (Guru)
- CPU - 2 x 2.4 ГГц
- RAM - 2 ГБ
- SSD - 40 ГБ
- ОС - Ubuntu 18.04

## Привязка тестового домена

Направим тестовый домен `leto-volovo.ru` на наш VDS с IP `188.225.58.119`

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

Переходим по адресу http://leto-volovo.ru:8000 и проходим стандартный процесс установки WordPress.
Плагины и темы можно ставить черех админку или WP-CLI через вызовы типа:

```
make wp plugin install woocommerce
make wp plugin activate woocommerce
```

В данной сборке установлены следующие плагины:

- [pj-object-cache-red](https://github.com/pressjitsu/pj-object-cache-red)
- [WP Rocket](https://wp-rocket.me/)
- [WooCommerce](https://wordpress.org/plugins/woocommerce/)

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

# Тестирование

Берем самые популярные сервисы по тестированию:

- [Measure](https://web.dev/measure)
- [GTmetrix](https://gtmetrix.com/reports/leto-volovo.ru/K01Gh7ul)
- [WebPageTest](https://www.webpagetest.org/result/190221_YM_07d76fee9d3bd4b5c27504affeb816fa/)
- [Loaddy](https://loaddy.com/result/527566779/)
- [Alertra](https://www.alertra.com/)

Генерим 100 тестовых записей через `WP-CLI`:

```
make wp post generate
```

## Measure
![](http://screenshot.kobzarev.com/2019-02-21-1550755101.png)

## GTmetrix

![](http://screenshot.kobzarev.com/2019-02-21-1550755135.png)

## WebPageTest

![](http://screenshot.kobzarev.com/2019-02-21-1550755192.png)

## Loaddy

![](http://screenshot.kobzarev.com/2019-02-21-1550754642.png)

## Alertra

![](http://screenshot.kobzarev.com/2019-02-21-1550754782.png)

