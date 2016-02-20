---
layout: page
title: Blog
excerpt: "An archive of blog posts sorted by date."
search_omit: true
---


<div class="toc">
  <h2>Sample texts</h2>
    <ul class="post">
      {% for item in site.categories.blog do %}
        
            <li class="post-title">
                  <a href="{{ site.baseurl }}{{ item.url }}">
                          {{ item.title }}
                  </a>
            </li>
      {% endfor %}
    </ul>  
</div>
