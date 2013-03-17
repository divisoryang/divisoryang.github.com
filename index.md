---
layout: default
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
		 <div class="hero-unit">
      <span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
      <div style="font-size:26">{{ post.content }}</div>
		 </div>
  {% endfor %}
</ul>

[1]: http://github.com/divisoryang/pic/raw/master/wikish-folder.png "wikish"


