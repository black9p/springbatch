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
