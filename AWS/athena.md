* athena jdbc driver 로 query 할때는 `Properties` 에 **S3OutputLocations** 가 필수다.
* athena 로 query 하려면 glue catalog 에 database / table 을 만들어야한다. glue catalog table 을 terraform 으로 기술할때, console 에서 table 을 만들고 view properties json 을 참고하면 편하다.
