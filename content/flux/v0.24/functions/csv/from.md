---
title: csv.from() function
description: The csv.from() function retrieves data from a CSV data source.
aliases:
  - /flux/v0.24/functions/inputs/fromcsv
  - /flux/v0.24/functions/built-in/inputs/fromcsv
menu:
  flux_0_24:
    name: csv.from
    parent: CSV
    weight: 1
---

The `csv.from()` function retrieves data from a comma-separated value (CSV) data source.
It returns a stream of tables.
Each unique series is contained within its own table.
Each record in the table represents a single point in the series.

_**Function type:** Input_

```js
import "csv"

csv.from(file: "/path/to/data-file.csv")

// OR

csv.from(csv: csvData)
```

{{% warn %}}
`csv.from()` is not avaialable in InfluxCloud.
{{% /warn %}}

## Parameters

### file
The file path of the CSV file to query.
The path can be absolute or relative.
If relative, it is relative to the working directory of the `influxd` process.
_The CSV file must exist in the same file system running the `influxd` process._

_**Data type:** String_

### csv
Raw CSV-formatted text.

{{% note %}}
CSV data must be in the CSV format produced by the Flux HTTP response standard.
See the [Flux technical specification](https://github.com/influxdata/flux/blob/master/docs/SPEC.md#csv)
for information about this format.
{{% /note %}}

_**Data type:** String_

## Examples

### Query CSV data from a file
```js
import "csv"

csv.from(file: "/path/to/data-file.csv")
```

### Query raw CSV-formatted text
```js
import "csv"

csvData = "
result,table,_start,_stop,_time,region,host,_value
mean,0,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:00Z,east,A,15.43
mean,0,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:20Z,east,B,59.25
mean,0,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:40Z,east,C,52.62
"

csv.from(csv: csvData)
```
