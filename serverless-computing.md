# Serverless 
 - Enables you to build scalable application quickly without managing servers. [Continuous Scaling]
 - Serverless apps are event driven and you are only charged when you code is executed [Low Cost]
 - AWS handles all the heavy lifting, you can focus on writing code instead of configuring servers

## Characteristics of serverless architechture
 - Event Driven and Ashynchronous
 - Building blocks - Think of aws services as builing blocks that can be integrated together to create and applications
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

## Step functions
 - great way to visualize serverless apps
 - automates trigger and track of each step. The output of one step is often the input to the next.
 - logging, so you can track when someting goes wrong
 - **standarad workfolows**
  - long running, durable and auditable workflows that may run upto a year.
  - tasks are never executed more than once unless you explicitly specidy retry actions
  - non-idemptent(cannot be processed multiple times and always causes a change in state) actions like processing payements
 - **express workflows**
  - great for high volume, event processing type workloads
  - executed more than once or require multiple concurrent execution
  - idempotent , no additional side effects
   - **synchronous express workflow**, begins, waits until it completes, returns the result
   - **asynchronous express workflow**, confirms the workflow has started and result is returned later. the result of the workflow can be found in cloudwatch logs.
   
## API Gateway
 - Provides an endpoint to your applications running in AWS
 - Throttling - you can throttle API gateway to prevent your application from being overloaded by too many requests
 - Cloudwatch - Everything is logged to cloudwatch for eg. api calls, latencies and errors. 
