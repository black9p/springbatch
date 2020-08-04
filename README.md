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

## Conditional Flow
- StepNextConditionalJobConfiguration

- on() : 캐치할 ExitStatus 지정. * 일경우 모든 ExitStatus 지정. BatchStatus가 아니라 ExitStatus.
- to() : 다음으로 이동할 Step 지정
- from() : 일정의 이벤트 리스너 역할.
- end() : FlowBuilder 반환 end(on(*) 뒤에있다)와 FlowBuilder를 종료하는 end(build() 앞에있다) 2개가 있다.

### Batch Status vs Exit Status
- Batch Status : Job 또는 Step의 실행결과를 Spring에서 기록할 때 사용하는 Enum
- Exit Status : Step 실행 후 상태