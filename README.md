# microdockers
Reference implementation of building microservices based application around ephemeral Docker

The idea is to have each Microservice be made available as an Docker image. Each such Docker image is only run when it is needed. If the service is not needed, then there is NO footprint of the service in the overall infrastructure. The footprint represents the runtime Docker instance that executes a particular microservice. 

**Service Ingredients**

We model a simple Order processing flow. Following are the microservices included in the flow :-
  1. OrderAcceptService
  2. OrderCheckService
  3. OrderRegisterService
  4. OrderGenerationService

Each service is Java Spring Boot application. We will build a workflow that will take the lastest code base of each Service and bundles it with a pre-configured Docker Base Image. The pre-configured Docker image is based on any of the available docker image from Docker Hub. 

If each Service wants to call another, it will attempt to discover the relevant service through a Service Locator. 
The Caller Service registers a Callback handler and pushes the data to be passed to the other service on to a Data Store. 

The Service Locator locates the appropriate Docker image and Tag that is needed to run the service implementation. A new Container is run with the identified Docker Image. The data passed by the Caller service is made available to the newlycreated Docker instance through ENVIRONMENT variables.  Usually the Environment variables will hold the access key and credentials for the Data Store. The Java Spring Boot application will use the Environment variables to access the remote Data Storage to access the parameters passed by the Caller. 

On successful execution of the applicatiom, the service returns the data back to the Data Storage, and performs the callback invocation similar to how Caller used. 

The core idea is to test a solution that will trigger creation of a Container instance on each Transaction request in the Application. 

**Advantages of this architecture**

1. No footprint of service if its not needed. This leads to better utilization of hardware. 
2. Provides a better view of how certain services are needed, and better manage the Capacity for each service
3. Each Service can run anywhere, and on any infrastructure. 
4. Each Service has no knowledge of the implementation and the runtime location of the Service

I think through this reference implementation, we can arrive at new type of software design techniques that could be helpful for building scalable systems. 
