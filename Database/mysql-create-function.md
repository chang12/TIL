PostgreSQL 의 `DATE_TRUNC` 함수와 동일한 기능을 MySQL 에서 사용하고 싶었다. `DATE_TRUNC` 는 **"microsecond"** 부터 **"century"** 에 이르기까지 다양한 시간 스케일에 대한 truncation 을 제공하지만, 일단 나는 **"week"**, **"month"** 정도면 충분한 상황이었다. 그래서 태어나 처음으로 RDBMS 에 직접 function 을 만들어 사용했다.

```sql
DELIMITER //

CREATE FUNCTION DATE_TRUNC(field CHAR(5), source CHAR(10)) RETURNS CHAR(10) DETERMINISTIC
    BEGIN
        DECLARE truncated CHAR(10);
        
        IF field = "week" THEN SET truncated = DATE_SUB(source, INTERVAL WEEKDAY(source) DAY);
        ELSEIF field = "month" THEN SET truncated = DATE_FORMAT(source, "%Y-%m-01");
        ELSE SET truncated = source;
        END IF;
        
        RETURN truncated;
    END //

DELIMITER ;
``` 