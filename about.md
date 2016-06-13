---
layout: page
title: About me
logo: this is logo
---

{% if page.logo %}
                <a class="blog-logo" href="{{ site.baseurl }}">
                    <img src="{{ page.logo }}" alt="Blog Logo" />
                </a>
            {% endif %}
This is a static page. It could be an 'about page' if you'd like.
