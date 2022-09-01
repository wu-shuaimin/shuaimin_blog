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











 
 
 
 
 
 
 
 
  
  
  
  
