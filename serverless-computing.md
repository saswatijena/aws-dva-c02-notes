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
 - if a function errors, you can also use **dead letter queues** : save failed invocations for further invocations , are associated with particular version of a function, can be an even source for a function, allowing your to re-process events. SQS (holds failed events in the queue until they are retrieved) or SNS (fan-out to multiple destinations) can be used
 - **lambda destinations**, configure lambda to send the invocation records(success or error) to another services (eg, for success send to SQS and error send to SNS)
     - SQS
     - SNS
     - lambda (invoke another lambda)
     - EventBridge
  
### Lambda deployment packaging options
- when working on aws console: a zip file is automatically created in the background
- zip file contains application code and optionally dependencies
- you can create a deployment package zip file on your own and use it, however if deployment package is more than 50 mb, upload the zip file on s3 in the region when you would like to create your function
- **lambda layers** is a distribution mechanism , where it can use used by multiple functions, reducing the size of deployment package, reduces the size of deployment package and enables function to initialize faster.

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
 - Provides an endpoint to your applications running in AWS
 - Throttling - you can throttle API gateway to prevent your application from being overloaded by too many requests
 - Cloud watch - Everything is logged to cloud watch for eg. api calls, latencies and errors. 
