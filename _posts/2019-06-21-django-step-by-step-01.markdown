---
layout: post
title:  "Tutorial paso a paso con Django Framework - Parte 1"
date:   2019-06-21 11:24:59 +0100
categories: django tutorial
---

Crear un buen entorno de trabajo es muy importante porque ganamos en productividad. Es verdad que configurarlo nos llevará un poco de tiempo, tiempo que ganaremos en el futuro.

Los entornos virtuales nos ayudan a organizar las dependencias de cada proyecto y un buen editor nos ayuda a programar, dándonos ayuda sobré los métodos disponibles de un objeto o resaltar la sintaxis para una mejor comprensión del código.

* [Creación del entorno de trabajo](#creacion-del-entorno-de-trabajo)
* Instalación de Django 2.2.2 (LTS)
* Trabajar con Sublime Text
* [Obedece a la cabra](http://www.obeythetestinggoat.com/)
* Crear el proyecto Django
* Arrancar el servidor de desarrollo.

### Creación del entorno de trabajo

Para empezar el tutorial necesitamos **Python3** y el gestor de paquetes **pip**.

También necesitaremos un editor. Personalemente recomiendo *SublimeText* pero pueden utilizar el que más les guste (incluso *vim*). Aquí hay una guía para configurar un proyecto en este [SublimeText][1].

```bash
cd dev/python/django/django_template_project
python3 -m venv env`
source env/bin/activate
(env) felix@felix-XPS13-9333:~/dev/python/django/django_template_project$

```

Con esto crearemos una subcarpeta `env` que contendrá el intérprete de `Python3` y los paquetes que posteriormente instalaremos mediante `pip`.

Repetiremos el comando para crear un entorno alternativo que simulará la puesta en *Producción* de nuestro proyecto.

`python3 -m venv env_prod`


### Instalación de Django 2.2.2 (LTS)

```
source env/bin/activate
pip install Django==2.2..2
```


### Trabajar con Sublime Text

```
cd ..
subl django_template_project
```

Instalaremos el paquete 'virtualenv' de Sublime y lo activamos: Virtualenv: Activate, seleccionando la carpeta correspondiente.

![subl]({{ "/assets/img/posts/2019-06-21-django-step-by-step-01_subl.png" | relative_url }}){:class="img-responsive"}

### [Obedece a la cabra](http://www.obeythetestinggoat.com/)

### Crear el proyecto Django

### Arrancar el servidor de desarrollo.


[1]: http://www.marinamele.com/2014/03/install-and-configure-sublime-text-3.html
