# Keywords

* binary data format
* fully self-describing
* how schema defined
    * e.g. avro ~ json
* when schema changes
    * e.g. avro ~ both the old and new schema are always present when processing data
* row based vs columnar
    * read 때는 columnar 가 좋은듯. (일부 column select 가능)
    * write 때는 columnar 가 더 비싼듯.