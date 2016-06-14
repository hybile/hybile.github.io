---
layout: page
title: Articles
permalink: /articles/
---


<h5>2016</h5>
<!--<a href="http://medium.com" target="_blank">Article 1</a><br/>
<a href="http://medium.com" target="_blank">Article 2</a>-->

<ul class="list-unstyled">
    {% for post in site.posts limit:10 %}
        <li>
            <span>{{ post.date | date_to_string }}</span> &raquo;
            <a href="{{ BASE_PATH }}{{ post.url }}">
            {{ post.title }}</a>
        </li>
    {% endfor %}
</ul>