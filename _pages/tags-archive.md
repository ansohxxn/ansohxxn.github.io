---
title: Tags
permalink: /tags/
layout: single
author_profile: true
---

<!-- Create empty arrays -->
{% assign all_tags = '' | split: ',' %}

<!-- Map and flatten -->
{% assign notes_tags =  site.notes | map: 'tags' | join: ',' | split: ',' %}
{% assign post_tags =  site.posts  | map: 'tags' | join: ',' | split: ',' %}

<!-- Push to tags -->
{% for tag in notes_tags %}
    {% assign all_tags = all_tags | push: tag %}
{% endfor %}
{% for tag in post_tags %}
    {% assign all_tags = all_tags | push: tag %}
{% endfor %}

<!-- Grouping -->
{% assign grouped_all_tags = all_tags | sort | group_by: self %}

<ul class="tag-cloud">
    {% for tag in grouped_all_tags %}
    {% assign tag_name = tag.name | slugize %}
    {% assign tag_size = tag.size | times: 1500 | divided_by: all_tags.size | plus: 50 %}
    <a href="#{{ tag_name }}"
       style="font-size: {{ tag_size }}%">
        {{ tag_name }}
    </a> <sup>{{ tag.size }}</sup> &nbsp;&nbsp;
    {% endfor %}
</ul>

{% assign tags = site.tags | sort %}

<div id="archives">
    {% for tag in grouped_all_tags %}
    <div class="archive-group">
        {% assign tag_name = tag.name | slugize %}
        <h2 id="#{{ tag_name }}">{{ tag_name }}</h2>
        <a name="{{ tag_name }}"></a>
        <!-- Merge posts -->
        {% assign all_posts = '' | split: ',' %}
        <!-- Posts -->
        {% for post in site.tags[tag_name] %}
            {% assign all_posts = all_posts | push: post %}
        {% endfor %}
        <!-- Notes -->
        {% for post in site.notes %}
            {% if post.tags contains tag_name %}
                {% assign all_posts = all_posts | push: post %}
            {% endif %}
        {% endfor %}

        <!-- Sort by date -->
        {% assign all_posts = all_posts | sort: 'date' | reverse %}
        {% for post in all_posts %}
        <article class="archive-item">
            <div>
                <span><a href="{{ post.url }}">{{post.title}}</a></span><small> <i class="fas fa-fw fa-calendar-alt" aria-hidden="true"> </i>{{ post.date | date: " %b %-d, %Y" }}</small>
            </div>
        </article>
        {% endfor %}
    </div>
    {% endfor %}
</div>