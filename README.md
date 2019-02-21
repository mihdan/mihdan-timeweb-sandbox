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

Сразу после запуска получим работающий сайт и домены

Service | Domain
--------|-------
nginx/apache | http://leto-volovo.ru:8000
pma	| http://pma.leto-volovo.ru:8000
adminer	| http://adminer.leto-volovo.ru:8000
mailhog	| http://mailhog.leto-volovo.ru:8000
varnish	| http://varnish.leto-volovo.ru:8000
portainer	| http://portainer.leto-volovo.ru:8000
webgrind | http://webgrind.leto-volovo.ru:8000
