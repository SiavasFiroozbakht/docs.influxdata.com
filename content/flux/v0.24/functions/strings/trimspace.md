---
title: strings.trimSpace() function
description: The strings.trimSpace() function removes leading and trailing spaces from a string.
menu:
  flux_0_24:
    name: strings.trimSpace
    parent: Strings
weight: 1
---

The `strings.trimSpace()` function removes leading and trailing spaces from a string.

_**Output data type:** String_

```js
import "strings"

strings.trimSpace(v: "  abc  ")

// returns "abc"
```

## Paramters

### v
The string value from which to trim spaces.

_**Data type:** String_

## Examples

###### Trim leading and trailing spaces from all values in a column
```js
import "strings"

data
  |> map(fn:(r) => strings.trimSpace(v: r.userInput))
```
