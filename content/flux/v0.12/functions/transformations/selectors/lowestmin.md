---
title: lowestMin() function
description: The lowestMin() function returns the bottom 'n' records from all groups using the minimum of each group.
menu:
  flux_0_12:
    name: lowestMin
    parent: Selectors
    weight: 1
---

The `lowestMin()` function returns the bottom `n` records from all groups using the minimum of each group.

_**Function type:** Selector, Aggregate_

```js
lowestMin(
  n:10,
  columns: ["_value"],
  groupColumns: []
)
```

## Parameters

### n
Number of records to return.

_**Data type:** Integer_

### columns
List of columns by which to sort.
Sort precedence is determined by list order (left to right).
Default is `["_value"]`.

_**Data type:** Array of strings_

### groupColumns
The columns on which to group before performing the aggregation.
Default is `[]`.

_**Data type:** Array of strings_

## Examples
```js
from(bucket:"telegraf/autogen")
  |> range(start:-1h)
  |> filter(fn: (r) =>
    r._measurement == "mem" and
    r._field == "used_percent"
  )
  |> lowestMin(n:10, groupColumns: ["host"])
```

## Function definition
```js
// _sortLimit is a helper function, which sorts and limits a table.
_sortLimit = (n, desc, columns=["_value"], tables=<-) =>
  tables
    |> sort(columns:columns, desc:desc)
    |> limit(n:n)

// _highestOrLowest is a helper function which reduces all groups into a single
// group by specific tags and a reducer function. It then selects the highest or
// lowest records based on the columns and the _sortLimit function.
// The default reducer assumes no reducing needs to be performed.
_highestOrLowest = (n, _sortLimit, reducer, columns=["_value"], groupColumns=[], tables=<-) =>
  tables
    |> group(columns:groupColumns)
    |> reducer()
    |> group(columns:[])
    |> _sortLimit(n:n, columns:columns)

lowestMin = (n, columns=["_value"], groupColumns=[], tables=<-) =>
  tables
    |> _highestOrLowest(
        n:n,
        columns:columns,
        groupColumns:groupColumns,
        // TODO(nathanielc): Once max/min support selecting based on multiple columns change this to pass all columns.
        reducer: (tables=<-) => tables |> min(column:columns[0]),
        _sortLimit: bottom,
      )
```
