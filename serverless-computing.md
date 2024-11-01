# Serverless 
 - Enables you to build scalable application quickly without managing servers. [Continuous Scaling]
 - Serverless apps are event driven and you are only charged when you code is executed [Low Cost]
 - AWS handles all the heavy lifting, you can focus on writing code instead of configuring servers

  ## Lambda 
  - You are charged based on the number of requests, their duration and the amount of memory used by your lambda function
  - **Event-driven-architecture**
    - They can be triggered by other aws services or called directly from any web or mobile app [Services that can trigger lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html)
    - Triggered by Events like changes made to data in s3 or dynamodb
    - Triggered by user requests like you can use api gateway to configure an http endpoint allowing you to trigger a function at any time using an http request
   
  - Lambda are created with an execution role, which can be allowed to get access to s3, dynamodb etc.

  ### Lambda versioning
   - $latest is the latest version
   - use lambda versioning and aliases to point your applications to a specific version
   - update your aliases when updating the code
   - Weighted alias
   
  ## API Gateway
  - Provides an endpoint to your applications running in AWS
  - Throttling - you can throttle API gateway to prevent your application from being overloaded by too many requests
  - Cloudwatch - Everything is logged to cloudwatch for eg. api calls, latencies and errors. 
