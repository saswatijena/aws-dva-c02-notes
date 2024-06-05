# Elastic Compute Cloud (EC2)
- Secure, resizable compute capacity in the cloud
- its ike a VM hosted in AWS instead of your own data center 🔥
- Designed to make web-scale cloud computing easiter for developers 
- The capacity you want when you need it 🔥
  - no wasted capacity, allows to grow and shrink your infrastructure to suit the needs of your capacity
  -  pay only for what you use 🔥
- You re in complete control of your own instances
- instance type determines the hardware of the host computer 🔥

## Pricing models
  - On Demand
      - pay by the hour or the second depending on the type of instance you run
  - Reserved:
      - reserved capacity for one or three years. Up to 72% discount on the hourly charge 
      - great for applications with steady state
      - standard RI
      - convertible RI : has option to change to different reserved instance type
      - scheduled RI: launch within the time window you define
  - Spot
      - Purchase unused capacity at a discount up to 90%
      - prices fluctuate with supply and demand
      - great for application that have flexible start and end times
      - great for urgent ened for large amounts of additional computing capacity
  - Dedicated
      - a physical EC2 server dedicated for your use. Most expensive
      - great for compliance and licensing
      - can be purchaed on demand or reserved
  - Savings plan
      - commit to one or three years to get discounted prices
   

# Elastic Block Store (EBS Volumes)
  - storage volumes you can attach to ED2 instances
  - when you first launch EC2 it has at least 1 EBS volume which runs the operating system
  - designed for mission critical workloads
  - highly available
  - highly scalable

## Types of EBS 🔥 [EBS Volume Types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html)
  - General Purpose SSD (GP2)
    - suitable for boot disks and general applications
    - 3 iops per GiB
    - upto 16K IOPS per volume
    - 99.9% durability
  - General Purpose SSD (GP3)
      - suitable for boot disks and general applications
      - 3K iops per GiB
      - upto 16K IOPS per volume
      - 99.9% durability
      - latest generation 
  - Provisioned IOPS SSD (io1)
      - High performance and most expensive
      - suitable for online transaction processing and latency-sensitive applcations
      - 50 iops per GiB
      - upto 64K IOPS per volume
      - 99.9% durability
      - I/O intensive applications (large databases and latency sensitive applications)
  - Provisioned IOPS SSD (io2)
      - High performance
      - latest generation, same as io1
      - 500 iops per GiB
      - upto 64K IOPS per volume
      - 99.999% durability (more durable than io1)
      - I/O intensive applications (large databases and latency sensitive applications)
  - Provisioned IOPS SSD io2 Block Expres
      - 4x throughpuut IOPS and capacity of regular io2 volumes
      - for mission critical, high-performance applications
      - 256K IOPS per volume
      - 99.999% durability
      - AWS SAN in the cloud offering
  - Throughput Optimized HDD (st1)
      - low cost HDD volume
      - frequenctly accessed throughput intensive workloads
      - big data, date warehouses , ETL and log processing
      - cannot be a boot volume
      - max throughput isn 500MB/s per volume
      - 99.9% durability
  - Cold HDD (sc1)
      - Lowest cost option
      - data which requires fewer scans per day
      - max throughput isn 250MB/s per volume
      - cannot be a boot volume
      - 99.9% durability
 
## Encyption
  - Default encryption: If encription by default is set on your account by your account admin, you cannot create unencrypted EBS volumes
  - Encrypted snapshots: If you can create an EBS volume from an encrypted snapshot, then you will get an encrypted volume.
  - Unencypted snapshots: encyptions is option only if default encryption is not set


# Elastic Load Balancer 🔥

  A load balancer distributes network traffic across a group of servers
  we can increase capacity as needed

  - Application load balancer (HTTP and HTTPS) - Operate at layer 7 and are application aware
  - Network load Balancer (TCP and high performance and expensive) - Operates at Layer 4
  - Classic load balancer (HTTP/HTTPS and TCP ) [Legacy option]
  - Gateway load balancer 🆕 - provides load balancing for third-party virtual applications, like firewalls, intrusion detection and prevention systems (cisco, palo alto etc)


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
if you need the IPV4 address of your end user, look for the X-Forwaded-For header, since the server will recieve the header of the loadbalancer

  
  
