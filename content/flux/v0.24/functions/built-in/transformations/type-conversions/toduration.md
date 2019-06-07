---
title: toDuration() function
description: The toDuration() function converts a value to a duration.
aliases:
  - /flux/v0.24/functions/transformations/type-conversions/toduration
menu:
  flux_0_24:
    name: toDuration
    parent: built-in-type-conversions
    weight: 1
---

The `toDuration()` function converts a value to a duration.

_**Function type:** Type conversion_  
_**Output data type:** Duration_

```js
toDuration()
```

## Examples
```js
from(bucket: "telegraf")
  |> filter(fn:(r) =>
    r._measurement == "mem" and
    r._field == "used"
  )
  |> toDuration()
```

## Function definition
```js
toDuration = (tables=<-) =>
  tables
    |> map(fn:(r) => duration(v: r._value))
```
