---
layout: post
title:  "Tutorial paso a paso con Django Framework"
date:   2019-06-21 11:24:59 +0100
categories: django tutorial
---

Ya era hora de hacer un tutorial de [Django] y de camino refrescar los aspectos a tener en cuenta para futuros proyectos.

Este tutorial está basado en [TaskBuster Django Tutorial](http://www.marinamele.com/taskbuster-django-tutorial) adaptándolo a la última versión [LTS] (Long Term Support) de [Django], la 2.2.2.

Vamos a crear un proyecto Django desde '0' o, como dicen, 'from scratch', con la intención de que sea un esqueleto o 'template'.

Quiero agradecer a Marina Mele por el estupendo trabajo en el proyecto TaskBuster.

Repartiremos el tuturial en varias entradas del blog cuyo índice inicial podría ser este:

1. [Creación del entorno virtual y del proyecto Django]({% post_url 2019-06-21-django-step-by-step-01 %})
    * Creación del entorno de trabajo
    * Instalación de Django 2.2.2 (LTS)
    * Trabajar con Sublime Text
    * [Obedece a la cabra](http://www.obeythetestinggoat.com/)
    * Crear el proyecto Django
    * Arrancar el servidor de desarrollo.

2. Control de versiones (git) y estructura de ficheros.

3. Creación de la página principal. Agregar [TDD] (Test Driven Development)

4. Herencia de plantillas, ficheros del sitio web y pruebas con cobertura.

5. Internacionalizción y localización. Idiomas y zonas horarias

6. Documentación del proyecto.
    * Instalación y configuración de Sphinx

7. Instalación y configuración de la base de datos
    * PostgreSQL
        * tenemos instalado postgresql 11
        * psql -d example -U postgres
        * y habilitado PostGIS con datos de NY cargados

    * MySQL
    * Spatialite

8. Autenticación
    * Google
    * Twitter
    * CAS (Central Authentication Service )

9. Creación de modelos, relaciones y señales.

10. Django Admin

11. Django Rest Framework

12. uWSGI + Nginx + systemD

[Django]: https://docs.djangoproject.com/en/2.2/
[TDD]: https://es.wikipedia.org/wiki/Desarrollo_guiado_por_pruebas
[LTS]: https://www.djangoproject.com/download/
