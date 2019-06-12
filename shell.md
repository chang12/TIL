cli 로 argument 넘겨서 날짜별로 for-loop 돌며 명령어를 실행하고 싶었음.

```bash
for i in $(seq 0 1 $2)
do
  d=$(date -I -d "$1 -$i days")
  nohup /usr/bin/spark-submit --class kr.co.vcnc.data.tada.batch.emr.LogJSONToParquet /home/hadoop/jar/batch-emr-assembly-0.1.0-SNAPSHOT.jar $d >> /tmp/LogJSONToParquet-$d 2>&1 &
done
```