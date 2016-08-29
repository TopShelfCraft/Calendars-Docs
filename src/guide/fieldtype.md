---
title: Field Type
type: guide
order: 3
---

Any Element can be turned into an Event (placed onto a Calendar) using the **Event Dates** Field Type.

## Event Dates

To begin placing events on your calendars, simply add an **Event Dates** field to your element.

In the field settings, you will specify which calendar should contain the events edited by this field.

You can add multiple **Event Dates** fields in a layout, if you want one element to represent events on multiple calendars (or even multiple types of events on a single calendar).

## Using fields in templates

When you invoke an **Event Dates** field in your templates, it returns an _EventCriteriaModel_ pre-filtered according to the element ID and field ID.

For example, if `entry` represents an _EntryModel_ with ID 42, and `myEventDates` is an **EventDates** field in the entry's layout, then...

`entry.myEventDates`

...would return the same _EventCriteriaModel_ as...

`craft.calendars.events.id(42).field('myEventDates')`
