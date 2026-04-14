---
layout: default
---

## About Me

Hi! I am a professor of practice at Tulane University. I graduated with my PhD in [applied math from Arizona State University](https://math.asu.edu/degrees/graduate/applied-mathematics-phd).

## Teaching
At Tulane University, I teach and coordinate Math 1230: Statistics for Scientists. My teaching emphasizes critical and quantitative thinking, using relatable examples to engage students and deepen their understanding of probability and statistics. I integrate technology, such as Gradescope, to enhance learning efficiency while maintaining a strong focus on human interaction. In my classroom, I encourage active participation through hands-on activities, flipped classrooms, and discussions that challenge assumptions. My goal is to equip students with the analytical skills they need to navigate an increasingly data-driven world. See [this blog post](https://juhang62.github.io/normal-gender-imbalance-DEI/) for example on how I teach students to use normal distribution to reflect on DEI policies.

## Research Interest
My research is in mathematical biology.  During my postdoctoral work at the FDA, I employed mathematical modeling to gain insights into cancer biology and its treatment.

For my PhD, I studied cancer using mathematical models with a focus on temporal and spatial dynamics that arises from time delay and stochasticity. As part of the group of [Prof Yang Kuang](https://math.la.asu.edu/~kuang/) and [Prof Eric Kostelich](https://math.la.asu.edu/~eric/), I have worked on extracting digital markers from MRI data to infer patient-specific parameters for a partial differential equation model.

Under the advise of [Prof John Fricks](http://www.johnfricks.org/), I have developed a computationally efficient method to get asymptotic velocity and diffusivity of a group of motors.

## Publications

<ol class="publications">
{% for pub in site.data.publications %}
  <li>
    {%- for author in pub.authors -%}
      {%- assign sigil = author | slice: 0, 1 -%}
      {%- if sigil == "@" -%}
        <strong>{{ author | remove_first: "@" }}</strong>
      {%- elsif sigil == "*" -%}
        <span class="student-author">{{ author | remove_first: "*" }}</span><sup>&dagger;</sup>
      {%- else -%}
        {{ author }}
      {%- endif -%}
      {%- unless forloop.last %}, {% endunless -%}
    {%- endfor %} ({{ pub.year }}). {% if pub.doi %}<a href="https://doi.org/{{ pub.doi }}">{{ pub.title }}</a>{% else %}{{ pub.title }}{% endif %}. <em>{{ pub.venue }}</em>{% if pub.volume %}, {{ pub.volume }}{% endif %}{% if pub.pages %}, {{ pub.pages }}{% endif %}.
  </li>
{% endfor %}
</ol>

<p><small><sup>&dagger;</sup> student mentee</small></p>

<br>
