---
title: calendars.calendars
type: guide
order: 7
is_code: true
---

You can use the **`craft.calendars.calendars`** tag to retrieve a list of your calendars.

It returns an **array** of `CalendarModel` objects.

## Querying Calendars

```twig
{% for c in craft.calendars.calendars %}
    <h1>{{ c.name }}</h1>
    <p>Handle: {{ c.handle }}</p>
{% endfor %}
```

## Parameters

The `calendars.calendars` tag doesn't need any parameters, but accepts `id`, `handle`, and `name` parameters to filter the calendars list.

You can use these parameters just like querying for other elements in Craft:

    {% set calendars = craft.calendars.calendars.handle(['a', 'b']) %}


