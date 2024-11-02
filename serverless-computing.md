# Serverless 
 - Enables you to build scalable application quickly without managing servers. [Continuous Scaling]
 - Serverless apps are event driven and you are only charged when you code is executed [Low Cost]
 - AWS handles all the heavy lifting, you can focus on writing code instead of configuring servers

## Characteristics of serverless architecture
 - Event Driven and Asynchronous
 - Building blocks - Think of aws services as buildawsing blocks that can be integrated together to create and applications
 - Loosely coupled
 - Flexibility and Scalability - Services and components operate and scale independently of each other

## Lambda 
 - You are charged based on the number of requests, their duration and the amount of memory used by your lambda function
 - **Event-driven-architecture**
   - They can be triggered by other aws services or called directly from any web or mobile app [Services that can trigger lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html)
   - Triggered by Events like changes made to data in s3 or dynamodb
   - Triggered by user requests like you can use api gateway to configure an http endpoint allowing you to trigger a function at any time using an http request
   
 - Lambda are created with an execution role, which can be allowed to get access to s3, dynamodb etc.

### Lambda versioning
 - $LATEST is the latest version
 - use lambda versioning and aliases to point your applications to a specific version
 - update your aliases when updating the code
 - Weighted alias - you can distribute traffic between two versions

### Lambda concurrent executions
 - default is 1000 per regions
 - 429 http status code for too many requests exception, request troughput limit exceeded
 - request an increase in limit by submitting a request to AWS support center or configure reserved concurrency (guarentees a set number of executions which will always be available for your critical function, however this also acts as a limit)

### Lambda and vpc access 
  - its possible to enable lambda to access resources that are inside a private vpc
  - to do that we need to provide vpc config information, private subnet id, security group id
  - lambda creates ENI(elastic network interfaces) using IPs from the private subnets. The security group allows your function to access the resources in the vpc.

### Lambda data storage patterns
 - lambda are stateless and ephemeral (not used for application that need to run for longer than 15 minutes)

#### Lambda ephemeral storage
 - **/tmp**
   - temporary storage (provided in the *execution environment* of the lambda function)
   - 512 MB ,configurable up to 10 GB
   - dynamic read/write
   - like a cached file system - data can be accessed by multiple invocation of your function *sharing the execution environment* in order to optimize performance.
   - data is not persistent, only available for the lifetime of the execution environment
  
 - **Lamda layer**
    - additional libraries needed by the function can be included in your lambda deployment package, this increases the deployment package size.
    - (Best practice) add libraries and sdks as a layer that can referenced by multiple functions (best practice) (50MB zipped and 250 MB unzipped)
    - updates requires a new layer
    - shared across execution environments

 ### Lambda persistent storage
 - **S3** (object storage only) : cannot directly open and write data to objects. If you want to change data, you need to upload a new object
 - **EFS** - shared file system, data is persisted and can be *dynamically updated* , needs to be mounted by the function when execution environment is created. Lambda function must be in the same vpc as EFS File system

 ### Lambda environment variables and parameters
 - use environment variables are key value pairs to change function behavior without changing code
 - configureable parameters can be found under the configuration tab: memeory, ephemeral storage, function timeout. connect to other aws services like cloud watch, x-ray , vpc and efs file systems, monitoring, concurrency, permissions, function url, tags, 
 - they are key value pairs
 - they are locked when version is published

### Lambda event lifecycle and errors
 - when invoking a function you can invoke it *synchronously* or *asynchronously*
 - if a function errors, lambda automatically performs 2 **retries**, waits for 1 min for 1st try and then 2 min for 2nd try
 - if a function errors, you can also use **dead letter queues** : save failed invocations for further processing, are associated with particular version of a function, can be an event source for a function, allowing your to re-process events. SQS (holds failed events in the queue until they are retrieved) or SNS (fan-out to multiple destinations) can be used
 - **lambda destinations**, configure lambda to *send the invocation records which includes the response of the payload*(success or error) to another services (eg, for success send to SQS and error send to SNS)
     - SQS
     - SNS
     - lambda (invoke another lambda)
     - EventBridge
  
### Lambda deployment packaging options
- when working on aws console: a zip file is automatically created in the background
- zip file contains application code and optionally dependencies
- you can create a deployment package zip file on your own and use it, however if deployment package is more than 50 mb, upload the zip file on s3 in the region when you would like to create your function
- **lambda layers** is a distribution mechanism , where it can use used by multiple functions, reducing the size of deployment package, reduces the size of deployment package and enables function to initialize faster.

### Lambda performance tuning best practices 
- contol cpu capacity by controlling memory - **increasing memory** increases cpu
- 128 MB (default) , configurable upto 10 GB
- adding memory will improve performance if the function is either memory or cpu bound, it also reduces the duration that the function runs for

#### The execution environment
- download code -> configure -> static initialization (runs function static initialization code;imports libraries and dependencies) (adds the most amount of latency) -> Executes function
- three factors that contribute to latency
  - code : the amount of code that needs to run during initialization phase
  - function package size : including imported libraries, dependencies, and lambda layers
  - performance : libraries and other services that require connections to be set up (e.g. connections to database or s3)
- optimizing static initializations step
    - amount of code that needs to run during initialization
    - including imported libraies , depedencites and lambda layers
    - libraries and other services that require connections to be set up
    - **import only what you need** avoid importing entire sdk 🔥 (eg instead of import AWS from aws-sdk, import Dynamodb from aws-sdk/clients/Dynamodb)
      
## Step functions
 - great way to visualize serverless apps
 - automates trigger and track of each step of your workflow. The output of one step is often the input to the next.
 - logs the state of each step, so you can track when something goes wrong
 - **standard workflows**
   - long running, durable and auditable workflows that may run up to a year. Full execution history is available for up to 90 days after completion.
   - at-most-once
   - tasks are never executed more than once unless you explicitly specify retry actions
   - non-idempotent(cannot be processed multiple times and always causes a change in state) actions like processing payments
 - **express workflows**
   - great for high volume, event processing type workloads
   - at-least-once
   - executed more than once or require multiple concurrent execution
   - idempotent , no additional side effects. eg. reading data from db
   - **synchronous express workflow**, begins, waits until it completes, returns the result. eg confirm successful payment before sending an order.
   - **asynchronous express workflow**, begins, confirms the workflow has started and result is returned later. the result of the workflow can be found in cloud watch logs. eg. a messaging system
   
## API Gateway
 - Allows you to publish, maintan and monitor APIs
 - Supported API Types : Restful APIs and WebSocket APIs
 - Provides an endpoint to your applications running in AWS (Lambda, EC3, Elastic bean stalk, dynamodb, kenisis)
 - Supports multiple endpoints and targets
 - Supports multiple versions, so you can have different versions for prod, dev, test
 - Throttling - you can throttle API gateway to prevent your application from being overloaded by too many requests
 - Cloud watch - Everything is logged to cloud watch for eg. api calls, latencies and errors.
 - is serverless - low cost and scales automatically

### Advanced API gateway
  - you can import api's using the external definition files e.g. OpenAPI formerly known as swagger
  - when dealing with legacy applications which use SOAP, you can configure api gateway as a **SOAP web service passthrough**, or you can use api gateway to **convert the xml response to json**

### API Gateway mock endpoints
- allows developers to create,test and debug software by creating mock endpoints
- allows you to simulate the responses and behaviours that you would expect from the real API.

### API Gateway stages for testing deployed code
- logical reference, that references the lifecycle stage of api (eg. dev, prod, v3 etc)
- each stage has a unique invoke url
- **stage variables** are key value pair to change the behaviour of api (dynamic deployments)

### API request and response transformations 
- can transform http request and response using **parameter mapping**
- request transformation : change header, query string or request path
- response transformation : change the header or status code of an api response

### API Gateway caching and throttling
- caches responses for specified TTL (5 mins default), improves performance and latency
- throttling is to prevent your gateway from being overwhelemed with too many requests. 10K requests per second per region, 5k max concurrent requests across all api's per region. 429 too many error, you can request to increase the limit

## X-Ray 
- is a tool which helps developers analyze and debug **distributed applications**
- provides a service map which is visual representation of app
- you can use x-ray with aws services or also integrate with your apps written java, node, etc and it will monitor you api calls
- x-ray agent must be **installed** on ec2 instance. use sdk to **instrument** your application to send traces to x-ray
- x-ray sdk gathers information from request and response headers, the code in your application and metadata about the aws resources on which it runs and sends this trace data to x-ray eg. incoming http requests, error codes, latency data.
- reports on latency, http status code and errors
- needs **x-ray sdk** (for instrumentation) and **x-ray deamon**
- x-ray deamon needs to be installed on EC2 instances or on prem server or elastic beanstalk. But containers, install x-ray deamon in its own docker container on your ECS cluster along side your app
- **annotations**
    - when instrumenting your app you can record additional information about requests by using annotations
    - they are user-defined key value pairs
    - allow you to group related traces, filter or search using the key values


 









