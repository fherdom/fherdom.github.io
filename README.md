## Requisitos

1. Comprobar si tenemos instalado Ruby y que sea superior a 2.1.0, si no, instalarlo [Ruby]

```bash
$ ruby --version
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux-gnu]
```

2. Install Bundler

`$ gem install bundler`

3. CLI to post, draft and publish

GemFile
`gem 'jekyll-compose', group: [:jekyll_plugins]`

_config.yml
```yaml
jekyll_compose:
    auto_open: true

    draft_default_front_matter:
      description:
      image:
      category:
      tags:
    post_default_front_matter:
      description:
      image:
      category:
      tags:
      published: false
      sitemap: false    
```
```bash
bundle exec jekyll draft "Django commands like DTS (Elasticsearch)"
bundle exec jekyll publish _drafts/django-commands-like-dts-elasticsearch.md --date 2019-06-22
bundle exec jekyll unpublish _posts/2019-06-22-django-commands-like-dts-elasticsearch.md 
bundle exec jekyll server -D --livereload
```


[Ruby]: https://www.ruby-lang.org/en/downloads/

