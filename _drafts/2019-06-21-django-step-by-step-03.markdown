---
layout: post
title:  "Tutorial paso a paso con Django Framework - Parte 3"
date:   2019-06-21 11:24:59 +0100
categories: django tutorial
---

Ahora crearemos una 'Home Page' para nuestro sitio web. 

Podemos optar una simple 'Hello World!' pero iremos un poco más allá he implementaremos una página 'Responsive' y los componentes necesarios.

Sin olvidar que seguiremos haciendo uso de *Test Driven Development* (TDD) para su desarrollo.

Lo ideal, para los tiempos que corren, sería hacer algo en plan Vue, React o Angular, pero claro, estos frameworks están orientados al consumo de interfaces API. Cuando veamos el Django Rest Framework (DRF), haremos un ejemplo.

Volviendo a nuestro tema, en este capítulo veremos:

* Ajustes de 'Static Files'
* Ajustes de 'Templates'
* 'Initializr: HTML5 Boilerplate and Twitter Bootstrap'
* Página incial con TDD - Test first
* Página incial con TDD - Code next
* Commit en nuestro respositorio y en el externo.

Static Files
------------

Primero debemos asegurarnos que tenemos esto en `base.py`:

```
INSTALLED_APPS = [
    ...
    'django.contrib.staticfiles',
]

STATIC_URL = '/static/'
```

Creamos la carpeta 'static' al mismo nivel que 'settings', editamos `base.py`

```
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static"),
)
```

Templates
---------

Hacemos algo parecido que antes quedando nuestro `base.py` así:

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, "templates")],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

Initilizr: HTML5 Boilerplate y Twitter Bootstrap
------------------------------------------------

Visitamos la página [http://www.initializr.com/](http://www.initializr.com/) en concreto nos descargamos [esto](http://www.initializr.com/builder?boot-hero&jquerymin&h5bp-iecond&h5bp-chromeframe&h5bp-analytics&h5bp-favicon&h5bp-appletouchicons&h5bp-robots&h5bp-humans&h5bp-404&modernizrrespond&izr-emptyscript&boot-css&boot-scripts)

[2019-06-21-django-step-by-step-03_initializr.png]

Una vez descargados hacemos lo siguiente:

* Movemos *index.html*, *404.html*, *humans.txt* y *robots.txt* a la carpeta *django_template_project/templates*
* Cambiamos el nombre de *index.html* por *base.html*. Más adelante crearemos la *index.html* heredando *base.html*.
* Movemos el resto a la carpeta *django_template_project/static*
* Reemplazamo el *favicon.ico* por el nuestro, si tenemos.
* Eliminamos *apple-touch-icon.png, browserconfig.xml, tile-wide.png* y *tile.png*.

Página incial con TDD - Test first
------------------------------------

Lo correcto hubiera sido hacer primero el test antes de configurar los ajustes de 'static' y 'templates', pero bueno, tampoco está tan mal ajustar el entorno de trabajo como paso previo.

Un par de cambios

`touch functional_tests/__init__.py`
`git mv functional_tests/all_users.py functional_tests/test_all_users.py`

Con esto conseguimos que `python manage.py test functional_tests` funcione.

Editamos 'functional_all_users.py' para dejarlo como sigue:

```
# -*- coding: utf-8 -*-
from selenium import webdriver
from django.core.urlresolvers import reverse
from django.contrib.staticfiles.testing import LiveServerTestCase  
 
 
class HomeNewVisitorTest(LiveServerTestCase): 
 
    def setUp(self):
        self.browser = webdriver.Firefox()
        self.browser.implicitly_wait(3)
 
    def tearDown(self):
        self.browser.quit()
 
    def get_full_url(self, namespace):
        return self.live_server_url + reverse(namespace)
 
    def test_home_title(self):
        self.browser.get(self.get_full_url("home"))
        self.assertIn("TaskBuster", self.browser.title)
 
    def test_h1_css(self):
        self.browser.get(self.get_full_url("home"))
        h1 = self.browser.find_element_by_tag_name("h1")
        self.assertEqual(h1.value_of_css_property("color"), 
                         "rgba(200, 50, 255, 1)")
```

Reemplazmoas 'unittest' por 'LiveServerTestCase' para no hacer persistentes los cambios en la bbdd de pruebas en cada ejecución y no tener que tener otra ventana con el 'runserver' ejecutándose.

Página incial con TDD - Code next
---------------------------------

Arreglamos el código para que los test funcionen:

'views.py'

```
# -*- coding: utf-8 -*-
from django.shortcuts import render
 
def home(request):
    return render(request, "core/index.html", {})
```

'urls.py'

```
from .views import home

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', home, name='home')
]
```

Hay que destacar que podemos tomar los *test* desde dos puntos de vista: usuario final y desarrollador. En este último caso, sería interesante saber si un fichero 'usa' una plantilla o no.

'test.py'

```
# -*- coding: utf-8 -*-
from django.test import TestCase
from django.core.urlresolvers import reverse
 
 
class TestHomePage(TestCase):
 
    def test_uses_index_template(self):
        response = self.client.get(reverse("home"))
        self.assertTemplateUsed(response, "taskbuster/index.html")
 
    def test_uses_base_template(self):
        response = self.client.get(reverse("home"))
        self.assertTemplateUsed(response, "base.html")
```

Ejecutamos: `python manage.py django_template_project.test`

Con respecto a las plantillas, en *base.html* incluimos:

```html
<head>
    ...
    <title>{% block head_title %}{% endblock %}</title>
    ...
</head>
```

y creamos *index.html* en *django_template_project/templates/core* con el siguiente contenido:

```
{% extends "base.html" %}
{% block head_title %}TaskBuster Django Tutorial{% endblock %}
```

`python manage.py test django_template_project.test`

```
(env) felix@felix-XPS13-9333:~/dev/python/django/django_template_project$ python manage.py test django_template_project.test
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
..
----------------------------------------------------------------------
Ran 2 tests in 0.008s

OK
Destroying test database for alias 'default'...
```

Pero que pasa con los functional test?

```
(env) felix@felix-XPS13-9333:~/dev/python/django/django_template_project$ python manage.py test functional_tests
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
F.
======================================================================
FAIL: test_h1_css (functional_tests.test_all_users.NewVisitorTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/home/felix/dev/python/django/django_template_project/functional_tests/test_all_users.py", line 27, in test_h1_css
    "rgba(200, 50, 255, 1)")
AssertionError: 'rgb(0, 0, 0)' != 'rgba(200, 50, 255, 1)'
- rgb(0, 0, 0)
+ rgba(200, 50, 255, 1)


----------------------------------------------------------------------
Ran 2 tests in 8.466s

FAILED (failures=1)
Destroying test database for alias 'default'...
```

Como ahora tenemos un sistema configurado con ficheros estáticos, el LiveServerTestCase' no nos vale, en su lugar pondremos 'StaticLiveServerTestCase'.

Con el *webdriver* de Chrome 

```
(env) felix@felix-XPS13-9333:~/dev/python/django/django_template_project$ python manage.py test functional_tests
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
..
----------------------------------------------------------------------
Ran 2 tests in 4.473s

OK
Destroying test database for alias 'default'...
```

Mientras que con el de *Firefox*

```
(env) felix@felix-XPS13-9333:~/dev/python/django/django_template_project$ python manage.py test functional_tests
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
F.
======================================================================
FAIL: test_h1_css (functional_tests.test_all_users.NewVisitorTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/home/felix/dev/python/django/django_template_project/functional_tests/test_all_users.py", line 38, in test_h1_css
    "rgba(200, 50, 255, 1)"
AssertionError: 'rgb(200, 50, 255)' != 'rgba(200, 50, 255, 1)'
- rgb(200, 50, 255)
+ rgba(200, 50, 255, 1)
?    +             +++


----------------------------------------------------------------------
Ran 2 tests in 8.144s

FAILED (failures=1)
Destroying test database for alias 'default'...
``` 

Habrá que investigar...


