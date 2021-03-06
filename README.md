---
theme: gaia
paginate: true
footer: "https://github.com/tc39/proposal-temporal"
style: |
  @import url('https://cdn.jsdelivr.net/npm/hack-font@3/build/web/hack-subset.css');
  code { font-family: Hack; }
  section { letter-spacing: 0; }
  ul ul li { font-size: 75%; }
---

<!-- _class: invert lead -->

# 🕓 Temporal

**Philip Chimento**
Igalia, in partnership with Bloomberg
TC39 March 2021

---

## Update for 2021-03

- Recap
- Summary of changes & delegate reviews
- What is still open
- Discussion
- Stage 3?

---

## ⏳ Why 2 hours?

- Editors recommended to reserve a generous amount of time in case delegates wanted to get more into the weeds during this plenary
- Hopefully we will not have to use the whole time box

---

## 🕓 Recap: Temporal

- Modern built-in date/time API for JavaScript
- Immutable types
- Nanosecond precision
- Time zone and calendar aware
- Broad group of champions

---

## 🔎 Recap: Where to find...

<style scoped>li a { font-size: 60%; }</style>

- ...the **spec draft**? [tc39.es/proposal-temporal](https://tc39.es/proposal-temporal/)
- ...the MDN-style **API docs**? [tc39.es/proposal-temporal/docs](https://tc39.es/proposal-temporal/docs/)
- ...the **cookbook**? [tc39.es/proposal-temporal/docs/cookbook.html](https://tc39.es/proposal-temporal/docs/cookbook.html)
- ...the **polyfill**? [npmjs.com/package/proposal-temporal](https://www.npmjs.com/package/proposal-temporal)
- ...a quick **JS environment with Temporal**?
  - _open your browser console on the API docs page_

---

## ❄️ Recap: At the last meeting, we...

- Froze the spec text: only made normative changes in response to delegate review
  - Other activity you may have seen: editorial changes, bugfixes, polyfill changes, docs, test262
- Warned you Stage 3 was coming
- Called for reviews

---

<!-- _class: invert lead -->

# Summary of changes
## & delegate reviews

---

## Removal of observable `from()` calls

- Previously, you could monkeypatch `Temporal.Calendar.from()` and `Temporal.TimeZone.from()` to include custom time zones and calendars
- Affects not only `from()` but all deserialization entry points
- Background:
  - [tc39/proposal-temporal#294](https://github.com/tc39/proposal-temporal/issues/294)

---

## Removal of observable `from()` calls

Previously:
```js
const origFrom = Temporal.Calendar.from;
Temporal.Calendar.from = function (calendarLike) {
  if (calendarLike === 'custom-calendar')
    return new MyCustomCalendar();
  return origFrom(calendarLike);
}
Temporal.PlainDate.from('1999-12-31[u-ca=custom-calendar]');
// => Party like it's 1999
```

---

## Removal of observable `from()` calls

- Resolution: remove this single monkeypatching point
- To install a custom time zone or calendar globally, user code must replace Temporal entirely
- `from()` still exists but is not observably called internally by Temporal

---

## Removal of observable `from()` calls

- Add a resolver or "local registry" parameter
- Plan to develop an API and submit a needs-consensus PR for this in April

---

## Removal of observable `from()` calls

(just an example, not a proposed API)
```js
const resolver = {
  'custom-calendar'() {
    return new MyCustomCalendar();
  },
};
Temporal.PlainDate.from('1999-12-31[u-ca=custom-calendar]', { resolver });
```

---

## ⛏️ Other minor changes

- From delegate reviews
  - Name changes to clarify ISO vs general operations
  - Improved precision of language re: mutability, time zones
  - Observable property access order on options objects
- Minor bugfixes

---

<!-- _class: invert lead -->

# What is still open?

---

## 🪢 ISO String Extensions

- In January we talked about standardizing extensions to the ISO string format
- `2020-08-05T20:06:13+09:00[Asia/Tokyo][u-ca=japanese]`
- Bracketed time zone annotation is already a _de facto_ standard
  - But published nowhere
- Bracketed calendar annotation is new

---

## 🪢 ISO String Extensions

- [Draft update to RFC 3339](https://ryzokuken.dev/draft-ryzokuken-datetime-extended/documents/rfc-3339.html)
- At time of writing, due to be presented at IETF 110
- Intended to align with whatever gets standardized
  - Even if there are subsequent changes
- Issue [proposal-temporal#1409](https://github.com/tc39/proposal-temporal/issues/1409)

---

## 🔢 Intl.NumberFormat

- Names of rounding modes in Temporal
  - `roundingMode: 'nearest'`, `'ceil'`, `'floor'`, `'trunc'`
- Intended to align with Intl.NumberFormat V3 proposal
  - Even if there are subsequent changes
  - [proposal-intl-numberformat-v3#7](https://github.com/tc39/proposal-intl-numberformat-v3/issues/7)
- Issue [proposal-temporal#1308](https://github.com/tc39/proposal-temporal/issues/1038)

---

## 📅 Month Code Format

- String format for `monthCode` property
- e.g. `Temporal.now.plainDate('chinese').monthCode`
- Shared between Temporal and ICU4X
- No further changes expected, but _may_ want to standardize in future

---

<style scoped>li { font-size: 75%; }</style>

## 🗒️ Other editorial changes

- ...that we believe are okay to finish or iterate on during Stage 3
- [#519](https://github.com/tc39/proposal-temporal/issues/519)/[#541](https://github.com/tc39/proposal-temporal/issues/541) Wording ensuring correspondence between Intl and Temporal for time zones and calendars
- [#1244](https://github.com/tc39/proposal-temporal/issues/1244)/[#1249](https://github.com/tc39/proposal-temporal/issues/1249) "Rebase" on ECMA-402 2021 edition
- [#1388](https://github.com/tc39/proposal-temporal/issues/1388) Define property access order before impl.-defined operations
- [#1410](https://github.com/tc39/proposal-temporal/issues/1410) Remove redundant intrinsics definitions
- [#1411](https://github.com/tc39/proposal-temporal/issues/1411) Editorial improvements
- [#1413](https://github.com/tc39/proposal-temporal/issues/1413) Check correct usage of 𝔽 and ∞
- [#1416](https://github.com/tc39/proposal-temporal/issues/1416) Fix names of abstract operations
- [#1418](https://github.com/tc39/proposal-temporal/issues/1418) Tweak the split between 262 and 402 in the spec text

---

<style scoped>li { font-size: 75%; }</style>

## 🐜 We will also fix bugs that come up

- [#1415](https://github.com/tc39/proposal-temporal/issues/1415) just opened!

---

<!-- _class: invert lead -->

# 🗣 Discussions

---

<!-- _class: invert lead -->

# Stage 3?

---

## Criteria

- ✅ The solution is complete, no more work possible without impl. experience, significant usage, external feedback
- ✅ Complete spec text
- ❓ Designated reviewers have signed off on the spec text
- ❓ ECMAScript editors have signed off on the spec text

---

## Anticipated changes

- Readability and other editorial fixes
- Issues raised in delegate review

---

<!-- _class: lead -->

# Requesting consensus to move Temporal to Stage 3

---

<!-- _class: invert lead -->

# Thanks!

---

<!-- _class: invert lead -->

# Bonus material

---

<!-- _class: lead -->

![w:1100](string.png)

---

## Removal of observable `from()` calls

- Advantages:
  - React to **short-notice geopolitical changes** in time zones and calendars, before environments are able to ship updates
  - **Limit damage** by users making `MyCustomCalendar` available globally
- Disadvantages:
  - Endorses **monkeypatching** builtin objects
  - **Early running code** can't defend against late running code

---

![](hijricorrection.jpg)

---

## Removal of observable `from()` calls

- Advantages:
  - React to **short-notice geopolitical changes** in time zones and calendars, before environments are able to ship updates
  - **Limit damage** by users making `MyCustomCalendar` available globally
- Disadvantages:
  - Endorses **monkeypatching** builtin objects
  - **Early running code** can't defend against late running code
