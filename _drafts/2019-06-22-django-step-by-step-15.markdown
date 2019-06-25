---
layout: post
title: "Tutorial paso a paso con Django Framework - Parte 12 (deployment)"
subtitle: "Here you'll find all the ways to get in touch with me"
date:   2019-06-22 11:24:59 +0100
categories: django tutorial
---

## Git: Entorno en 192.168.20.X

```bash
$ sudo apt update
$ sudo apt-get install binutils libproj-dev gdal-bin
$ mkdir /servers
$ cd /servers
$ git clone --branch development http://gitlab.grafcan.es/idecanarias/stats.git sitcanapi_project
```

### Importante, permisos

```bash
$ sudo chown -R fherdom:www-data /servers
$ sudo chmod -R u=rwX,g=rX,o= /servers
```

```
$ cd sitcanapi_project
$ python3 -m venv env
$ source env/bin/activate
(env)$ pip install -r requirements/production.txt
(env)$ python manage.py runserver 0:8000
```

Test http://192.168.20.65:8000

# uWSGI

```
(env)$  uwsgi --module=sitcanapi_project.wsgi:application \
--master --pidfile=/tmp/sitcanapi.pid --http=0.0.0.0:49152 \
--processes=5 --max-requests=5000 --vacuum
```

Test http://192.168.20.65:49152

Lo pasamos a un fichero **sitcanapi_project.ini**

```ini
[uwsgi]
pidfile=/tmp/sitcanapi_project.pid
chdir=/servers/sitcanapi_project
module=sitcanapi_project.wsgi:application
env DJANGO_SETTINGS_MODULE=sitcanapi_project.settings.development
master=True
http=:49153
# socket=/servers/sitcanapi_project/site.sock
chmod-socket=664
processes=4
threads=2
max-requests=5000
vacuum=true
logger=syslog:sitcanapi_project
```

y probamos con: `uwsgi --ini sitcanapi_project.ini`.

Esta configuración pueder se mejorable. Fíjese que escribimos en `syslog`.

Test [http://192.168.20.65:49152](http://192.168.20.65:49152)

# systemD

`sudo vim /etc/systemd/system/sitcanapi_project.service`

```ini
[Unit]
Description=uWSGI sitcanapi_project
After=network.target

[Service]
User=fherdom
Group=www-data
WorkingDirectory=/servers/sitcanapi_project
Environment="PATH=/servers/sitcanapi_project/env/bin"
ExecStart=/servers/sitcanapi_project/env/bin/uwsgi --ini sitcanapi_project.ini
ExecStop= /servers/sitcanapi_project/env/bin/uwsgi --stop /tmp/sitcanapi_project.pid

[Install]
WantedBy=multi-user.target
```

* arrancamos el servicio con: `sudo systemctl start sitcanapi_project`
* comprobamos el servicio con: `sudo systemctl status sitcanapi_project`
* lo habilitamos para su inicio en el arranque (boot): `sudo systemctl enable sitcanapi_project`

Referencia: https://django-best-practices.readthedocs.io/en/latest/deployment/servers.html


# Nginx

Establecemos el canal de comunicación a nivel **socket** en lugar de puerto http.

```ini
[uwsgi]
pidfile=/tmp/sitcanapi_project.pid
chdir=/servers/sitcanapi_project
module=sitcanapi_project.wsgi:application
env DJANGO_SETTINGS_MODULE=sitcanapi_project.settings.development
master=True
# http=:49153
socket=/servers/sitcanapi_project/site.sock
chmod-socket=664
processes=4
threads=2
max-requests=5000
vacuum=true
logger=syslog:sitcanapi_project
```

```bash
$ sudo systemctl stop sitcanapi_project
$ sudo systemctl start sitcanapi_project
```

Configuramos el nginx

`sudo vim /etc/nginx/sites-available/sitcanapi_project`

```nginx
server {
    listen 80;
    server_name sitcanapi.idecanarias.es;
    charset utf-8;

    location /media  {
        alias /servers/sitcanapi_project/media;
    }

    location /static {
        alias /servers/sitcanapi_project/assets;
    }

    location / {
        uwsgi_pass unix:///servers/sitcanapi_project/site.sock;
        include uwsgi_params;
    }
}
```

* habilitamos a sites-enabled: 
`sudo ln -s /etc/nginx/sites-available/sitcanapi /etc/nginx/sites-enabled`
* probamos la configuración: 
`sudo nginx -t` y si fue todo bien `sudo service nginx restart`
* agregamos a nuestro `/etc/hosts` la siguiente línea: `192.168.20.65 sitcanapi.idecanarias.es` 

Test http://sitcanapi.idecanarias.es/admin