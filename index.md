---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---

<div class="home">
<h1 id="post_page" style="margin-bottom: 30px; font-size: 20pt; border-bottom: solid 1px #e8e8e8">Posts</h1>
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
