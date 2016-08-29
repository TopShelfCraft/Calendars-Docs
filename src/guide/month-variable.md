---
title: calendars.month
type: guide
order: 6
is_code: true
---

You can use the **`craft.calendars.month`** variable to produce a monthly calendar.

This variable returns a **`CalendarMonthModel`** object.


## CalendarMonthModel

A `CalendarMonthModel` is an object that represents a calendar month, as well as a query for events that occur during the month.

This makes it easy to find all of the events in a month and display them in a grid layout or weekly list.

```twig
{% set month = craft.calendars.month
     .calendarDay('today')
     .weekStartDay('Sunday')  %}
```

Adding parameters to the `CalendarMonthModel` changes:
 - whether event information will be included in the calendar
 - which events to include
 - how days are grouped into weeks  


## Parameters

A `CalendarMonthModel` accepts the same parameters as an `EventCriteriaModel` object.

(In other words, you can use all the parameters with `calendars.month` that you can use with the `calendars.events` variable.)

`CalendarMonthModel` also has some additional month-specific parameters:

### calendarDay

The date from which the monthly calendar will be generated. (The generated monthly calendar will include the dates from the month in which **calendarDay** falls, plus any leading or trailing days at the beginning/end of the included weeks.) Accepts a [DateTime](http://buildwithcraft.com/classreference/etc/dates/DateTime) object or a [date string](http://php.net/manual/en/datetime.formats.php).

### weekStartDay

The day of the week on which to start the weeks of the calendar. Accepts a string: the full name of the weekday.

### withEvents

Whether or not to include the `EventModel` objects for events that fall within the dates of the computed calendar. (This is set to `true` by default, but if you only need the calendar, without any events, you can disable it to improve performance.) Accepts a boolean.


## Excluding Events

By default, when you fetch a `CalendarMonthModel` using `calendars.month`, _Craft Calendars_ automatically creates a corresponding `EventCrtieriaModel` (based on the parameters you provide), fetches any matching events that occur during the month, and attaches them to the `CalendarMonthModel`.

However, you can use a `CalendarMonthModel` to create a calendar view for any arbitrary month, whether or not is has _Craft Calendars_ events attached.



## Properties

### `daysInMonth`

The number of days in the given month.

### `thisMonth`

A _DateTime_ representing the beginning of the given month.

### `nextMonth`

A _DateTime_ representing the beginning of the next month.

### `prevMonth`

A _DateTime_ representing the beginning of the previous month.

### `calendarStartDay`

A _DateTime_ representing the first day on the calendar, including leading days from the previous month.

### `calendarEndDay`

A _DateTime_ representing the first day on the calendar, including trailing days from the next month.

### `weekdays`

A list of abbreviations of the week days, in order. (The order of week days changes based on the `weekStartDay` parameter you provide.)

Each item is an array with `weekday` _("Sunday")_, `weekdayShort` _("Sun")_, and `weekdayFirst` _("S")_ properties.

### `weeks`

An array list of the weeks in the month.
 
(This is the property you would loop through to create the 'rows' of a calendar grid view.)

Each item of `weeks` has a single property: `days`.

Items in the `days` array represents days in the week. Each day has these properties:

 - `date`
 - `isNextMonth`
 - `isPrevMonth`
 - `isCurrentMonth`
 - `isGivenDay`
 - `isToday`
 - `events`

(These properties are described in detail below.)


## Properties of `days`

### `date`

A _DateTime_ representing the calendar day.

### `isPrevMonth`

A _boolean_ representing whether the calendar day is a leading day from the previous month.

### `isCurrentMonth`

A _boolean_ representing whether the calendar day is part of the given month.

### `isNextMonth`

A _boolean_ representing whether the calendar day is a trailing day from the next month.

### `isGivenDay`

A _boolean_ representing whether the calendar day is the given day from which the month was generated.

### `isToday`

A _boolean_ representing whether the calendar day is today.

### `events`

A list of `EventModel` objects, representing the events that occur on the calendar day.

<blockquote>This property will be empty if you have chosen to [exclude events](#Excluding-Events) by setting [`withEvents`](#withEvents) to `false`.</blockquote> 


## Example Template

You can use this example template as a starting point to create a table-based calendar grid: 

```twig
{#  First, we pull a CalendarMonthModel  #}
{#  by invoking craft.calendars.month    #}
 
{% set params = {
    calendarDay: 'today',
    weekStartDay: "Sunday",  
} %}
 
{% set month = craft.calendars.month(params) %}
 
{#  Now `month` is a CalendarMonthModel  #}
 
<table>
 
    {#  We can use the `weekdays` property  #}
    {#  to create the calendar header.      #}
 
    <tr>
        {% for day in month.weekdays %}
            <th>{{ day.weekdayFirst }}</th>
        {% endfor %}
    </tr>
 
    {#  Then we loop through the `weeks` and `days`  #}
    {#  to output the events.                        #}
 
    {% for week in mmonth.weeks %}
    
    <tr>
    
        {% for day in week.days %}
    
        <td>
        
               isPrev:  {{ day.isPrevMonth }}
               isNext:  {{ day.isNextMonth }}
            isCurrent:  {{ day.isCurrentMonth }}
              isGiven:  {{ day.isGivenDay }}
              isToday:  {{ day.isToday }}
                 Date:  {{ day.date | date('Y-m-d') }}
          Event Count:  {{ day.events | length }}
          
        </td>
        
        {% endfor %}
        
    </tr>
    
    {% endfor %}
    
</table>
```
