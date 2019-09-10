---
layout: post
title:  "Tutorial paso a paso con Django Framework - Parte 4"
date:   2019-06-21 11:24:59 +0100
categories: django tutorial
---

Terminaremos con la herencia entre la *base* y la *home page*.

Luego configuraremos los ficheros más útiles de las páginas web.

El fichero **robots.txt** es muy importante en el inicio del proyecto, porque no te interesa que Google indexe ningua de tus páginas. Así no encontrará nada que esté en estado de desarrollo ni una hoja de estilos con errores.

Después veremos como implementar **Coverage** en tus tests que nos dará una idea de cuanto código tenemos contemplado en los tests.

Veremos

* Herencia de plantillas
* Robots.txt y humans.txt
* Imagen favico.ico
* Testing con covertura (Coverage)

Herencia de plantillas
----------------------

En esta parte del tutorial crearemos **index.html** a partrir de **base.html**. Primero haremos una base lo más flexible posible.

Agremos dos bloques que contendrán los estilos y js necesarios en cada página:

{% raw %}
{% block head_css %}{% endblock %}
{% block head_javascript %}{% endblock %}
{% endraw %}

Si queremos una barra de navegación dinámmica:

{% raw %}
{% block navbar %}
    <nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
      <div class="container">
      ...
{% endblock %}
{% endraw %}

Pasamos el contenido del *jumbotron* y el contenido al **index.html**

{% raw %}
{% block content %}
    <div class="jumbotron">
        <div class="container">
        <h1>Hello, world!</h1>
        <p>This is a template for a simple marketing or informational website. It includes a large callout called a jumbotron and three supporting pieces of content. Use it as a starting point to create something more unique.</p>
        <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more &raquo;</a></p>
        </div>
    </div>
    ...
{% endblock %}
{% endraw %}

Dejándo así el bloque *content* de **base.html**

{% raw %}
{% block content %}
{% endblock %}
{% endraw %}

Cuidado con el tag 'footer', este deberá estar en **base.html**

Y finalmente los *js* del pié.

{% raw %}
{% block footer_js %}{% endblock %}
{% endraw %}

Robots.txt y humans.txt
-----------------------

Normalmente los buscadores preguntan por estos dos ficheros:

* robots.txt
* humans.txt

Vamos a preguntar por estos ficheros, creamos el test funcional.

