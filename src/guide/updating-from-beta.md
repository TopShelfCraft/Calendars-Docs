---
title: Updating From Beta
type: beta
order: 99
---

Version 0.8 of the private beta introduced a completely new scheme for storing and querying event data.​ This allowed us to significantly improve speed, data safety, and usability.

## Migrating your event data

When you update from **0.6.x** to **0.8.x​**, a migration will install a `calendars_schedulerules` table in your database. However, **your Event Data is not migrated automatically**.

If you want to start fresh with a new set of event rules, simply delete your old **Event Data** fields, and create some new **Event Dates** fields.

<p class="tip">Rules stored in the old schema will be cleaned out in the **1.x** branch.</p>

If you want to migrate your existing schedule rules into the new schema, we've provided a **BetaController** and some tasks to help handle that for you.

1. Back up your database. _Always back up the database first._
1. Change your existing **Event Data** fields to the new **Event Dates** fieldtype.
1. Clear our your log files &mdash; The migration tasks generate success/error logs for each migrated rule. You'll avoid potential confusion by clearing out Craft's log directory before you run the migration, so you know you're only looking at one process's worth of information.
1. Navigate to `/actions/calendars/beta/migrate06rulesTo08`. This will queue up a migration task for each legacy rule. These should run quite quickly.
1. Verify that no errors occurred in the migration &mdash; A quick search through `calendars.log` for the word "error" should suffice.
1. If errors occurred, shoot us an email with your log file attached and we'll take a look.

_n.b. The migration controller/tasks are provided as a convenience for participants in the private beta. Like the beta version itself, these are not suitable for use on production projects. You are responsible for your own data during the private beta!_

## Changes to recurrence behavior

Events with a start date that does not match their recurrence rule(s) will no longer include the start data in their lists of occurrences. This is to match the expected behavior according to the [RRULE spec](http://www.kanzaki.com/docs/ical/recur.html).

_For example: If you have an event that starts on 12/1/2015 and repeats weekly on Wednesdays, that event would not include December 1 in its list of occurrences, because December 1 was not a Wednesday and thus does not match the provided rule._

You'll need to update any events affected by this design change.


## Updating your templates

Some properties and methods of the `calendars` variables have changed.
 
- `calendars.eventData` is now `calendars.events`
- The _EventCriteriaModel_ now matches the behavior of Craft's _ElementCriteriaModel_ much more closely. It accepts chainable property settings, implements `ArrayAccessor` methods, etc.

See the Changelog and Templating docs for information about the updated API.
