# CppConTimer

A full-screen countdown timer for conference presenters.

https://quuxplusone.github.io/CppConTimer

Original version by Jon Kalb.  
Fully-client-side version by Arthur O'Dwyer.

# Usage

The Timer Service is designed to generate pages that "count down" the time
remaining in a session or a set of sessions.

The Timer Service can be used in One-Off Mode or Conference Mode.

## One-Off Mode

In One-Off Mode, the Timer Service generates a page that counts down the time
for a single session (as if it has already started) ending at a specific time.

## Conference Mode

In order to use the Timer Service in Conference Mode, you will need to create a
JSON data file that can be loaded from a URL. Here is an example: [CppCon2018.json](/CppCon2018.json).

The JSON file represents an array of sessions. Each session is a dictionary
with the following keys:

| Key        | Required? | Data Type        | Content                     | Meaning
|------------|-----------|------------------|-----------------------------|----------------
| `begin`    | required  | string           | datetime in ISO 8601 format | start time of session in local time
| `end`      | optional  | string           | datetime in ISO 8601 format | end time of session in local time
| `duration` | optional  | integer          | positive values             | duration of session, in minutes
| `tracks`   | optional  | array of strings | track names                 | tracks in which this session should be included

### Duration

If the `end` key is present, the duration is taken to be until that time.
Otherwise if the `duration` key is present, the end of the session is calculated
using that value. Otherwise the session is assumed to last for one hour.

### Tracks

A single data file should be adequate for all the requirements of a single conference,
even if that conference has multiple tracks with different sessions times. Any session
without a `tracks` key is assumed to be in all tracks. If a `tracks` key is present,
then the session is only added to the specified tracks.

# URL

The timer service is invoked in one of these forms.

## Conference Mode

- [https://quuxplusone.github.io/CppConTimer/index.html?track=B&json=...](https://quuxplusone.github.io/CppConTimer/index.html?track=B&json=https://raw.githubusercontent.com/Quuxplusone/CppConTimer/master/CppCon2018.json)

where:

- `track` is an optional track name (if not specified, only sessions without `tracks` will be used)
- `json` is the URL of the JSON-formatted file containing the session schedule

## One-Off Mode

- https://quuxplusone.github.io/CppConTimer/index.html?end=3:00pm
- https://quuxplusone.github.io/CppConTimer/index.html?duration=60m

where:

- `end` is the end time of the one-off session
- `duration` is the length of the one-off session, in minutes

Times can be represented in ISO 8601 format (`end=2018-04-01T01:23:45`), or in
"friendlier" formats (`end=3:00pm`, `end=15:00`).
The parsing is handled by [Moment.js](https://momentjs.com).
If you try something sensible and it doesn't work,
please [file an issue](https://github.com/Quuxplusone/CppConTimer/issues).

Durations can also be represented as seconds (`duration=10s`) and hours (`duration=2h`)
for testing purposes.

## Show Hours

By default, the time is shown as a number of minutes.
Optionally, for times longer than 59 minutes, time can be shown as hours and minutes.
Enable it by adding `&show_hours=true` to your URL.

## Show Seconds

By default, the time is not shown with seconds (until the time remaining is less
than ten minutes), but can optionally do so.
Enable it by adding `&show_seconds=true` to your URL.

## Class Mode

In "class mode" the timer always displays hours and seconds, is always in yellow
on black (no color changes), and omits the flashing at five-minute intervals.
Enable it by adding `&class=true` to your URL.
