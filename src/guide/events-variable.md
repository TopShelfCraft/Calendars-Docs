---
title: calendars.events
type: guide
order: 5
is_code: true
---


You can access event data from your templates using the **`craft.calendars.events`** variable.

This variable returns an **`EventCriteriaModel`** object.


## EventCriteriaModel

An `EventCriteriaModel` is an object that represents a query for events.

Adding [parameters](#parameters) to the `EventCriteriaModel` changes how events will be filtered or sorted by the query.



## How it Works

Whenever you use `craft.calendars.events` in your templates, this happens:

1. An `EventCriteriaModel` object is created and wired to fetch events for elements matching the supplied parameters.

1. You can set additional [parameters](#parameters) to filter the dates and/or events further, or to change how they are returned (e.g. set the sorting order, limit how many events should be returned, and so forth).

1. Elements matching the supplied criteria are queried from the database. _Craft Calendars_ processes all the applicable schedule rules for those elements and produces the list of event occurrences.

1. When you [fetch the events](#fetching-the-events), those occurrences are sorted, limited, offset, and assembled into the collection of `EventModel` objects you'll use in your template.

The `EventCriteriaModel` is optimized so that it only fetches new data from the database (or recomputes events and dates) when it has to — i.e. when you change the parameters of the date range or event attributes. You can change the [`eventLimit`](#eventLimit), [`eventOffset`](#eventOffset), and [`eventOrder`](#eventOrder) parameters multiple times without incurring any performance hits from additional database queries.


## Setting the Parameters

The main point of `EventCriteriaModel` objects is to define the parameters Craft should factor in when it's searching for matching elements and events.

<blockquote> The `EventCriteriaModel` behaves just like Craft's own `ElementCriteriaModel`. Finding events using `calendars.events` should feel almost identical to finding entries using the `craft.entries` variable. </blockquote>

There are two ways you can add parameters to your EventCriteriaModel object:

1. You can chain parameters onto the `EventCriteriaModel`:
```twig
{% set events = craft.calendars.events
     .dateRangeEnd('+1 month').eventLimit(10) %}
```

1. You can pass them all at once to the method that creates the `EventCriteriaModel` using an object:
```twig
{% set events = craft.calendars.events({
        dateRangeEnd: '+1 month',
        eventLimit: 10
}) %}
```

Use whichever syntax feels nicest to you. The chaining syntax is usually more readable, but the object syntax is more DRY if you have a need to reuse the parameters multiple times in your template.

**Example:** To query events from _My Awesome Calendar_ that occur between _today_ and _next Friday_, you could use either of the following template snippets:

```twig
{% set params = {
    calendar: 'myAwesomeCalendar',
    dateRangeStart: 'today',
    dateRangeEnd: 'next Friday'
} %}
 
{% for event in craft.calendars.events(params) %}
    ...
{% endfor %}
```

```twig 
{% set events = calendars.events
    .calendar('myAwesomeCalendar')
    .dateRangeStart('today')
    .dateRangeEnd('next Friday')  %}
 
{% for event in events %}
    ...
{% endfor %}
```

## Parameters

`craft.calendars.events` accepts any defined attribute of `ElementCriteriaModel` as a parameter, plus some additional calendar-specific parameters.

### dateRangeStart

The start of the date range (inclusive) for which event occurrences will be calculated. Accepts a [DateTime](http://buildwithcraft.com/classreference/etc/dates/DateTime) object or a [date string](http://php.net/manual/en/datetime.formats.php).

### dateRangeEnd

The end of the date range (inclusive) for which event occurrences will be calculated. Accepts a [DateTime](http://buildwithcraft.com/classreference/etc/dates/DateTime) object or a [date string](http://php.net/manual/en/datetime.formats.php).

### startsAfter _(coming soon)_

A filter parameter for the calculated events. (The list of `dates` retrieved will still reflect the **dateRangeStart** and **dateRangeEnd**, but only `events` that begin after the **startsAfter** date will be included.) Accepts a [DateTime](http://buildwithcraft.com/classreference/etc/dates/DateTime) object or a [date string](http://php.net/manual/en/datetime.formats.php).

### startsBefore _(coming soon)_

A filter parameter for the calculated events. (The list of `dates` retrieved will still reflect the **dateRangeStart** and **dateRangeEnd**, but only `events` that begin before the **startsBefore** date will be included.) Accepts a [DateTime](http://buildwithcraft.com/classreference/etc/dates/DateTime) object or a [date string](http://php.net/manual/en/datetime.formats.php).

### endsBefore _(coming soon)_

A filter parameter for the calculated events. (The list of `dates` retrieved will still reflect the **dateRangeStart** and **dateRangeEnd**, but only `events` that end before the **endsBefore** date will be included.) Accepts a [DateTime](http://buildwithcraft.com/classreference/etc/dates/DateTime) object or a [date string](http://php.net/manual/en/datetime.formats.php).

### endsAfter _(coming soon)_

A filter parameter for the calculated events. (The list of `dates` retrieved will still reflect the **dateRangeStart** and **dateRangeEnd**, but only `events` that end after the **endsAfter** date will be included.) Accepts a [DateTime](http://buildwithcraft.com/classreference/etc/dates/DateTime) object or a [date string](http://php.net/manual/en/datetime.formats.php).

### calendarId

The ID(s) of the calendar(s) from which events will be fetched.

### calendar

The calendar from which events will be fetched. Accepts a calendar handle, array of handles, _CalendarModel_, or array of _CalendarModel_ objects.

### ownerId

The ID(s) of the owner element(s) whose event dates will be used to compute the events list.

### fieldId

The ID of the Field whose data will be used to compute the events list.

### field

The field whose data will be used to compute the events list. Accepts a field handle or array of field handles.

### elementType

The element type(s) of the owner elements whose event data will be used to compute the events list.

### startDate

The starting date of the _original_ event whose eventData will be used to calculate the `events` list. (This is relevant if you have recurring events, and you want to filter out any event whose original occurrence was before or after a particular date.)

### repeats

Filter the `events` list to include only events which do or do not have recurrences. Accepts a boolean.

### repeatsForever

Filter the `events` list to include only events which do or do not recur forever. Accepts a boolean.

### eventLimit

A limit for the number of events to be included in the `events` list. Accepts an integer.

### eventOffset

The number of events to skip/remove from the beginning of the (sorted) `events` list. (Use with **eventLimit** to create paginated lists.) Accepts an integer.

### eventOrder

The Property and direction by which to sort the computed events list. (e.g. `'startDate asc'` or `'endDate desc'`)

### attachOwners

Whether to attach the actual ElementModel for each event. **Craft Calendars** will fetch all the events of each Element Type at once, to help prevent excessive database queries. However, if you don't need the owner element data (such as titles or custom fields), you can disable this to further improve performance.

Disabling `attachOwners` will also disable ordering events by title, since the `title` property belongs to the owner element.

### attachCalendars

Whether to attach the actual CalendarModel for each event. **Craft Calendars** will fetch all the calendars at once, to help prevent excessive database queries. However, if you don't need the calendar data, you can disable this to further improve performance.


## Fetching the Events

Once you've set all of the parameters on your `EventCriteriaModel`, the last step is to have it fetch the events/elements from the database. You have a few different options on that front, depending on what you need:

### `.findEvents()`

Gets a sorted array of EventModel objects, filtered by the criteria.

_Whenever you use `calendars.events` in a loop or array context, Craft calls `.findEvents` automatically._

### `.countEvents()`

Returns the count of sorted events, taking into consideration the **eventOffset** and **eventLimit** attributes.

### `.totalEvents()`

Returns the total number of events that match the supplied criteria, without taking the **eventOffset** or **eventLimit** parameters into consideration.

### `.firstEvent()`

Returns the first EventModel in the generated list.

### `.lastEvent()`

Returns the first EventModel in the generated list.

### `.nthEvent(n)`

Returns the Nth EventModel in the generated list.

### `.ownerIds()`

Returns an array listing the IDs of elements from which events were generated.

(This will be provided even if you set the **attachOwners** parameter to `false`, so it can be useful if you want to run your own query to retrieve some subset of the actual Elements.)

### `.calendarIds()`

Returns an array listing the IDs of calendars from which events were generated.

(This will be provided even if you set the **attachCalendars** parameter to `false`, so it can be useful if you want to run your own query to retrieve some subset of the associated calendars.)
