---
layout: page
title: Announcements
description: A feed containing all of the class announcements.
---

# Announcements

A feed of all class assignments throughout the duration of the course

{% assign announcements = site.announcements | reverse %}
{% for announcement in announcements %}
{{ announcement }}
{% endfor %}
