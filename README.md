# Spring Cloud Sleuth with Zipkins
Spring Cloud Sleuth is a distributed tracing framework for a microservice architecture in the Spring ecosystem.

Zipkins is a distributed tracing system usually used to troubleshoot latency problems in service architectures.

## Example modules

- **Eureka Server**: Acts as a service registry and runs on port 8761.
- **Address Service**: A REST service that has a single endpoint of /address/{customerId} and runs on port 8070.
- **Customer Service**: A REST service that has a single endpoint of /customer/{customerId} and runs on port 8060.
- **Portal Service**: A REST service that has a single endpoint of /fullDetails/{customerId} and runs on port 8050. This service internally calls address-service and customer-service to get data and combines them before the response.
- **Gateway**: Single point of entry to our microservice architecture, runs on port 8080.

## How to run this example

1. Clone the repository
2. Starts the above applications
3. Run Zipkin server (runs on port 9411):
    1. curl -sSL https://zipkin.io/quickstart.sh | bash -s
    2. java -jar zipkin.jar
4. Hit the endpoint `http://localhost:8080/portal-service/fullDetails/12`.
5. Navigate your browser to `http://localhost:9411/zipkin/` to access Zipkin home page and click "Find Traces".



## How to integrate Spring Cloud Sleuth in your project

- Add the dependency

`````<dependency>`````
`````<groupId>org.springframework.cloud</groupId>`````
`````<artifactId>spring-cloud-starter-sleuth</artifactId>`````
`````</dependency>`````

- Add the following property in each service to specify its application name:

`spring.application.name=customer-service`


## How to integrate Zipkins in your project

- Add the dependency

`````<dependency>`````
`````<groupId>org.springframework.cloud</groupId>`````
`````<artifactId>spring-cloud-starter-zipkin</artifactId>`````
`````</dependency>`````

- Add the following properties in each service:

`spring.zipkin.baseUrl= http://localhost:9411/`

It tells Spring and Sleuth where to push data to. Also, by default, Spring Cloud Sleuth sets all spans to non-exportable. This means these traces (Trace Id and Span Id) appear in logs but are not exported to another remote store like Zipkin.

`spring.sleuth.sampler.probability=100`

In order to export spans to the Zipkin server, we need to set a sampler rate. A value of 100 means all the spans will be sent to the Zipkin server too.





