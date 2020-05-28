<!--
theme: default
paginate: true
footer: "https://github.com/tc39/proposal-temporal"
-->

# Temporal 🕓

## Update 2020-06

- Recap
- What's new?
- Roadmap
- Feedback
- Open questions

---

# Recap: Where to find... 🔎

- ...the **spec draft**? [tc39.es/proposal-temporal](https://tc39.es/proposal-temporal/)
- ...the MDN-style **API docs**? [tc39.es/proposal-temporal/docs](https://tc39.es/proposal-temporal/docs/)
- ...the **cookbook**? [tc39.es/proposal-temporal/docs/cookbook.html](https://tc39.es/proposal-temporal/docs/cookbook.html)
- ...the **polyfill**? [tc39/proposal-temporal](https://github.com/tc39/proposal-temporal/tree/main/polyfill) on GitHub
- ...a quick **JS environment with Temporal**?
  - _open your browser console on the API docs page_

---

# What's new since last time? 🆕

- [Cookbook](https://tc39.es/proposal-temporal/docs/cookbook.html) complete
- Calendar
- Custom time zones
- Parse

---

## Calendar

Needed e.g. when doing calculations in another calendar system

Works transparently with the existing API

```javascript
date.withCalendar('...').plus({ months: 1 })
  // adds the number of days in that month according to the calendar
```

---

## Custom time zones

Needed e.g. when recreating particular versions of tzdata in secure environments

```javascript
class MyTimeZone extends Temporal.TimeZone {
  constructor() { super('Some_Identifier'); }
  getOffsetNanosecondsFor(absolute) { ... }
  getPossibleAbsolutesFor(dateTime) { ... }
  *getTransitions(absolute) { ... }
}
```

---

## Parse

Needed when dealing with partially valid data

```javascript
parsed = Temporal.parse('2020-05-27T99:99')
⇒ {
    absolute: null,
    date: '2020-05-27',
    dateTime: '2020-05-27T99:99',
    time: '99:99',
    ...
  }
Temporal.Date.from(parsed.date).withTime(Temporal.Time.from('00:00'))
```

---

# Roadmap 🛣

This week:
- Release and publicize polyfill

---

# Roadmap 🛣

Before Stage 3:
- Gather feedback from and have conversations with polyfill users
- Incorporate decisions & feedback
- Release an updated polyfill with API improvements based on feedback
- W3C TAG review
- Finalize specification

---

# Roadmap 🛣

Request Stage 3 advancement in a few months

---

# Polyfill release 🔜

- "Does it block releasing a version of the polyfill?"
  - Yes? — Address it now
  - No? — Make a provisional decision for now, and revisit before Stage 3

---

# Feedback 📢

- Feedback on the API from the JS developer community
- What we have gotten so far has proved valuable

---

# Open questions for plenary ❓

- Binary comparison operators
- Default calendar

---

## Binary comparison operators

How to correctly compare two dates:

```javascript
date1.equals(date2)  // ⇒ true or false
Temporal.Date.compare(date1, date2)  // ⇒ -1, 0, or 1
dates.sort(Temporal.Date.compare)
```

---

## Binary comparison operators

How people will probably try to compare two dates:

```javascript
date1 === date2
date1 >= date2
```

---

## Binary comparison operators

Open question:

**Do we make `.valueOf()` throw so that this is impossible?**

Champions group answers:

**TBD**

---

## Default calendar

??
