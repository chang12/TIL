# [Optimizing Apache Spark SQL Joins](https://www.youtube.com/watch?v=fp53QhSfQcI)

**Vida Ha (Solutions Architect in databricks) in Spark Summit East 2017**

## Shuffle Hash Join

* join key 에 대해서 dataframes 들이 evenly distributed 면 좋다.
* join key 의 cardinality 가 적당히 커서 좋은 parallelism level 이면 좋다.
* 위의 두 condition 이 만족하지 않는다면?
 * e.g. 전체 미국 국민 DF 와 미국 state DF 의 join
 * Uneven sharding & Limited parallelism 문제라고 부른다.

## Broadcast Hash Join

* 작은 dataframe 을 모든 partition 에 복제한다. 
* 따라서 shuffle 할 필요가 없어지기도 한다.
* explain 을 호출해서 어느 hash join 이 사용되는지 확인하자.

## Theta join

* join condition 이 equality condition 이 아닌 경우를 얘기한다.
* 이 경우 Spark 의 default behavior 는 full cartesian join 이다.
* 적절히 partitioning 될 수 있는 bucket 을 새롭게 정의하자.
* window function 을 쓰는것도 theta join 과 유사하다.

## ETC

* left join 하는 DF 가 크다면, join condition 으로 미리 걸러서 shuffle 되는 데이터의 크기를 줄이자.
* Spark UI 로 들어가서 task level detail 을 보자.
 * 다른 task 보다 유독 시간이 오래 걸리는 task 가 존재한다면 uneven sharding 이다.
 * 유독 input or shuffle output 이 큰 shard 가 존재하는지도 확인하자.