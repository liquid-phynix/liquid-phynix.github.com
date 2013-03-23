---
layout: page
title: <div style="display:table; width:100%;"><div style="display:table-cell;vertical-align:middle;float:left;">Building Better Worlds</div><img style="box-shadow:0px 0px 0px 0px;display:table-cell;vertical-align:middle;float:right" height=150px src="assets/images/wl.png"></div>
tagline: # Supporting tagline
---
{% include JB/setup %}

## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> => <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


## About
This is mcstar's web log. You may also know me as liquid phynix.

<!-- Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html) -->

<!-- Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com) -->

<!-- ## Update Author Attributes -->

<!-- In `_config.yml` remember to specify your own data: -->
    
<!--     title : My Blog =) -->
    
<!--     author : -->
<!--       name : Name Lastname -->
<!--       email : blah@email.test -->
<!--       github : username -->
<!--       twitter : username -->

<!-- The theme should reference these variables whenever needed. -->
    
<!-- ## Sample Posts -->

<!-- This blog contains sample posts which help stage pages and blog data. -->
<!-- When you don't need the samples anymore just delete the `_posts/core-samples` folder. -->

<!--     $ rm -rf _posts/core-samples -->

<!-- Here's a sample "posts list". -->

<!-- <ul class="posts"> -->
<!--   {% for post in site.posts %} -->
<!--     <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li> -->
<!--   {% endfor %} -->
<!-- </ul> -->

<!-- ## To-Do -->

<!-- This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)! -->
<!-- We need to clean up the themes, make theme usage guides with theme-specific markup examples. -->


