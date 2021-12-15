---
layout: default
title: Getting Help
permalink: /help/
---

### Online Help
1. You can use [Piazza](https://www.cs.usfca.edu) to ask questions any time. We try to answer as soon as possible.
1. Public questions: if you're asking a question about course material.
1. Private questions: if you're asking a question about your grade.
1. Piazza is preferred over email.
1. Github links are preferred over screen shots.

### Drop-In Office Hours

{% for person in site.people -%}
| [{{person.name}}](mailto:{{person.email}}) ({{person.role}}) | {{person.office_hours}} |
{% endfor %}

- If our regular office hours don't match your schedule, please contact us on Piazza to make an appointment at a mutually available time.

### CS Tutoring Center

The [Computer Science Tutoring Center](https://tutoringcenter.cs.usfca.edu/) is a great resource to get help from other students.
