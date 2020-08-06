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

## Decider
- from() : 이벤트 리스너 역할. 일치상태에 따라 to() 호출.

## JobParameter와 Scope
- JobParameter : 스프링배치 내부 혹은 외부에서 받아 쓸수 있는 파라미터값으로 Scope를 반드시 지정해주어야 한다.
- Scope : @StepScope, @JobScope
    - Job Parameter를 사용하기 위해선 반드시 @StepScope, @JobScope로 Bean을 생성해야 한다.
    - @JobScope : Step 선언문에서 사용가능. Double, Long, Date, String 가능
    - @StepScope : Tasklet, ItemReader, ItemWriter, ItemProcessor에서 사용가능.

## @StepScope, @JobScope
- Bean 생성 시점을 지정된 Scope가 실행되는 시점으로 지연시킴
- Job Parameter의 Late Binding이 가능. 비지니스 로직 처리단계에서 Job Parameter를 할당시킬수 있다.
- 동시성 이슈 발생을 예방할 수 있다.

## Chunk
- Chunk : 각 커밋 사이에 처리되는 row수
- Spring Batch는 Chunk 단위로 트랜잭션을 수행한다
- Reader와 Processor에서 1건씩 다뤄지고, Writer에선 Chunk 단위로 처리된다.

## Page Size vs Chunk Size
- Chunk Size : 한번에 처리될 트랜잭션 단위
- Page Size : 한번에 조회할 Item 양
- Chunk size 100, Page size 10 일때, 10번의 조회가 일어나고 이것은 성능문제 및 JPA 영속성 컨텍스트가 깨지는 문제도 발생시킬 수 있다.
- 2개의 값(Page Size == Chunk Size)을 일치시키는 것이 보편적으로 좋은 방법이다.

