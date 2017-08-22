## 문제

```scala
"2017-08-22".toDateTime  // org.joda.time.DateTime = 2017-08-01T00:00:00.000Z
"2017-08-22 13:55:26".toDateTime  // java.lang.IllegalArgumentException: Invalid format ...
```

## 해결

[joda scala docs](http://www.joda.org/joda-time/apidocs/org/joda/time/format/DateTimeFormat.html#forPattern-java.lang.String-)

```scala
val formatter = DateTimeFormat.forPattern("yyyy-MM-dd HH:mm:ss")  // org.joda.time.format.DateTimeFormatter
DateTime.parse("2017-08-22 13:55:26", formatter)  // org.joda.time.DateTime = 2017-08-22T13:55:26.000Z
```