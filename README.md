# Spring Boot + Spring Data JPA + MySQL example (TPCF)


## Technologies used:
* Spring Boot 3.1.2
* Spring Data JPA (Hibernate 6 is the default JPA implementation)
* MySQL 8
* Java 17
* Maven 3
* JUnit 5
* Spring Test using TestRestTemplate
* Docker, [Testcontainers](https://testcontainers.com/) (for Spring integration tests using a MySQL container)
* cf cli

## How to run it
```
$ cd spring-data-jpa-mysql

## Run MySQL container for testing
$ docker run --name c1 -p 3306:3306 -e MYSQL_USER=testuser -e MYSQL_PASSWORD=password -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -d mysql:8.1

## connect to docker container
```
docker exec -it c1 mysql -uroot -p
enter password
show databases;
use mydb;
select * from books;
```

# Skip test, the Testcontainers takes time
$ ./mvnw clean package -Dmaven.test.skip=true

$ ./mvnw spring-boot:run

```

## How to test it
Post
```
curl -X POST -H "Content-Type: application/json" -d '{"title":"Book E", "price":49.99, "publishDate":"2023-04-01"}' "http://localhost:8080/books"
```
Get
```
curl -s http://localhost:8080/books | python3 -m json.tool
```

### Create a MySQL Service Instance in TPCF
Choose the appropriate MySQL service plan (db-small, db-leader-follower, db-small-ha, db80-small) from the Pivotal MySQL (p.mysql) service offerings and create a service instance:

```
cf marketplace
cf create-service p.mysql SERVICE_PLAN SERVICE_INSTANCE_NAME
cf create-service p.mysql db-small mysql-service
```


### Bind Service Instance to Your Application
Bind the MySQL service instance to your deployed application springdatamysql:

```
cf bind-service springdatamysql mysql-service
```

### Check env variables
```
cf env springdatamysql
```