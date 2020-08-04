# springbatch
Study Spring Batch

## Install Mysql
```shell script
$ docker run --name batchdb -p 13306:3306 -e MYSQL_ROOT_PASSWORD=1234 -d mysql
$ docker exec -it batchdb bash

# mysql -u root -p

mysql> CREATE DATABASE test;
```

## Spring Batch Metadata
- 인텔리제이 파일찾기 (schema-mysql.sql) 내용을 test db에 추가

## Next
- StepNextJobConfiguration

### 지정한 배치 Job만 실행되도록
```yaml
spring.batch.job.names: ${job.name:NONE}
```
- application.yml
- Program arguments로 job.name 값이 넘어오면 해당 값과 일치하는 Job만 실행. 없으면 NONE 할당.

```shell script
$java -jar batch-application.jar --job.name=simpleJob
```
