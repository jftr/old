---
layout: page
title: Tags
header: Posts By Tag
group: navigation
---


  {% capture tagString %}{% for tag in site.tags %}{{ tag[0] }}{% unless forloop.last %}|{% endunless %}{% endfor %}{% endcapture %}
  {% assign tags = tagString | split: '|' | sort: 'downcase' %}
  <div class="tagcloud">
    {% for tag in tags %}
    {% assign number = site.tags[tag].size %}
    {% assign slug = tag | downcase | replace: ' ', '_' %}
    <span class="{% if number == 1 %}small{% elsif number <= 5 %}medium{% elsif number <= 10 %}large{% else %}huge{% endif %}">
      <a href="#tag-{{ slug }}">{{ tag }}</a>
    </span>
    {% endfor %}
  </div>
  <div class="tagroup">
    {% for tag in tags %}
    {% assign slug = tag | downcase | replace: ' ', '_' %}
    <div class="grpbytag">
      <a class="anchor" id="tag-{{ slug }}"></a>
      <h3>{{ tag }}</h3>
      <ul>
        {% for post in site.tags[tag] %}
        <li><a href="{{post.url}}">{{post.title}}</a></li>
        {% endfor %}
      </ul>
    </div>
    {% endfor %}
  </div>