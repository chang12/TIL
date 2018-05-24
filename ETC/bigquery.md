## Casting

Standard SQL 에서는 형변환을 위해 `cast` 를 사용한다.

```sql
select 1503325261387000 / 1000 -- float64 가 되어 1.2332...E12 로 표기된다.
select cast(1503325261387000 / 1000 as int64) -- 원하는대로 int64 로 변환!
```

## Date / Time 관련

### micro second 에서 date 로 변환

**Firebase** SDK 에서 이벤트를 로깅하면, `timestamp_micros` 라는 이름으로 unix timestamp 를 찍어준다. 이름에서 알 수 있듯이 단위는 micro second 이다. 이를 date 로 변환하고 싶은 니즈가 있었고, bigquery 의 [built-in 함수로](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#timezone-definitions) 쉽게 변환할 수 있었다. time-zone 도 고려할 수 있었다.

```sql
DATE(TIMESTAMP_MICROS(1502713851000000 + 3 * 3600 * 1000 * 1000), "+09:00”)
```	

### string 에서 timestamp, timestamp 에서 unix epoch 으로 변환

[Bigquery Standard SQL 문서](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#parse_timestamp)

```sql
SELECT PARSE_TIMESTAMP("%Y%m%d", "20170829", "Asia/Seoul")  -- 2017-08-28 15:00:00 UTC
SELECT UNIX_MILLIS(PARSE_TIMESTAMP("%Y%m%d", "20170829", "Asia/Seoul"))  -- 1503932400000 
``` 
