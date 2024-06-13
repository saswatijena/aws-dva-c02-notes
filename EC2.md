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
    

  

  
  
  
  
