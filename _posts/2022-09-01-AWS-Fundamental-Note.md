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
 
 S3 is an **object storage** services for the cloud that is highly available. It stands for Simple Storage Service (S3)
 
 
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
 
###  Leveraing Storage Services


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
 
### Content Delievery Service

AWS CloudFront: a content delivery network (CDN) that delivers data and application globally with low latency. Think about it as a cache. Caches are stored in edge location.

* global distribution of content
* Security feature like DDoS protection and geo-restriction

applications:

* S3 Static website
* IP Blocking
* Prevent Attacks
 
Amazon Global Accelerator: sends your users through AWS global network when accessing your content, speeding up delivery.

* automatically re-routes to better network when in traffic
* improves latency and availability of single region app.
 
Amazon S3 Transfer Acceleration: improves content uploads and downloads to and from S3 buckets
 
 
### Understanding Networking Services: VPC and Subcomponents

Networking: it conntects computers and allows sharing of data and application across the global in a secure way using virtual routers, firewalls, and network management services.

Amazon Virtual Private Cloud (VPC): VPC is a foundational service that allows you to create a secure private network in the AWS cloud where you lauch your resources.


VPC is like a fence which protects your applications.

Internet Gateway: it allows public traffic to the internet from a VPC.

Subnet: it allows you to *split* the network inside the VPC. In subnet, you lauch your EC2 instances

**VPC Peering**: it allows you to connect two VPC together

#### Creating an EC2 instance in a VPC

Create a VPC
Navigate to VPC > Your VPCs.
Click Create VPC, and set the following values:
Select: VPC Only
Name tag: my-vpc
IPv4 CIDR block: 10.0.0.0/16
Leave the IPv6 CIDR block and Tenancy fields as their default values.
Click Create VPC.
Create a Public Subnet
Click Subnets in the left-hand menu.
Click Create subnet, and set the following values:
VPC ID: my-vpc
Subnet name: my-public-subnet
Availability Zone: us-east-1a
IPv4 CIDR block: 10.0.0.0/24
Click Create subnet.
Create Routes and Configure Internet Gateway
With my-public-subnet selected, click Actions > Edit subnet settings.
Check the box to Enable auto-assign public IPv4 address.
Click Save.
Click Internet Gateways in the left-hand menu.
Click Create internet gateway.
Set Name tag as "my-internet-gateway".
Click Create internet gateway.
On the next screen, click Actions > Attach to VPC.
In the Available VPCs dropdown, select my-vpc.
Click Attach internet gateway.
Click Route Tables in the left-hand menu.
Click Create route table, and set the following values:
Name: publicRT
VPC: my-vpc
Click Create route table.
On the next screen, click Edit routes.
Click Add route, and set the following values:
Destination: 0.0.0.0/0
Target: Internet Gateway, my-internet-gateway
Click Save changes.
Click the Subnet associations tab.
Click Edit subnet associations.
Select the box for my-public-subnet.
Click Save associations.
Launch EC2 Instance in Subnet
Navigate to EC2 > Instances.
Click Launch instances.
On the AMI page, select the Amazon Linux 2 AMI.
Ensure t2.micro is selected.
Click Review and Launch > Launch.
In the key pair dialog, select Create a new key pair.
Give it a Key pair name of "my-keypair".
Click Download Key Pair.
Click Launch Instances.
Click View Instances, and give it a few minutes to enter the Running state.
Access EC2 Instance
Once the instance has a Running state, select the box next to it.
Click Connect at the top.
In the EC2 Instance Connect section, click Connect.
This will open a new browser tab showing a command line interface.


### Additional Networking services

DNS: domiain name system. It directs interne traffic by connecting domain names with web servers.

Amazon Route 53: a DNS service that routes users to applications

* perform health checks on AWS resources
* supports hybrid cloud architecture


AWS Direct Connect: is a dedicated physical network connection from your on-premises data center to AWS

* Data travels over a private network
* supports a hybrid enviroment
* AWS direct connects is a physical network connection specifically for your on-premises data with AWS data center


AWS VPN: creates a secure connection between your internal networkds and your AWS VPCs

* similar to direct connects but travels through public internet
* data is encrypted


API Gateway: it allows you to build and manage APIs

* share data between system and integrate with services like lambda

### AWS Database services

Amazon Relational Database Service (RDS): is a service that makes it easy to lauch relational databases

* automatic backups, operating system maintenance
* offer high availability


Amazon Aurora: is relational database compitibale with MySQL and PostgreSQL created by AWS

* 5x faster than normal mysql and 3x faster than normal postgresql


Amazon DynamoDB: is a fully managed NoSQL key-value and document database

* key-value database
* serverless (upgrade is done by AWS)
* No relational
* NoSQL

Amazon Document DB: is a fully managed document database that supports MongoDB

* serverless
* non-relational
* Supports MongoDB

Amazon ElastiCache: is a fully managed in-memory datastore compatible with Redis or Memcached.

* In-memory database
* compatible with Redis or Memcached engines


Amazon Neptune: is a fully managed graph database that supports highly connected datasets

* good for social media datasets


### Migration and Transfer Services

Many companies want to migrate their business to the cloud with low cost and fast speed.


Database Migration Service (DMS): helps you migrate database to or within AWS

* migrate on-premieses db to AWs
* virtually no downtime
* support homogeneous and heteroeneous migration


Server Migration Service (SMS): allows you to migrate on-premises servers to AWS.

* Migrate on-premises servers to AWS
* servers saved as a new Amazon Machine Image 
* Use AMI to lauch servers as EC2 instances


Snow Family: it allows you to transfer large amount of on-premises data to AWs using a physical device.

* Snowcone:  smallest data transport devices
  * 8 terabytes
  * offline shipping
  * online with DataSync
* Snowball and snowball edge
  * petabyte-scale data transport solution, which is *cheaper* than internet transfer
  * transfer data in and out
  * snowball edges supports EC2 and Lambda
* Snow mobile:
  * multi-petabytes or exabytes scale
  * data loaded to S3
  * securely transport


DataSync: it allows for online data transfer from on-premises to AWS storage services like S3 or EFS. 

* migrates data from on-premises to AWS
* copy data over direct  connect or the internet
* replicate data cross-region or cross account


### Data Warehouse

A data warehouse is a data storage solution that aggregates massive amount of historical data from disparate sources.

Benefits of data warehouse:

* support quering, reporting, analytics and business intelligence. they are not used for transaction processing


Amazon Redshift: is a scalable data warehouse solution

* Data Warehousing solution. Redshift improves speed and effciency
* handles exabyte-scale data

real world application for redshift

* data consolidation: when you want to consolidate multiple data sources for reporting
* when you want run a database that doesn't require real time transaction processing (insert, update, and delete) 


#### Analytics

analytics:the act of querying or processing your data


Athena: is a query srevice for Amazon S3

* Query service
* Analyze S3 data using SQL
* pay per query
* serverless

Amazon Glue: prepares your data for analytics

* prepare and load data
* Extract, transform, load (ETL) service
* helps to better understand your data

Kinesis: allows you to analyze data and video streams in real time.

* analyze real-time streaming data
* supports video, audio application logs, website clickstreams and IoT

Elastic MapReduce (EMP): it helps you process large amount of data

* process big data
* analyze data using hadoop
* works with big data frameworks
  
Data Pipeline: it helps you move data between compute and storage services running either on AWS or on-premises

* moves data at specific intervals
* sends notification on success or failure
* Moves data based on conditions


QuickSight: helps you visualize your data
  

### Machine Learning Services

Rekognition: allows you to automate your image and video analysis.


Comprehend: is a NLP service that finds relationships in text.


Polly: turns text into speech

SageMaker: helps you build, train, and deploy machine learning models quickly

* prepare data for models
* train and deploy models
* provides deep learning AMIs

Translate: provides language translation

Lex: it helps you build conversational interfaces like chatbots.

* recoginizes speech and understands language
* build highly engaging chatbots


### Developer Tool

Cloud 9: web based IDE

CodeCommit: private git repository

CodeBuild: build and test application source code

CodeDeploy: manages the deployment of code to compute services in the cloud or on-premises

* deploy code to EC2, Fargate, Lambda, and on-premises

CodePipeline: automatse the software release process

* quickly deliver new features and updates
* integrate with codebuild and codecommit, and codedeploy
* dev->test->prod

X-Ray: helps you debug production application

CodeStar: helps developers work together projects

* It integrate whole production pipeline, including codecommit, codebuild, codedeploy


Exam: 

* codecommit: services similar to github
* cloud 9: web based IDE
* codedeploy: deploy an application to servers running on premises and in the cloud
* codepipeline: allows you to implement a CI/CD pipeline


### Deployment and Infrastructure Management Services

Infrastructure as Code (IaC): allows you to write a script to provision AWS resources. The good things about it is that you can provision resources in a *reproducible* manner which saves time.


CloudFormation: allows you to provision AWS resources using Infrastructure as Code (IaC). 

* create template for provisioning resources


Elastic BeanStalk: allows you to deploy your web app. and web services to AWS

* provision resources
* auto. handle deployment

OpsWorks: allows you to use Puppet to automate the configuration of your services and deploy code.

* manage on-premises servers or EC2 instance in AWS

Exam: 

* CloudFormation: it supports infrastructure automation using infrastructure as Code (IaC)
* Elastic Beanstalk: it is *only* used to deploy  applications to the AWS Cloud. **Not for application deployed on-premises**
* OpsWorks: can deploy both on premises and AWS cloud. iIt automates infrastructure management usign Chef or Puppet


### Create a DynamoDB using CloudFormation

Download template:
1. Navigate to the provided CloudFormation template for this lab: acg-dynamodb-template.yaml.
2. Click Raw.
3. Right-click and select Save As.
4. Make sure the Format is YAML.
5. save

Lauch Cloudformation Stack

1. In the AWS Management Console, navigate to CloudFormation.
2. Click Create stack > With new resources (standard).
3. Select Template is ready.
4. Select Upload a template file.
5. Click Choose file.
6. Select the YAML file you downloaded.
7. Click Open.
8. Click Next.
9. For Stack name, enter MyStack.
10. Click Next.
11. On the Configure stack options page, click Next.
12. On the Review MyStack page, click Create stack.

Verify DynamoDB Table:

1. Once the stack is created, navigate to DynamoDB > Tables.
2. Click the listed Inventory table.
3. Note that there aren't currently any items, but the table is ready for items to be added.


### Messaging and Integration Services: SQS

Loose coupling: components are connected but are not dependent

Simple Queue Service (SQS): is a message queuing service that allows you to build loosely coupled distributed systems.

* Allows components to components communication using messages
* multiple components can add messages to the queue.
* messages are processed in an asynchonous manner.

Exam: 

* SQS is processed in FIFO order
* supports loose coupling

Simple Notification Service (SNS): allows you to send emails and text messages from your applications.


Simple Email Service (SES): is an email service that allows you to send richly formatted HTML emails from your applications.

Exam: 

* SNS: plain text emails
* SES: rich HTML-formated emails


### Auditing, Monitoring, and Logging Services

CloudWatch: is a collection of services that help you monitor and observe your cloud resources.

* cloudwatch alarm
* cloudwatch logs
* cloudwatch metrics
* cloudwatch events

CloudTrail: tracks user activity and API calls within your account. 

Exam:

* cloudwatch: monitor your EC2 instances
* cloudtrail: track username, event time and name, IP address, access key, region, and error code


### Aditional Services


Amazon workspaces: allows you to host virtual desktops in the cloud

Amazon Connect: is a cloud contact center service

Exam: 

* workspace: allows you to virtualize desktops
* connect allows you to build a help desk in the cloud


Identity and Access Management (IAM): allows you to control access to your AWS services and resources

* secure you cloud resources
* you define what they can do and who can access
* a free global service


Identities: who can access your resources

Access: what resources they can access

Principle of Minimum Privilage: involves giving a user the minimum access required to get the work done.


Groups: a collection of IAM users which helps you apply access control to all group members

1. Administrators
2. developers
3. analysts


### IAM users permission

Roles: define access permission and are temporarily assumed by an IAM user or service


Policies: you managem permission for IAM users, groups, and roles by creating a policy document in JSON format and attaching it.


Best practice of IAM

1. enable MFA privileged users
2. use strong password
3. use individual users instead of the root user
4. use roles for Amazon EC2 instances

Exam:

* difference between users, groups, roles, and policies
* IAM best practices
* real world user cases
* IAM credential report: list of all users in your account and the status of each account


Create Users and manage permission using groups and policies in IAM

Create a customer managed policy
1. Navigate to IAM > Users.
2. Note the list of lab-provided users.
3. Click Policies in the left-hand menu.
4. Click Create policy.
5. Click Import managed policy on the right side of the page.
6. Search for and select AWSLambda_FullAccess.
7. Click Import.
8. Next to EC2, click Remove.
9. Click Next: Tags.
10. Click Next: Review.
11. Name the policy my_dev_access.
12. Click Create policy.

Create a group controlled via a customer manage policy

1. Click User groups in the left-hand menu.
2. Click Create group, and set the following values:
3. User group name: Enter developers.
4. Attach permissions policies: Select my_dev_access.
5. Click Create group.

Assign Users to the Group

1. Click the developers group.
2. Under the Users tab, click Add users.
3. Select the four developer users.
4. Click Add users.


### Application security services

Web application firewall (WAF): protect your web app. against common web attacks

* protect against SQL injection
* protect against cross site scripting
* deploy WAF for cloud front to block attacks

Distributed Denial of Service (DDos): causes a traffic jam on a web to cause it crash



Shield: DDos protection service

Shield standard: free protection agains common and freqentyly occuring attacks

Shield Advanced: an enhanced protection for a fee

Sheidl Advanced is supported on following services:

* Cloud Front
* Route 53
* Elastic Load Balancing
* AWS global accelerator

Macie: use machine learning to discover and protect sensitive data

Exam: 

* WAF: protect against SQL injection and cross site scripting
* Shield: protect from DDos attacks, and works with CloudFront, Route 53, Elastic load balancing, and AWS global accelerator
* Macie: helps you find sensitive information

### Additional Security Services:

Config: allows you to assess, audit, and evaluate the configurations of your resources


GuardDuty: is an intelligent threat detection system that uncovers unauthorized behavior.

* use ML, and built-in detection for EC2,S3, and IAM

Inspector: works with EC2 instances to uncover and report vulnerabilities

Artifact: security and compilance reports

Cognito: helps you control authentication and authorization of your mobile and web app.

* built-in sign in and sign up service in your app.


### Data Encryption and secrets management services

Data in flight: data is moving from one to another

Data at rest: data is inactive stored

Key management service(KMS): generage and store encryption keys

Secrets Manager: store you secrets (passworkd or keys)

* integrate with documentDB

Cloud HSM: is a hardware security module (HSM) used to generate encryption keys

## Billing and Pricing


### AWS Pricing

Pricing:

1. compute: hourly from lauch to termination
2. storage: data you stored in the cloud
3. outbound data transfer: data moving between system.

Free Offer Types

1. 12 month free: following initial sign up data
2. always free: offers do not expire to all AWS customers
3. trials: short term free trails starting from the data you activate a service

EC2 Pricing

1. on demand
2. saving plan
3. reserved instances
4. spot instances: only lauch when there is available resources
5. dedicated hosts

Lambda Pricing:

1. number of requests
2. code execution time
3. 1 million requests per month


S3 pricing: pay as you go. you pay based on

1. storage class
2. number of objects stored in the cloud
3. data transfer: the amount of transfered out of S3 region
4. requests and data retrieval

RDS Pricing:

1. running clock hours
2. types of database
3. storage
4. purchase type
5. database count
6. API requests:
7. deployment type: single AZ or multiple Az
8. data transfer


TCO: total cost of ownership: financial estimate that helps you understand both the direct and indirect costs of AWS

Application discovery service: helps you plan migration projects to AWS

Reduce TCO using AWS

* minimize capital expense
* utilize reserved instances
* right size your resources

AWS price list api: query the price of AWS services

* json or html
* price alarm

Pricing: 3 fundamental cost

1. compute
2. storage
3. outbound data transfer

Exam:

1. pricing calculator for TCO
2. remember ways to reduce TCO
3. pricing
4. pricing list api

### Billing service

Know ongoing cost


Budgets: allows you to set custom budgets that alert you when your costs or usage exceed the amount.

Budget Types:

* cost budgets
* usage budgets
* reservation budgets

Cost and Usage report: most comprehensive set of cost and usage data

Cost Explore: allows you to visualize and forecast your cost over time

Exam:

1. Budgets: alert you when your usage exceeds a certain amount
2. cost and usage report: provide detailed report to your AWS usage
3. cost explore: visualize and forecast your AWS spending
4. cost allocation tags: helps you track spend


## Governace and management services:

Organizations: allows you centrally manage multiple AWS accounts in one go.

* group multiple accounts
* single payment for all accounts
* automate account creation
* allocate resoruces and apply policies across accounts

benefits of using Organizations:

1. consolidated billing: recieve one bill for multiple accounts
2. cost savings
3. account governace: quick way to create accounts or invite existing accounts

Control Tower: helps you ensrue your accounts conform to company-wide policies

* works directly with AWS organizations
* enfor


System Manager: gives you visibility and control over your AWS resources

* automate operational task on resources
* group actions
* use CLI on many EC2 instances to manage them


Trusted Advisor: provides real-time guidance to help you provision your resources.

free:

* checks for unrestricted access for specififc ports
* checks S3 bucket permission to determine if public access
* MFA
* RDS

no free checks:
* checks for IAM passrowd policy
* service usage greater than 80% over the limit
* exposed access keys
* cloud front content delivery optimization

License Manager: helps you manage software licenses.

Certificate manager: manage and provision SSL/TLS certificates


Exam:

* organization: saves money by consolidated billing
* SCP service control policies: an organization policy that you want everyone in your organization follow
* control tower: enforces best use of services across your organization
* system manager: maintain resources by automatic patching and updates
* trusted advisor: remember the recommondation made by Trusted Advisor
* License Manager: manage licenses
* certificate manager: provides free public certificates


### Utilizing Management Services

Managed services: helps you efficently operate your AWS infrastructure

Professional Services: helps enterprise customers moves to a cloud-based operating model


APN: is a global community of approved partners that offers software solutions and consulting servies for AWS


Marketplace: digital catelog of prebuilt solutions. you can buy the solutiosn

Personal Health Dashboard: alerts you to events that mgiht impact your AWS environment


Exam:

* managed services: augments your staff and helps improve your overall operational efficiency.
* Marketplace and APN: global partner community
* professional services: help you migrate from on-premises to the cloud
* personal health dashboard: provides tailored advice


### Support Plans

* Basic: included for free to all AWS account
* Developer: starts at 29 dollor a month and is recommonded for testign and development
* Business: starts 100 dollor a month and is recommonded for production workloads
* Enterprise supports: starts at 15000 dollor a month. recommonded for business mission critical production workloads

Exam:

* basic support: know how to open tickets and the types of tickets you're allowed to open
* developer support: know how to open tickets and the types of tickets you're allowed to open
* Businesss support: know how to open tickets and the types of tickets you're allowed to open. this supports include the full set of Trusted Advisor checks
* Enterprise Support: 
  * know how to open tickets and the types of tickets you're allowed to open.
  *  this supports include the full set of Trusted Advisor checks
  *  provides access to a technical Account Manager (TAM) and the concierge support team












