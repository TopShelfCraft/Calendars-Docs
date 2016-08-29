---
title: EventModel
type: guide
order: 8
is_code: true
---

An **`EventModel`** is an object that represents an event.

Whenever you’re dealing with an **event** in your template, you’re actually working with an **`EventModel`**.

## **Properties**

`EventModel` objects have the following properties:

### ruleId

The ID of the schedule rule from which this event was generated.

### calendarId

The ID of the calendar that contains this event.

### calendar

An alias to `getCalendar()`

### fieldId

The ID of the field that generated this event.

### field

An alias to `getField()`

### ownerId

The ID of the owner element that generated this event.

### owner

An alias to `getOwner()`

### elementType

The event Element's type.

### startDate

A `DateTime` object representing the event's start date.

### endDate

A `DateTime` object representing the event's end date.

### isAllDay

A boolean representing whether the event spans the entire day.

### isZeroDuration

A boolean representing whether the event's start and end times are the same.

### repeats

A boolean representing whether the event has recurrences.

### repeatsForever

A boolean representing whether the event repeats forever.

### dateCreated

A `DateTime` object of the date the entry data was created.

### dateUpdated #

A `DateTime` object of the date the entry data was updated.


## Methods

`EventModel` objects have the following methods:

### .getCalendar()

Returns a `CalendarModel` object representing the calendar that contains the event.

### .getOwner()

If you left the **attachEvents** parameter set to `true` (the default), this property contains the full owner element object, including all its properties and methods. (This varies by Element Type, but will be some subclass of `BaseElementModel`.

If you set the **fetchElements** parameter to `false`, using this property spawns a one-off query to return the element to you.

### .getField()

Returns a `FieldModel` representing the field responsible for storing the event's data.
