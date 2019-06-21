---
layout: post
title:  "Jekyll - Generador de sitios estáticos para blogs"
date:   2017-04-20 08:36:54 +0100
categories: jekyll update
---

Jekyll es un generador de sitios estáticos para blogs. Esto que és? pues tener
un *snapshot* del blog en el servidor web, vamos que nos olvidamos de las _base
de datos_, _php_, _python_, _java_, etc. Entre las principales ventajas está la
velocidad de respuesta y el poco uso de recursos, pero es muy técnico, con
mucha línea de comandos y trabajo de ficheros. No obstante, es como todo, si tu
cliente es ordenado, le das un editor de _Markdown_ y luego que te pase los
ficheros para colocarlos en el servidor. Para nosotros es ideal, edito con el
vim y listo.

La documentación la podemos encontrar [Documentación Jekyll][jekyll-docs]

Aquí les dejo los pasos que seguí para instarlo:

```bash
wget https://rubygems.org/gems/rubygems-update-2.6.11.gem
sudo gem install --local rubygems-update-2.6.11.gem
sudo update_rubygems
sudo gem install bundler
sudo gem install jekyll
jekyll new porfolio
cd portolio
jekyll serve
```

Con esto ya podemos empezar a trasterar con las plantillas (_layouts_), las páginas (_*.md_), los '_includes_', etc., y ver los resultados en la ventana de depuración del 'serve'.

![Jekyll serve]({{ "/assets/img/posts/2017-04-20-jekyll-serve.png" | relative_url }}){:class="img-responsive"}

Finalmente, en la capeta _site_ contendrá el resultado de proceso de generación estática que subiremos al servidor.

Es interesante la opción `bundle exec jekyll serve -D --livereload` para visualizar los cambios automáticamente.

### SEO

Instalamos _jekyll-sitemap_ [[1]](https://github.com/jekyll/jekyll-sitemap), luego editamos el fichero Gemfile:
{% highlight config %}
gem "jekyll-feed", "~> 0.6"
gem "jekyll-sitemap"
gem "jekyll-paginate"
{% endhighlight %}

Y lanzamos _jekyll build_. Si queremos generar para *producción* tenemos que establecer:

`export JEKYLL_ENV=production`

### DISQUS

Registramos la web en disqus [[2]](https://disqus.com/websites/) y ponemos en el _config.yml:

```yalm
url: "https://felixjosehernandez.es"
disqus:
    shortname: felixjosehernandez
```

[jekyll-docs]: https://jekyllrb.com/docs/home
