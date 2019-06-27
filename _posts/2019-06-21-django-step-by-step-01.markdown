---
layout: post
title:  "Tutorial paso a paso con Django Framework - Parte 1"
date:   2019-06-21 11:24:59 +0100
categories: django tutorial
---

Crear un buen entorno de trabajo es muy importante porque ganamos en productividad. Es verdad que configurarlo nos llevará un poco de tiempo, tiempo que ganaremos en el futuro.

Los entornos virtuales nos ayudan a organizar las dependencias de cada proyecto y un buen editor nos ayuda a programar, dándonos ayuda sobré los métodos disponibles de un objeto o resaltar la sintaxis para una mejor comprensión del código.

Pasos
-----

* [Creación del entorno de trabajo](#creacion-del-entorno-de-trabajo)
* Instalación de Django 2.2.2 (LTS)
* Trabajar con Sublime Text
* [Obedece a la cabra](http://www.obeythetestinggoat.com/)
* Crear el proyecto Django
* Arrancar el servidor de desarrollo.

Creación del entorno de trabajo
-------------------------------

Para empezar el tutorial necesitamos **Python3** y el gestor de paquetes **pip**.

También necesitaremos un editor. Personalemente recomiendo *SublimeText* pero pueden utilizar el que más les guste (incluso *vim*). Aquí hay una guía para configurar un proyecto en este [SublimeText][1].

```bash
$ cd dev/python/django/django_template_project
$ python3 -m venv env
$ source env/bin/activate
(env)$ 
```

Con esto crearemos una subcarpeta `env` que contendrá el intérprete de `Python3` y los paquetes que posteriormente instalaremos mediante `pip`.

Repetiremos el comando para crear un entorno alternativo que simulará la puesta en *Producción* de nuestro proyecto.

```
(env)$ deactivate
$
$ python3 -m venv env_prod
(env_prod)$
```

Instalación de Django 2.2.2 (LTS)
---------------------------------

```
(env_prod)$ deactivate
$ source env/bin/activate
(env)$ pip install Django==2.2.2
```


#### Trabajar con Sublime Text


```
(env)$ cd ..
(env)$ subl django_template_project
```

Instalaremos el paquete 'virtualenv' de Sublime y lo activamos: Virtualenv: Activate, seleccionando la carpeta correspondiente.

![subl]({{ "/assets/img/posts/2019-06-21-django-step-by-step-01_subl.png" | relative_url }}){:class="img-responsive"}

### [Obedece a la cabra](http://www.obeythetestinggoat.com/)

>
Escribe el test antes de escribir el código.
>

Crearemos un test que comprueba el título de la página de bienvenida cuando arrancamos el servidor de desarrollo para nuestro nuevo proyecto Django.

Activamos el entorno de pruebas

```bash
$ source env/bin/activate
(env)$ pip install --upgrade selenium
```

```bash
(env)$ mkdir functional_tests
(env)$ touch functional_tests/all_users.py
(env)$ vim functional_tests/all_users.py 
```

{% highlight python%}
from selenium import webdriver
import unittest
 
 
class NewVisitorTest(unittest.TestCase):
 
    def setUp(self):
        self.browser = webdriver.Firefox()
        self.browser.implicitly_wait(3)
 
    def tearDown(self):
        self.browser.quit()
 
    def test_it_worked(self):
        self.browser.get('http://localhost:8000')
        self.assertIn(
            'Django: the Web framework for perfectionists with deadlines.',
            self.browser.title
        )

 
if __name__ == '__main__':
    unittest.main(warnings='ignore')
{% endhighlight %}


`(env)$ python functional_tests/all_users.py`

Si nos aparece este error

`FileNotFoundError: [Errno 2] No such file or directory: 'geckodriver': 'geckodriver'`

es porque tenemos que instalar del driver de Firefox para **selenium**. Nos vamos a [https://github.com/mozilla/geckodriver/releases](https://github.com/mozilla/geckodriver/releases) y descargamos la versión según nuestro sistema operativo. En mi caso, utilizaré [geckodriver-v0.24.0-linux64.tar.gz](https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-linux64.tar.gz)

Lo descomprimimos y creamos un enlace simbólico en la carpeta `/usr/local/bin`. Es decir,

`sudo ln -s /home/felix/apps/selenium/geckodriver-v0.24.0-linux64/geckodriver /usr/local/bin/`

Volvemos a lanzar el test y saldrá

`ERROR: test_it_worked (__main__.NewVisitorTest)`

debido a que no tenemos en ejecución el proyecto de *Django*

Crear el proyecto Django
------------------------

```
(env)$ pwd
-> /home/fherdom/dev/python/django/django_template_project
(env)$ django-admin.py startproject django_template_project .
```

```
(env) $ tree -I 'env*|__pycache__'
.
├── db.sqlite3
├── django_template_project
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── django-template-project.sublime-project
├── django-template-project.sublime-workspace
├── functional_tests
│   └── all_users.py
├── geckodriver.log
└── manage.py
```


Arrancar el servidor de desarrollo
----------------------------------

```
(env)$ python manage.py runserver
```

Abrimos otra sesión de 'terminal' y probamos el test. Ahora si vemos un resultado correcto.

```bash
(env) $ python functional_tests/all_users.py 
.
----------------------------------------------------------------------
Ran 1 test in 3.343s

OK
```

[1]: http://www.marinamele.com/2014/03/install-and-configure-sublime-text-3.html
