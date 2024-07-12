---
layout: page
title: Staff
description: A listing of all the course staff members.
---

# Instructor

{% assign instructors = site.staffers | where: 'role', 'Instructor' %}
{% for staffer in instructors %}
{{ staffer }}
{% endfor %}

## Bio
Ali Krema graduated from the University of Pennsylvania Class of 2023, majoring in Computer Science. As of July 2024, he works as a Backend Engineer at Aydi. His current teaching experience include:

- Teaching Assistant @ UPenn's CIS2400: Intro to Computer Systems (Fall 2021 - Fall 2022)
- Instructor @ UPenn's CIS1950: Intro to Android Development (Spring 2023)