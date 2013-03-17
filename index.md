---
layout: default
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
		 <div class="a-post">
      <a class="home-page-artical-title" href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
			<span class="date pull-right">{{ post.date | date_to_string }}</span>
			<div>{{ post.content }}</div>
		 </div>
  {% endfor %}
</ul>

[1]: http://github.com/divisoryang/pic/raw/master/wikish-folder.png "wikish"


