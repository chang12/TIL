## Date / Time 관련

### micro second 에서 date 로 변환

**Firebase** SDK 에서 이벤트를 로깅하면, `timestamp_micros` 라는 이름으로 unix timestamp 를 찍어준다. 이름에서 알 수 있듯이 단위는 micro second 이다. 이를 date 로 변환하고 싶은 니즈가 있었고, bigquery 의 [built-in 함수로](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#timezone-definitions) 쉽게 변환할 수 있었다. time-zone 도 고려할 수 있었다.

```sql
DATE(TIMESTAMP_MICROS(1502713851000000 + 3 * 3600 * 1000 * 1000), "+09:00”)
```	
