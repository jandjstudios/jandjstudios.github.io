---
permalink: /
title: "Home"
layout: collection
excerpt: "Current products offered by J&J Studios"
last_modified_at: 2018-06-04T12:04:24-04:00
toc: false
sidebar: false
---
{% for datum in site.datum %}
  <a href="/datum/{{datum.title}}">
    <img src="/assets/images/{{datum.title}}-small.jpg" alt="{{datum.title}}"
    style="width:175;height:100px;">
  </a>
  <a href="/datum/{{datum.title}}">{{datum.title}}</a>
{% endfor %}
