---
layout: default
---
<table>
   <tr>
   <td>
<div class="home">
<h1 id="post_page" style="margin-bottom: 30px; font-size: 20pt; border-bottom: solid 1px #e8e8e8">Tag first search</h1>
{% comment %}
   Create empty arrays
{% endcomment %}
{% assign tags = '' | split: ',' %}
{% assign unique_tags = '' | split: ',' %}

{% comment %}
   Map and flatten
{% endcomment %}
{% assign tags =  site.posts | map: 'categories' | join: ',' | join: ',' | split: ',' %}

{% comment %}
   Uniq
{% endcomment %}
{% assign tags = tags | sort %}

{% for tag in tags %}
   {% comment %}
      If not equal to previous then it must be unique as sorted
   {% endcomment %}
   {% unless tag == previous %}

      {% comment %}
         Push to unique_tags
      {% endcomment %}

      {% assign unique_tags = unique_tags | push: tag %}
   {% endunless %}

   {% assign previous = tag %}
{% endfor %}

{% for tag in unique_tags %}
   {% unless tag == empty %}
<li>
   <a class="post=link" href="{{site.baseurl}}nav/?find={{ tag }}">
      {{tag}}
   </a>
</li>
   {% endunless %}
{% endfor %}
</div>

<td>
<div class="home">
<h1 id="post_page" style="margin-bottom: 30px; font-size: 20pt; border-bottom: solid 1px #e8e8e8">Recent Posts</h1>
   <ul class="posts">
        {% for post in site.posts limit:8 %}
            <li>
                <span class="post-date">{{ post.date | date: "%B %-d, %Y" }}</span>
                <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                    {{ post.title }}
                    {% if post.subtitle %}
                        <span class="subtitle">{{ post.subtitle }}</span>
                    {% endif %}
                </a>
            </li>
        {% endfor %}
    </ul>
   <div id="archive_link">
      <a class="post-link" href="{{ '/archive/' | prepend: site.baseurl }}">More Posts</a>
   </div>
   </div>
