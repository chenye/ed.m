---
layout: page
title: Publications
excerpt: "An archive of publications sorted by date."
search_omit: true
---


<div class="toc">
  <h2>Sample texts</h2>
    <ul class="post">
      {% for item in site.categories.publications do %}
        
            <li class="post-title">
                  <a href="{{ site.baseurl }}{{ item.url }}">
                          {{ item.title }}
                  </a>
            </li>
      {% endfor %}
    </ul>  
</div>
