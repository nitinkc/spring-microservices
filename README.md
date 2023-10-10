# Spring Cloud microservices

Clone all Projects together. Remember to switch to main branch instead of being into detached head.
```sh
git clone --recurse-submodules -j8 https://github.com/nitinkc/spring-microservices.git
cd spring-microservices

git submodule init
git submodule update 
```

Read the Spring Modules that comprise the
**[Spring Cloud](https://spring.io/projects/spring-cloud#:~:targetText=Spring%20Cloud%20provides%20tools%20for,distributed%20sessions%2C%20cluster%20state)**

Diagram demonstrating the major components of Spring microservices
**[Architecture](https://spring.io/img/homepage/diagram-distributed-systems.svg)**

[test](###3. Conversion microservice)

# Ports used in the project

|Application | Port |
-------------|-------
|Conversion microservice |8000, 8001, 8002, ..|
|Calculation microservice |8100, 8101, 8102, …|
|Netflix Eureka Naming Server | 8761|
|Netflix Zuul API Gateway Server | 8765|
|Zipkin Distributed Tracing Server |9411|
|||
|Test Config microservice | 8080, 8081, …|
|Spring Cloud Config Server | 8888|


### 3. Conversion microservice
Simple Spring Project that reads values from a DB and returns a conversion Factor for the consuming microservice

Group : com.microservices.learning.conversion

Artifact : conversion-service

Dependencies :

    • Web
    • Actuator
    • dev tools
    • config client
    • jpa
    • H2 Db

H2 DB Web Client URL : http://localhost:8000/h2-console/

### 4. Calculation micro service
Consumes Conversion micro service and performs x*value returned by conversion micro service operation to

Following technologies used :

* Rest template for consuming a rest microservices
* [Declarative REST Client: Feign](https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-feign.html)
* Ribbon Load Balancing
* Support for Eureka Naming Server (adds eureka client)

**The rest calls are distributed using Ribbon & Eureka**
* Zuul API Gateway integration

https://dzone.com/articles/microservices-communication-zuul-api-gateway-1


* [Spring cloud sleuth](https://cloud.spring.io/spring-cloud-sleuth/reference/html/) : Provides a unique id to all the requests. Place the dependency to all the projects needing a unique request id
* Zipkin distributed Tracing server

Group : com.microservices.learning.calculation

Artifact : calculation-service

Dependencies :

	• Web
	• Actuator
	• dev tools
	• config client

##### VM Argument to run multiple instances of a microservice
		-Dserver.port=8001

### 5. Eureka Naming Server

Group : com.microservices.learning.eureka

Artifact : eureka-naming-server

Dependencies :

	• Eureka Server
	• Config Client
	• Spring Boot Actuator
	• Spring Boot DevTools

Access Eureka Server UI at : <http://localhost:8761/>


### 6. Zuul API Gateway

Group : com.microservices.learning.zuul

Artifact : zuul-api-gateway

Dependencies :

	• Zuul
	• EurekaDiscovery
	• Actuator
	• DevTools

### 7. Zipkin Distributed Tracing server

Has to be started in a terminal after starting Rabbit MQ server

* Rabbit MQ Should be installed and run
```sh
/usr/local/sbin/rabbitmq-server# Zipkin Distributed Tracing Server

* Rabbit MQ Should be installed and run
```sh
/usr/local/sbin/rabbitmq-server
```
* Install Zipkin Distributed Tracing Server from
<https://zipkin.io/pages/quickstart>
* Run Zipkin server on terminal as
```sh
RABBIT_URI=amqp://localhost java -jar zipkin.jar
```

* Launch Zipkin server on terminal

<http://localhost:9411/zipkin/>

## Sequence of starting the servers
1. Eureka Naming Server
2. Zuul API Gateway
3. Conversion Service
4. Calculation service
5. Rabbit MQ
6. Zipkin tracing server
### Maven Dependencies

```sh
<!-- Spring Cloud Config Client -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<!--Feign Support Added-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
<!-- Ribbon Load Balancing added -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
<!-- Client for Eureka NamingServer -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<!-- Spring Cloud Sleuth -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<!-- Zipkin Distributed Tracing Server added -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
<!-- Rabbit Messaging Support added -->
<dependency>
	<groupId>org.springframework.amqp</groupId>
	<artifactId>spring-rabbit</artifactId>
</dependency>
```
