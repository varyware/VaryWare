# Home Page

Check GitHub Pages Dependency versions:

https://pages.github.com/versions/

## Install

```
$ brew install ruby@2.7
$ cd ~ && sudo chown -R Roger .bundle;
$ sudo gem install bundler
$ sudo gem install jekyll -v 3.9.0
$ bundle install --local
$ bundle exec jekyll serve --watch
```

## Modify Content

### Create Posts
```
$ python3 manage.py -c {post name here}
```

### Add New Product
Modify `_data/products.csv`

## Enable TOC

```
  {% capture sidebar_content %}
  <!-- <div class="share-sidebar">
    <button> Share </button>
  </div> -->
  <div>
    <p class="toc-sidebar-header">目录</p>
    {%- include toc.html html=content h_min=2 h_max=2 class='toc-sidebar' item_class='toc-sidebar-item' -%}
  </div>
  {% endcapture %}
  {%- include sidebar.html sidebar_content=sidebar_content -%}


  // Version 2.0

  {% capture sidebar_content %}
  <div class="sidebar-category">
    <p class="category-header">分类</p>
    <ul class="category-list">
      <li class="category-item"><a class="category-item-link current" href="/categories/">全部</a></li>
      {% assign category_count = 0 %}
      {%- for category in site.categories -%}
      {% if category_count <= 7 %}
      <li class="category-item"><a class="category-item-link"
          href="/categories/#{{ category | first | strip}}">{{ category | first | capitalize }}</a></li>
      {% assign category_count = category_count | plus: 1 %}
      {% endif %}
      {%- endfor -%}
    </ul>
  </div>
  {% endcapture %}

  {%- include sidebar.html sidebar_content=sidebar_content -%}
```
