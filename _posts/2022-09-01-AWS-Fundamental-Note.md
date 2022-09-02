---

---

## Foundation of Cloud Computing

**Cloud Computing**: The delievery of *computing services* over the internet

**Virtual Machine**: Virtualization lets you divide hardware resources on a single physical server into smaller units. This maximizes the use of computing resources in AWS servers.

* On demand usage: no long term contract
* pay as you go: pay the portion of the service you used.

6 advantages to cloud computing

* Go global in minutes: AWS has servers in regions across the globe
* Stop spending money raunning and maintaining data centers
* Benefit from massive economies of scale
* Increase speed and agility
* Stop guessing capacity
* Trade capital expense for variable expense: you pay what you use instead of investing upfronts

Technical Terms for Advantages of AWS

* Availability: The service is alway usable, which means whenever you want to use, the services are available.
* Elasticity: You have the chance to vary your use of service without constraints
* Agility: You can quickly deploy and lauch your application
* Durability: Data will be stored in long time without lost of data


CapEx vs. OpEx

1. Capital Expenditures (CapEx): Upfronts purchases to fixed assests
2. Operating Expenses (OpEx): Operating expenses are cost to run daily operations

### Cloud Computing Models

1. Infrastructure as a Service (IaaS)
  * Fundamental building blocks that can be rented
  * Example: web hosting
2. Platform as a Service (PaaS)
  * For developers to develop softwares using web-based tools without worrying about the underlying platform
  * Example: provide a linux system with java langauge as well as other developing tools such IDEA or Jenkins
3. Software as a Service (SaaS)
  * Complete application: Provide users with finished softwares, and they use softwares on demand
  * Example: Email providers
  
 
 Cloud Deployment Model:
 
 1. Private Cloud: 
  * More security and infrastructures are not shared with other companies
  * Do not have advantages mentioned above
 2. PUblic Cloud:
  * Offered by cloud computing companies
  * Don't need to concern physical hardware
  * Provid all the advantages of cloud computing
 3. Hybrid Cloud
  * Combination of private cloud public cloud
  * AWS Cloud provides the web application and tools to read the data, and confidential data is stored locally in private cloud
 
### AWS global infrastructure
 
Choose the location that is closest to your users allowing fast downloading and uploading.
 
Regions: Physical location in the world that contains Availability zones. 

* independent to each other
* deploy resources in the region closest to your users
* most resources are tied to a specific region
 
Availability Zone: 
 
It consists of one or more physically separated data centers as well as servers that hosting your application. One avialability zone has more than one data centers. 

An AZ is associated with a single Region
 
Characteristics of AZ

* Physically separater
* Connected through low-latency links
* Fault tolerant
* Allows for high availability

If your application were deployed through multiple availability zone and one zone fails, the application should still run, hosting in other AZ

**Edge Locations**: are cache content for fast delievery to users. There are more edge locations than AZ. The user of edge location is to decrease latency. Think this idea in OS, in which memory cache speed up information moving.

**Latency**: the time that takes between an user request and the corresponding response.

Low latency is good, and high latency is bad.


### AWS Management Console

**AWS Management Console**: it allows you to access your AWS account and manage applications in your acount from a *web browser*


Root user: the most powerful user that can create and delete the AWS acount

AWS command line interface (CLI): it allows users to access AWS resource through API. This allows you to gain programmatic access to AWS resoruces

## AWS Technology

Services: Keep in mind what the services do, and who use this services

### Elastic Compute Cloud (EC2)

ECS allows you to rent and manage virtual servers in the cloud. You can grow and shrink the services you need.

Methods to access an EC2 instance

1. AWS management console: configuare EC2 instance through web browser
2. Secure shell (SSH): establish a secure connection to EC2 instance from local laptop **Most common way to access linus EC2 instance**
3. EC2 instance connect (EIC): 
4. AWS system manager: access instance by AWs managemet console or CLI


SSH Access

1. generate a key pair, private key and public key
2. connect via SSH

EC2 Pricing Option

* On-demand: pay for instances you used down to seconds. No contract and front cost
  * You want low cost, and don't want to have contract or up front cost
  * Your applications have unpredictable workloads that should be available all the time
  * Your application is under development
  * Your workloads will not run longer than a year
  * **You can reserve capacity**
* Spot: **only** when there is unused EC2 capacity your requests will be fullfilled.
  * you application can be interrupted, and you don't care the start and stop time of your app.
  * save up to 90% off on-demand
 * Reserved Instance (RIs): you commit to use AWS for 1 or 3 year in a region
  * you choose this when ou have steady usage, and you can commit 1 or 3 years
  * your app. requires a capacity reservation
  * you can choose convertible type to gain flexibility
* Dedicated Hosts: you pay for the physical servers which can only be used by your instances.
  * choose this when you have regulatory or corporate compilance requirements around tenancy model
* Savings plan: allows you to commit to compute usage (in hours) for 1 or 3 years
  * you choose this when you want to lower your bill across multiple compute services.
  * you want the flexibility to change compute services, instance types, operating systems, or Regions
 
 ### Features of EC2
 
 * Elastic Load balancing: automatically distribute your load to multiple instances to avoid traffic
  * classic load balancers
  * application load balancers
  * gateway load balancers
  * network load balancers
 * EC2 Auto Scaling (horizontal scaling): add or remove EC2 instances automatically across AZs, according to the demand
  * vertical scaling: upgrades an EC2 instance by adding more power (CPU, RAM)
 
 ### Compute Services: Lambda
 
Lambda: is a serverless compute service that lets you run code wihtout managing servers

* Scales automatically
* Serverless indicates that you don't need to manage servers like EC2
 
 Developers focus on business logic and software without considering low level machine set up
 
 Lambda Features:
 
 1. supports different programming languages
 2. you write code using your prefered enviroment
 3. lambda execute your code
 4. lambda has 15-minutes time-out, so if your app. has processes that run longer than 15 min, it is not good.
 
 
 Pricing Model
 
 1. Compute Time: pay only for compute times used, no charging if your code didn't run
 2. Request Count: Each request is counted when it start excuetion 
 3. Awalys free
 
  
### Compute Services: Additional Compute Services:
  
AWS Fargate: is a serverless compute engine for containers. It allows to manager containers like Docker


AWS Lightsail: it allows you to quickly lauch all the resources you need for small projects
 
* Deploy preconfigured application, like wordpress
* low and predictable monthly fee.
* Includes a virtual machine, SSD based storage, data transfer, DNS management, and a static IP
  
 
 AWS Outposts: allows you to run cloud services in your internal data center. Used for hybrud premises.
 
AWS Batch: allows you to process large workloads in smaller chunks (or btaches). runs hundreds and thousands of smaller batch processing jobs.
 
 
 #### Storage Services S3
 
 S3 is an **object storage** services for the cloud that is highly available.
 
 
 * Objects (or files) are stored in *buckets* (or directories)
 * Objects can be public or private.
 * Upload objects via console, the CLI or programmatically from within code using SDK
 
 S3 Options
 
 * S3 Standard: 
  * General-purpose storage, data is stored across multiple Availability Zone
  * Low latency and high throughtput
  * **good for frequently accessed data**
 * S3 Intelligent-Tiering:
  * automatically moves your data to the most effective storage class
  * automatic cost savings, and data is stored across multiple AZ
  * **Good for data with unkown changing access pattern**
 * S3 standard-infrequent access
  * data accessed less frequently but required rapid access
  * data is stored across multiple AZ. This option is cheaper than S3 standard.
  * Good for long-lived data and infrequently accessd, but fast access when needed
 * S3 One Zone-infrequent Access (IA)
  * Like s3 standard but stored in a single AZ
  * cost 20% less than standard
  * data stored in this storage can be **lost**
  * Good for *re-creatable* data, infrequent access. Durability and availability are not essential
 * S3 Glacier
  * Long-term data storage, and data retrieval takes long time (1-5 min, 3-5h, and 5-12h). Data is stored in multiple AZ
  * Good for long term backups, cheaper storage
 * S3 Glacier Deep Archive
  * Similar to S3 Glacier but longer access time
  * 2 retrieval options: 12h or 48h
  * cheapest of all S3 options
 * S3 Outposts:
  * provide objects storage on-premeises
  * a single storage class
  * store data across multiple devices and servers.
  * Good for data needs to be stored locally, demanding application performance needs
 
 S3 in real world application
 
 * static website
 * data archive
 * analytics system
 * mobile application
 
 ### Leveraing Storage Services


Amazon Elastic Block Store (EBS): EBS is a storage device (called volume) that can be attached to your instance. think about it as a hard drive
 
* Data persists when instance is not running
* In the same AZ, EBS can only be attached to *one* instance
* Tied to *one* AZ
* Good for quickly accessible data, running a database on an instance, and long-term data storage
 
EC2 Instance Store: An instance store is local storage that is physically attached to the host computer and cannot be removed.
 
* Storage on disks *physically* attached to an instance
* storage is *temporary* since data loss occurs when EC2 instance is stopped
* Faster with high I/O speeds
* good for: temporary storage needs, data replicated across instances
 
 
Amazon Elastic File System (EFS): EFS is a serverless network file system for sharing files.
 
* only support Linux file system
* accessible across different AZ in the same Region
* more expensive than EBS
* good for: main directories for business-critical apps, lift and shift enterprise apps
 
 
Storage Gateway: is a hybrid storage service

* connects on-premises and cloud data
* supports a hybrid model
* good for: moving backups to the cloud, reducing costs for hybrid cloud storage, low latency access to data
 
 AWS Backup: helps you manage data backups across multiple AWS services
 
 EC2 Storage contains:
 
 1. Elastric block store (EBS)
 2. Instance Store 
 3. Elastric File System (EFS)
 
Storage Gateway: is a hybrid storage service that connects on-premises and cloud data
 
AWS Backup: helps you to manage backup across multiple instances
 
 
 
 
 
 
  
  
