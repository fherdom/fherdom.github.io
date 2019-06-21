---
layout: post
title:  "Spatialite como 'TEST Database' de PostGIS"
date:   2019-06-12 11:24:59 +0100
categories: django spatialite postgis
---

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.contrib.gis.db.backends.postgis',
        ...
    }
}

if 'test' in sys.argv:
    SPATIALITE_LIBRARY_PATH = 'mod_spatialite.so'
    DATABASES['default'] = {
        'ENGINE': 'django.contrib.gis.db.backends.spatialite',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
```

```bash
(env) fherdom@felix-lubuntu:~/dev/python/django/sitcanapi_project$ python -Wa manage.py test -k
Using existing test database for alias 'default'...
System check identified no issues (0 silenced).
..
----------------------------------------------------------------------
Ran 2 tests in 0.007s

OK
Preserving test database for alias 'default'...
(env) fherdom@felix-lubuntu:~/dev/python/django/sitcanapi_project$
```
