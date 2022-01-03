# Dates

- Time in Javascript is measured throught the Date object (Instantiated using `new Date()`)
- For some reason Javascript thinks the beginning of time is January 1st, 1970.
  - Thus a `Date` instance represents a single point in time **measured in milliseconds, where 0 is 0:00 Jan 1st 1970 (UTC).**
- The`new Date()` constructor displays the counted milliseconds in a **platform-independent format**: `year-Month-DayTHour:Min:Sec.MilSecZ`. -> where "Z" means UTC time zone.
  - **If a number is used to instantiate** the Date instance will be the counted milliseconds from Jan 1st, 1970 and displayed in the platform-independent format
    - Example: `console.log(new Date(1))` logs `1970-01-01T00:00:00.001Z`
  - **If a string is used to instantiate** (such as `new Date('2018-07')`), Javascript will do its best to parse the string and display the milliseconds in the platform-independent format ([See examples](https://flaviocopes.com/javascript-dates/))
    - Example: `console.log(new Date(01/01/01))` logs `2001-01-01T07:00:00.000Z`
- **The `date.parse()`** works the same as `new Date()`, but returns the total number of milliseconds, not the platform-independent format
- A **Unix Time Stamp** is the number of **seconds** (not milliseconds) from January 1, 1970. So **to convert it to milliseconds** multiple the Unix Time Stamp by 1000
