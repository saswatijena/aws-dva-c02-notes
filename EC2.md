# Elastic Compute Cloud (EC2)

- Secure, resizable compute capacity in the cloud
- its ike a VM hosted in AWS instead of your own data center 🔥
- Designed to make web-scale cloud computing easiter for developers 
- The capacity you want when you need it 🔥
  - no wasted capacity, allows to grow and shrink your infrastructure to suit the needs of your capacity
  -  pay only for what you use 🔥
- You re in complete control of your own instances
- instance type determines the hardware of the host computer (diff compute, memory. storage capabilites) 🔥

## Pricing models

  - **On Demand**
      - pay by the hour or the second depending on the type of instance you run
  - **Reserved**
      - reserved capacity for one or three years. Up to 72% discount on the hourly charge 
      - great for applications with steady state
      - standard RI
      - convertible RI : has option to change to different reserved instance type
      - scheduled RI: launch within the time window you define
  - **Spot**
      - Purchase unused capacity at a discount up to 90%
      - prices fluctuate with supply and demand
      - great for application that have flexible start and end times
      - great for urgent ened for large amounts of additional computing capacity
  - **Dedicated**
      - a physical EC2 server dedicated for your use. Most expensive
      - great for compliance and licensing
      - can be purchaed on demand or reserved
  - **Savings plan**
      - commit to one or three years to get discounted prices
   

# Elastic Block Store (EBS Volumes)

  - storage volumes you can attach to EC2 instances
  - when you first launch EC2 it has at least 1 EBS volume which runs the operating system
  - designed for mission critical workloads
  - highly available
  - highly scalable

## Types of EBS 🔥 [EBS Volume Types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html)

  - **General Purpose SSD (GP2)**
    - suitable for boot disks and general applications
    - 3 iops per GiB
    - upto 16K IOPS per volume
    - 99.9% durability
  - **General Purpose SSD (GP3)**
      - suitable for boot disks and general applications
      - baseline 3K iops per GiB
      - upto 16K IOPS per volume
      - 99.9% durability
      - latest generation 
  - **Provisioned IOPS SSD (io1)**
      - High performance and most expensive
      - suitable for online transaction processing and latency-sensitive applcations
      - 50 iops per GiB
      - upto 64K IOPS per volume
      - 99.9% durability
      - I/O intensive applications (large databases and latency sensitive applications)
  - **Provisioned IOPS SSD (io2)**
      - High performance
      - latest generation, same as io1
      - 500 iops per GiB
      - upto 64K IOPS per volume
      - 99.999% durability (more durable than io1) 🔥
      - I/O intensive applications (large databases and latency sensitive applications)
  - **Provisioned IOPS SSD io2 Block Express**
      - 4x throughpuut IOPS and capacity of regular io2 volumes
      - for mission critical, high-performance applications
      - 256K IOPS per volume
      - 99.999% durability
      - AWS SAN in the cloud offering
  - **Throughput Optimized HDD (st1)**
      - low cost HDD volume
      - frequenctly accessed throughput intensive workloads
      - big data, date warehouses , ETL and log processing
      - cannot be a boot volume
      - max throughput isn 500MB/s per volume
      - 99.9% durability
  - **Cold HDD (sc1)**
      - Lowest cost option
      - data which requires fewer scans per day
      - max throughput isn 250MB/s per volume
      - cannot be a boot volume
      - 99.9% durability
 
## Encyption 🔥

  - Default encryption: If encription by default is set on your account by your account admin, you cannot create unencrypted EBS volumes
  - Encrypted snapshots: If you can create an EBS volume from an encrypted snapshot, then you will get an encrypted volume.
  - Unencypted snapshots: encyption option is only availaible if default encryption is not set


# Elastic Load Balancer 🔥

  A load balancer distributes network traffic across a group of servers
  we can increase capacity as needed

  - **Application load balancer (HTTP and HTTPS)** - Operate at layer 7 and are application aware
  - **Network load Balancer (TCP and high performance and expensive)** - Operates at Layer 4
  - **Classic load balancer (HTTP/HTTPS and TCP )** [Legacy option]
  - **Gateway load balancer** 🆕 - provides load balancing for third-party virtual applications, like firewalls, intrusion detection and prevention systems (cisco, palo alto etc)


  ## What is 7 Layer Model [!not important]

  A conceptual framework which describes the functions of a network. Begining with the application layer, which directly serves the end user, down to the physical later.

  
| Layer | Description |
| ------------- | ------------- |
| Layer 7 (Application) | What the end user sees, HTTP, web browsers  |
| Layer 6 (Presentation) | Data is in a usable format. Encryption, SSH  |
| Layer 5 (Session) | Maintains connections and sessions |
| Layer 4 (Transport) | Transmits data using TCP and UDP |
| Layer 3 (Network) | Logically rotues packets, based on IP address |
| Layer 2 (Data Link) | Physically transmits data based on MAC addresses |
| Layer 1 (Physical) | Transmits bits and bytes over physical devices |

## What is x-forwaded-for 🔥
if you need the IPV4 address of your end user, look for the **X-Forwaded-For header**, since the server will recieve the header of the loadbalancer. supported by application and classic load balancer.

# Route 53 
amazons DNS service 
**hosted zone** : container for DNS records for your domain
**alias** : allows you to route traffic addressed to the zone apex, or the top of the DNS namespace
**a record** : allows you to route traffic to a resource, such as a web server, using and IPv4 address.

# AWS CLI   

  - Always give your users the minimum amount of access required to do their job
  - Best practice to create groups and assign users to groups
  - group permissions are assigned through policies
  - you only see secret access key once, if you loose it you would need to regenerate them and run ``` aws configure ``` again
  - dont share key pairs
  - aws cli is supported in linux, winows and mac and ec2 instances (comes with aws cli installed)
  - if you see errors like timed out or errors related to too many results being returned, adjust the pagination in of CLI  --page-size , CLI still retrieves the full list, but performs a larger number of API calls in the background and retrieves a smaller number of items with each call.

# EC2 to S3 or other AWS resources

  - IAM Roles are the preffered option to access AWS resources
  - Avoid hardcoding your credentials
  - policies control a role's permissions
  - you can attach or dettach roles from running ec2 instances
  - you can update a policy for a role and it will take effect immediately

# RDS 

  ## Features 

  - Multi AZ 
  - Failover capability
  - Automated back ups
  - types : sql server, arora, portgres, oracle, maria
  - designed fro OLTP
  - not suitable for OLAP (redshift should be used instead)

  ## What is the difference b/w OLTP and OLAP

  OLTP (Online transaction processing)
  - large number of small transactions in real time (orders, banking transactions, payments, booking systems)

  OLAP (Online analytics processing)
  - processes complex queries to analyze historical data (analyzing net profit figures from past 3 years, sales forecasting etc)

## Multi AZ

  - (exact copy of prod database in another availibility zone)
  - provides resilience
  - its for disaster recovery, not for improving performance (in the event of failure, RDS will failover to the standby instance)

## What is read replica

  A read only copy of your primary database, used to improve performance, which can be used to run reports etc w/o impacting the customer performance
  - can be located in the same AZ or diff AZ or altogether a diff region
  - each replica has different endpoints
  - primarily used for scaling read performance
  - requires atomatic back ups
  - multiple read replicas are supported upto 5

## Ways to backup RDS
  ### Manual Databse snapshot
  - are user initiated
  - no retention period, live forever
  - back up to a known state
  - no transaction logs

  ### Automated backup
  - enabled by default
  - point in time recovery (with the help of snapshots and transaction logs)
  - perfomes full daily back up (stored in s3 [free]) and store transactions logs
  - you defined the back up window
  - retention period of 1-35 days

replaced version is a new RDS enpoint with a new DNS enpoint

  ## Encyption at rest   
  - can be enabled at creation time
  - integrated wtih KMS AES-256
  - encrypts all db storage ( automated back ups, snapshots, logs, read replicas etc)
  - you cant enable encryption on an unencrypted RDS DB instance.
  - workaround to encrypt an existing db : take a snapshot , create an encrypted snapshot and perfome a database restore

  ## Increasing scalability using RDS proxy
  - sits b/w user and db. it pools and shares db connections to assist with app scalability and db efficiency.
  - its serverles and scales automatically
  - preserves application connection during failover and routes requests to standby quickly
  - deployble over multi-az for protection
  - upto 66& faster failover times

# Elasticache
 - in memory cache
 - key value data store
 - designed to improve db performance for read heavy db workloads
 - really useful for storing sessiond data for ditributed applications
 - great option when db is ready heavy
 - not helpful for db with heavy write loads, instead scale db
 - not helpful for olap queries (redshift instead)

two options for elasticache
  ## memchached
  - for basic *simple object caching*
  - scales horizontally
  - not *persistence* , multi az or failover
  - no support for data sorting and ranking

  ## Redis
  - sophisticated solution with enterprise fetures like *persistence*, replication, multi az, failover etc
  - allows data sortng, ranking such as gaming leader borads
  - advnaced data types, such as *lists and hashes*


# Memory db for redis 
- in memory db
- multi az and has transaction log for recovery and durability
- can be used as primary database
- ultra fast performance
- great for ultra fast redis compatible, high performance, low latency, eg online gaming company

# Diff b/w elasticache and memory db for redis 
elacticache
 - in memory db
 - fast but not ultra fast
 - eg. for storig session data

memorydb
  - used as primary db
  - ultra fast
  - online gaming company which required massive concurrent users sharing digital assets
  - can store entire data set in memory

# System management parameter store
- store parameters for applications like licence keys, cofiguration variables etc
- can be store in plain text or encrypted
- can be used with EC2, clould formation, lambda etc

# AWS Secrets Manager
- used to store secrets
- can **automate rotation** (30, 60, 90 or custom # of days. create new of existing lambda for rotation)
- allows to secure using fine grain permission and encypt using KMS (Customer master keu, can encypt upt 4kb of data)
- can be used to strore database secrets (db username and pass, server address, db name and port)
- secrets for RDS db, redshift cluster, documetdb, other db, api keys

# EC2 Image builder 
  - automates the process of creating and maintaining your images
  - it a 4 step process
  - base OS -> add software (apache, tomcat, python etc) -> run tests on new image to see if it boots correctly -> distribute the image ot o the regions of your choice (default is the region you are operating on)

# Using AMI in a diff region
  - ami only exists in a single region
  - can be used in diff region by creating a copy
  - when copying AMI following options are supported (you can apply encryption during copying, but cannot remove encryption)
      - unecrypted to unencypted AMI ✔️
      - encrypted to encypted AMI ✔️
      - unencrypted to encrypted AMI ✔️
      - encrypted to unencrypted AMI ❎
   
# s3 (simple storage sevice)
  - is an object based storage 🔥
  - unlimited storage 🔥
  - not for OS or DB storage 🔥
  - 0 - 5tb 🔥
  - files are stored in buckets
  - all accounts share the same namespace, so all buckets should be globally unique 🔥
  - https://<bucket-name>s3.<region-name>.amazonaws.com/<key-name> 🔥
  - 200 status code on successful upload 🔥
  - **key** is the name of the object , **value** is the data iteself(bytes) , **version id**, **metadata** (data about data for eg. content-type, last modified etc) 🔥
  - higly available and highly durable
  - offers tiered storage
  - lifecycle management (transition objects to cheaper tier or delete them)
  - versioning


    ## security
    - all newly created buckets are private
    - server side encyption - set default encryption on a bucket
    - ACLS
       - define which AWS accounts or groups are granted access and the type of access
       - Grant access to object level
    - bucket policies
       - specifies which actions are allowed or denied
       - applied at bucket level
       - written in json
     
    s3 provides access logs which is not enabled by default. 
   
    ## s3 storage classes 🔥
    all are 99.99% availability and 11 9's durability except the once where specificall mentioned below
    - s3 standard
      - high availibility and durability
      - designed for frequent access
      - websites, content distribution, data analytices
     
    - s3 standard-infrequent access (S3-IA)
      - ong term, infrequently accessed, critical data but access rapidly
      - pay to access data
      - minimum storage duration is 30 days
      - backups, disaster recovery
      - 99.9% availibility
     
    - s3 one zone-infrequent access
      - long term, infrequently accessed, non-critical data
      - like s3-ia but data is stored redundently within a single AZ
      - 20% les than regular s3-ia
      - minimum 30 days duration
      - 99.5% availibility
    
    - s3 glacier instant retrival
      - cheap
      - good for long lived data, accessed once per quarter and needs millisecons retireval time
      - you pay each time to access data
      - 90 days storage minimum duration
      - 99.9 % availability
    
    - s3 glacier flexible retrival
      - cheap
      - good for long term archiving that needs to be accessed occasionally within a few hours or minutes
      - you pay each time to access data
      - 90 days storage minimum duration
     
    - Glacier deep archive
      - Rarely accessed data archiving, with a default retrieval time of 12 hours
      - financial data access once or twice a year
      - 180 days minimum storage
     
    - S3 - Intelligent tiering
      - unknow or unpredictable access patterns
      - automatically moves your data to the most cost effective tier based on how frequently you access each object
      - minimum 30 days duration
     
    ## s3 encryption
    - Encryption in Transit (SSL/TLS , HTTPS)
   
    - Encyption at rest (Server side encryption)
      - SSE-S3 - s3 managed keys, enabled by default (AES 256-bit)
      - SSE-KMS - AWS key managements service
      - SSE-C - Customer provided keys
     
    - Encryption at rest (client side encyption)
      you encrypt the files before uploading the files on s3

    - how will you enforce encryption with a bucket policy
      explicitly deny requests that do not include the z-amz-server-side-encryption parameter in the request header. Deny requests that do not use aws:SecureTransport to enforce the use of HTTPS/SSL


    ## CORS Configuration
    can be used to allowed resources in one s3 bucket to access resources located in another s3 bucket

# Cloud front
  - content delivery network
  - system of distributed servers to deliver content at hign speed
  - good for low latency and high data transfer speeds.
  - **cloud front edge location** : location where the content is cached. spearate to an AWS Region/AZ (they are not read only, you can put object on them)
  - **cloud front origin** : origin of the files that the distribution will server. can be and s3 bucket, ec2 instance, ELB or Route 53 or your own data center
  - **cloud from distribution**: the name given to the origin and configuration settings for the content you wish to distribute using CDN
  - TTL is 1 day, you can clear the cache yourself before TTL is up but you will be charged
  - used with another service called **s3 transfer acceleration** (faster transfer of files)
  - OAI (Origin access identity) is a special cloudfront user that can access the files in our bucket and server them to users, this has chaned to OAC [origin access control list](https://d3vp103kmhys3j.cloudfront.net/1620297128238-cute_dog.jpg)
  - OAI allows us to restrict access to the content of our bicket, so that all users must use the cloudfrom URL instead of a direct s3 url
  - what are cloud fornt allowed methods
      - GET, HEAD  - read
      - GET, HEAD, OPTIONS - read
      - GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE - read and write
   
  - AWS certificate manager is used to create and manage SSL/TLS to enable secure connections using https. When using ACM with cloudfront, the certificate must be created in the us-east-1 region.
   
# Athena
  - interactive query 
  - enables you to run sql on s3
  - useful for quering logs stored in s3, performing cost analysis, generate business reports, run queries on click stream data


# Serverless computing

  ## Lambda
  - are **event-driven** and you are only charged when your code is executed
  - provides continuos scaling and scales automatically
  - enables you to build scalable apps quickly w/o managing any servers
  - AWS handles all the heavy lifting, so you can focus on writing code
  - lambda, SQS, SNS, API gateway
  - they are usually stateless, but dynamo db and s3 can be used to store data
  - lambda triggers: dynamod db, kinesis, sqs, alb, api gateway, clouldforon, s3, sns, ses, clould formation etc.

  ## versioning
  - $latest is the latest version
  - use lambda versioning ans aliases to point your applications to a specific version
  - update your aliases when updating the code
    
  ## concurrent executions
  - default is 1000 per regions
  - 429 http status code for too many requests exception, request troughput limit exceeded
  - request an increase in limit by submitting a request to AWS support center or configure reserved concurrency (guarentees a set number of executions which will always be available for your critical function, however this also acts as a limit)

  ## lambda and vpc access 
  - its possible to enable lambda to access resources that are inside a private vpc
  - to do that we need to provide vpc id, private subnet id, security group id
  - lambda creates ENI(elastic network interfaces) using IPs from the private subnets. The security group allows your function to access the resources in the vpc.
  
  ## API gateway
  - service which allows to publish, maintain, monitor and secure api's at any scale
  - supported api types
      - RESTful apis : optimized for stateless, serverless workloads
      - Webscocket apis : for real time, two way, stateful communication eg. chat apps
  - allows you to connect ot apps running on lambda, ec2, dynamodb etc
  - supports multiple endpoints and targets
  - supports multiple version
  - logs calls, latencies, error rates to cloudwatch
  - helps you manage traffic with throttling so that backend operations can withstand traffic spikes and denial of service attacks

 ### Advanced api gateway
  - you can import api's using the external definition files eg. open api formerly known as swagger
  - when dealing with legacy applications which use SOAP, you can configure api gateway as a SOAP web service passthrough, or you can use api gateway to convert the xml response to json

### api gateway mock endpoints
- allows developers to create,test and debug software by creating mock endpoints

### api gateway stages for testing deployed code
- logical reference referencing the lifecycle stage of api (eg. dev, prod, v3 etc)
- stage variables are key value pair to change the behaviour of api  (dynamic deployments)

### api request and response transformations 
- can transform api request and response using paramter mapping
- request transformation : change header, query string or request path
- response transformation : change the header or status code of an api response

### api gateway caching and throttling
- caches responses for specified TTL (5 mins default), improves performance and latency
- throttling is to prevent your gateway from being overwhelemed with too many requests. 10K requests per second oer regions, 5k max concurrent requests across all api's per region. 429 too many error, you can request to increase the limit


  ## characteristic of event-driven architecture
  - asynchronous
  - loosely coupled
  - single prupose functions
  - helps with independent scalability and availibility
  - event source (s3, dynamodb) --> Event Router (EventBridge) --> Event destination (SNS, Lambda)

  ## step functions
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
   
 ## lambda data storage patterns
 - **ephemeral storage** /tmp : temporary storage : 512 MV , configurable up to 10 gb, only available for the lifetime of the execution environment 
 - storing lambda libraries : additional libraries needed by the function can be included in your lambda deployment package, this increases the deployment package size.
 - add libraries and sdks as a layer that can referenced by multiple functions (best practice) (50MB zipped and 250 MB unzipped) updates requires a new layer
 - s3 (object storage only) : cannot directly open and write data to objects
 - EFS - shared file system, data is persisted and can be dynamically updated , must be in the same vpc

## lambda environment variables and parameters
- use environment variables are key value pairs to change function behavior without changing code
- configureable parameters: memeory, ephemeral storage, connect to other aws services like cloud watch, x-ray , vpc and efs file systems, monitoring, concurrency

## Lambda event lifecycle and errors
- when invoking a function you can invoke it synchronously or asynchronously
- if a function errors, lambda automatically performs 2 retries, waits for 1 min for 1st try and then 2 min for 2nd try
- dead letter queues : save failed invocations for further invocations , SQS or SNS (fan-out to multiple destinations) can be used
- lambda destinations, configure lambda to send the invocaiton records(success or error) to another servers (eg, for success send to SQS and error send to SNS)
    - SQS
    - SNS
    - lambda (invoke another lambda)
    - EventBridge

## lambda deployment packaging options
- console: a zip file is automatically created in the background
- zip file contains app code and dependencies (optionally)
- if deployment package is more than 50 mb, upload on s3
- lambda layers is a distribution mechanism , where it can use used by multiple functions, reducing the size of deployment package

## lambda performance tuning best practices 
- contol cpu capacity by controlling memory - increasing memory increases cpu
- 128 MB to 10 GB
- optimizing static initializations step
    - amount of code that needs to run during initialization
    - including imported libraies , depedencites and lambda layers
    - libraries and other services that require connections to be set up
    - avoid importing entire sdk 🔥
 
## x-ray 
- is a tool which helps developers analyze and debug distributed applications
- provides a service map which is visual representation of app
- x-ray agent must be installed on ec2 instance. use sdk to instrument your application to send traces to x-ray
- reports on latency, http status code and errors
- needs x-ray sdk (for instrumentation) and x-ray deamon
- x-ray deamon needs to be installed on EC2 instances or on prem servers. But containers, install x-ray deamon in its own docker container on your ECS cluster along side your app
- annotations
    - when instrumenting your app you can record additional information about requests by using annotations
    - the are kye value pairs
 
# KMS and encrption
- key management service
- cmk (customer managed key) -
- set up cmk : crated alis and description -> choose key material option -> define Key administrative permissions (iam users and roles that can administer but not use the key) -> key usage permissions (iam users and roles that can use the key to encrypt and decrypt data)
- **aws managed cmk** used on your behald with the aws services integrated with kms  like s3, redis etc
- **customer managed cmk** - you manage yourself
- data key : encryption keys that you can use to encrypt data. you can use scmk to generate, encrypt and decrypt data keys
- ```aws kms encrypt``` to encrypt plain text to ciphertext
- ```aws kms decrypt``` to decrypt
- ```aws kms re-encrypt``` takes an encryped file and decrypts it in memory, does not saves it and encrypts it again, if you want to encrypt using a diff cmk eg. manually rotate cmk
- ```aws kms enable-key-rotation``` automatically rotates on an annual basis, store previous versions of key if you wanted to decrypt old files
- ```aws kms generate-data-key``` returns a plain and cyper-text version of data key , uses the cmk to generate the data key to encrypt data > 4kb
- **envelope encryption process**
  - encryption the key which encrypts the data.
  - CMK is userd to encrypt the data key
  - only encrypted data key is sent over network, avoiding the need to transer large amounts of data to kms
  - used fror encrypting anything over 4kb
 
- **AWS certificate manager** used to create SSL/TLS certificates
    - secures connections over https
    - when using acm with cloud from, you must create the certificate in us-east-1

## SQS 
- a distributed message queue service
- **decouple applications components** so that they can run independently
- messages can contain 256 kb of text in any format
- eg when the producer is producing results faster than consumer, network connectivity gets inturrupted
- its **pull based**, not pushed base (messages are pulled out, not pushed out)
- retention is max 14 days, default 4 days
- types of sqs
    - standard queues
        - unlimited transactions
        - guarantees a message will be delivered at least once
        - best effect ordering , meaning a couple of messages may not be in order and may have duplicates
     
    - FIFO
        - ordering is strictly preserved
        - no duplicates
        - 300 Transactions per second limit
        - guarantees a message will be delivered at least once
        - great for banking applications
     
- visibility timeout is the amount of time that the message is invisible in the sqs queue after a reader picks up that message, default is 30s
    - if the job is not processed in the 30s then another reader will pick up the message
    - if the reader takes longer than 30s then you would need to increase the visibility time out (max 12hrs)
 
- short polling
    - can result in lot of empty responses, expensive 
- long polling
    - does not returns a response until a response arrives or the long poll times out, can save money!
 
- sqs delay queues
    - postpone delivery of new messages to a queue for a number of seconds
    - messges sent remain invisible to consumers for the duration of delay period
    - default delay is 0 seconds, max is 900
    - for standard queues this setting doesnt affect the delay of messages already in the queue, only new messages
    - for FIFO, this affects the delay of messages already in the queue
    - eg for large distribution apps
 
- managing large sqs messages
    - over 256kb upto 2gb in size
    - use s3 to store these messages
    - use amazon sqs extended client library for java
    - also need aws sdk for java

# SNS 
- webservice to set up, operate and send notifications from the cloud
- you can use it to create push notifications
- can be used to trigger another lambda or another aws service like sqs etc
- a **pub-sub** model, push based not pull based

# SES (Simple Email Service)
- designed to help marketing teams and app developers to send marketing, notifications, transactions emails
- you can use it to send automated emails, eg, confirm online purchase
- can be used to trigger lambda or sns
- can be used for both incoming and outgoing email
- email only
- icoming emails saved to s3
- is not subscription based

# Kinesis
- kinsesis streams
    - data streams
      - retains data forfrom 24 hrs to 365 days
      - retains data in the form of shards
      - each shard is a sequence of one or more data records and provides a fixed unit of capacity
      - you can increase the capacity by increasing the number of shards
      - order of records is always maintains
    - video streams    
- kinesis data fire hose
    - no data retention
    - no shards and no consumers
    - capture, transform and load data continuosly into aws data stores
- kinesis data analytics
    - analyze, query and transform streamed data in **real time** using standard sql
    - source is kinesis streams or firehose to redshift , s3, opensearch
 
- the data capacity of your steam is the sum total capacity of its shards
- per shard - 5 reads transactions per second upto max of 2MB per second
- per shard - 1000 write records per second, up to a max of 1 MB per second
- as your data rate increases, you increase the number of shards
- kinesis client library detects the number of shards and makes sure there is a record processor for each shard , if you have 2 consumer instances it will load balance the number of record processors
- ensure number of instances does not exceed the number of shards
- use cpu utilization should drive the quantity of consumer instances you have not the number of shards 



  

  
  
  
  
