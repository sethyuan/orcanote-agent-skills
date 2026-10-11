---
name: orcanote-reminder
description: Guide for creating and managing reminders in Orca Note. Use this skill when the user wants to set a reminder or schedule a task.
---

# Orca Note Reminder Skill

This skill provides a standard workflow for creating reminders within Orca Note.

## Workflow

To create a reminder, follow these two steps:

1. **Insert Descriptive Content**: Use the `insert_markdown` tool to write the content of the reminder into the note.
2. **Tag the Reminder Block**: Use the `insert_tags` tool to apply a "Reminder" tag to the block, specifying its trigger time and recurrence properties.

## The Reminder Tag

The "Reminder" tag is used to define the scheduling logic. It supports the following properties:

| Property | Description | Required | Format/Units |
| :--- | :--- | :--- | :--- |
| `Date time` | The trigger date and time for the reminder. Ignored when `Cron` is set. | **Yes** (unless `Cron` is set) | Unix timestamp in seconds |
| `Repeat every` | The recurrence interval. Multiple intervals can be separated by commas (`,`). Ignored when `Cron` is set. | No | `m,h,d,w,M,y` (min, hour, day, week, month, year) |
| `Repeat until` | The expiration date for recurring reminders. Ignored when `Cron` is set. | No | Unix timestamp in seconds |
| `Cron` | A cron expression for recurring schedules. Takes precedence over `Date time`, `Repeat every` and `Repeat until`. | No | Cron expression (5 or 6 fields) |

> **Note**: `Cron` and the `Date time`/`Repeat every`/`Repeat until` group are mutually exclusive. When `Cron` is specified, do **not** set `Date time` (or the repeat properties); the other properties are ignored entirely.

### Recurrence Units

When setting the `Repeat every` property, use these abbreviated units:
- `m`: Minute(s)
- `h`: Hour(s)
- `d`: Day(s) - e.g., `1d` for every day
- `w`: Week(s) - e.g., `1w` for every week
- `M`: Month(s) - e.g., `1M` for every month
- `y`: Year(s) - e.g., `1y` for every year

### Cron Expressions

Use the `Cron` property for calendar-style schedules that `Repeat every` cannot express (e.g., "every weekday at 09:00" or "the last day of each month").

```
 # ┌────────────── second (optional)
 # │ ┌──────────── minute
 # │ │ ┌────────── hour
 # │ │ │ ┌──────── day of month
 # │ │ │ │ ┌────── month
 # │ │ │ │ │ ┌──── day of week
 # * * * * * *
```

- **5 fields** (`minute hour day-of-month month day-of-week`, e.g. `0 9 * * 1-5`) or **6 fields** with a leading seconds field (e.g. `30 0 9 * * 1-5`).
- Schedules are evaluated in the **local timezone** of the device (no timezone can be configured).
- Supported syntax: ranges (`1-5`), steps (`*/15`), lists (`1,15`), month/weekday names (`JAN`, `MON`), `L` (last day of month), `L-n`, `W`/`LW` (nearest weekday), `#` (nth weekday), a weekday token followed by `L` (last such weekday of month, e.g. `5L`), and `?` (alias for `*` in the day fields).
- Nicknames are supported: `@yearly`, `@annually`, `@monthly`, `@weekly`, `@daily`, `@midnight`, `@hourly`.
- Day-of-week uses standard numbering: `0`/`7` = Sunday, `1` = Monday, … `6` = Saturday.
- When both day-of-month and day-of-week are restricted, they are combined with **AND** (both must match), unlike classic Vixie cron.
- Occurrences are only generated from the current time forward: past occurrences are never back-filled, and a reminder fires only while Orca Note is running.

## Usage Examples

### Simple Reminder
1. `insert_markdown`: "Buy groceries"
2. `insert_tags`: `Reminder` with `Date time: 1771149600` (corresponds to 2026-02-15 18:00)

### Weekly Recurring Reminder
1. `insert_markdown`: "Team Sync Meeting"
2. `insert_tags`: `Reminder` with attributes:
   - `Date time`: `1770773400` (corresponds to 2026-02-11 09:30)
   - `Repeat every`: `"1w"`
   - `Repeat until`: `1798646400` (corresponds to 2026-12-31)

### Cron Reminder (Every Weekday at 09:00)
1. `insert_markdown`: "Stand up and stretch"
2. `insert_tags`: `Reminder` with a single attribute:
   - `Cron`: `"0 9 * * 1-5"`

(Do not set `Date time` when `Cron` is used.)

### Cron Reminder (Last Day of Each Month at 18:00)
1. `insert_markdown`: "Run monthly report"
2. `insert_tags`: `Reminder` with a single attribute:
   - `Cron`: `"0 18 L * *"`
