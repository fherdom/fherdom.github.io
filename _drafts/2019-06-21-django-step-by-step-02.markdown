---
layout: post
title:  "Tutorial paso a paso con Django Framework - Parte 2"
date:   2019-06-21 11:24:59 +0100
categories: django tutorial
---

Ahora configuraremos los entornos virtuales creados anteriormente.

Eliminaremos la `'SECRET_KEY'` para mantenerla segura.

Creamos un repositorio de datos para el versionado.


* Entornos virtuales y ficheros de requisitos
* Diferentes `settings.py` para cada entorno
* `Settings.py` del entorno de producción con `Debug=False`
* Seguridad de la `SECRET_KEY`
* Inicializar el repositorio Git y hacer el primer `commit` 
* Subir el projecto a GitHub


Entornos virtuales y ficheros de requisitos
-------------------------------------------

`pip freeze > requirements.txt`

```bash
mkdir requirements
touch requirements/{base.txt,development.txt,production.txt,testing.txt}
```

```
cd requirements
echo "Django==2.2.2" >> base.txt
echo "-r base.txt" | tee -a development.txt testing.txt production.txt
```

`echo "selenium==3.141.0" >> testing.txt`

```
source env/bin/activate
pip install -r requirements/development.txt

source env_pro/bin/activate
pip install -r requirements/production.txt
```

Diferentes `settings.py` para cada entorno
------------------------------------------

```
mkdir django_template_project/settings
cd django_template_project/settings
touch __init__.py development.py testing.py production.py staging.py
```

Editar `development.py testing.py production.py staging.py`

```python
# -*- coding: utf-8 -*-
from .base import *
```

`mv ../settings.py base.py`


Para establecer una configuración para cada entorno podemos editar el fichero `env/bin/activate` y `env_prod/bin/activate` y hacer estos cambios:

`export DJANGO_SETTINGS_MODULE="django_template_project.settings.development|production"` 

y en la función `deactivate`:

`unset DJANGO_SETTINGS_MODULE`

```
source env/bin/activate
python manage.py runserver

Django version 2.2.2, using settings 'django_template_project.settings.development'
```

```
deactivate
source env_prod/bin/activate
python manage.py runserver

Django version 2.2.2, using settings 'django_template_project.settings.production'
```

Probamos el sitio

`python manage.py runserver`

y en otra ventana

`python functional_tests/all_users.py`

y todo estará correcto!

`Settings.py` del entorno de producción con `Debug=False`
---------------------------------------------------------

Cortar la variable `DEBUG` y copiarla en `development.py|testing.py`

`DEBUG = True`

En `production.py` establecer a `False`

Seguridad de la `SECRET_KEY`
----------------------------

Editar de nuevo los ficheros `env/bin/activate`, como hicimos anteriormente y colocar:

`export SECRET_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"`

y su correspondiente 

`unset SECRET_KEY`

luego editamos el fichero  `base.py` y pegamos este código:

```python
from django.core.exceptions import ImproperlyConfigured


# SECURITY WARNING: keep the secret key used in production secret! 
def get_env_variable(var_name):
    try:
        return os.environ[var_name]
    except KeyError:
        error_msg = "Set the %s environment variable" % var_name
        raise ImproperlyConfigured(error_msg)
 
SECRET_KEY = get_env_variable('SECRET_KEY')
```

Reactivamos el entorno y probamos el sitio

`python manage.py runserver`

`python functional_tests/all_users.py`

Todo correcto, seguimos.

Inicializar el repositorio Git y hacer el primer `commit` 
-----------------------------------------------------------

 ```
 git init .
 touch .gitignore
 ```
 
 ```text
 db.sqlite3
__pycache__
*.sublime-workspace

env/
env_prod/

geckodriver.log
```

```
git status
git add .
```

Si vemos algún fichero que no nos interese `git rm --cached fichero`

```
git commit -a -m "Initial commit"
git log
```

Subir el projecto a GitHub
--------------------------

`git remote add origin https://github.com/fherdom/django_template_project.git`

```
git push -u origin --all
Username for 'https://github.com': fherdom
Password for 'https://fherdom@github.com': 
Contando objetos: 19, listo.
Delta compression using up to 4 threads.
Comprimiendo objetos: 100% (14/14), listo.
Escribiendo objetos: 100% (19/19), 3.81 KiB | 1.91 MiB/s, listo.
Total 19 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/fherdom/django_template_project.git
 * [new branch]      master -> master
Rama 'master' configurada para hacer seguimiento a la rama remota 'master' de 'origin'.
```

y comprobamos las ramas activas

`git branch -a`

```
* master
  remotes/origin/master
```

Con esto hemos terminado la configuración del entorno de trabajo, ahora empezaremos con la 'Home Page'.