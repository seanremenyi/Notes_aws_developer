# AWS Developer Associate Notes
 
 
 ## IAM
 
 Identity and Access Management
 Manage users and their level of access to the console
 
 Centralized control over account
 Shared access to AWS account
 Granular Permissions
 Idenetity Federation (use other creds like fb, linkedin, etc)
 
 1. Multi-factor Authentication
 2. Temporary Access for uses.devices/services
 3. Password Policies
 4. Integrate with AWS Service
 5. Compliance

Users : End Users, people
Groups: Collection of users under one set of permissions
Roles: Define set of permissions and can be assigned/assumed by users or services like ec2 
Policy: Document that defines one or more permissions (can be attached to any of the above)

Testing IAM permissions
- IAM Policy Simulator
- test the effect before commiting
- validate its working as expected
- test policies already attached to users (great for troubleshooting)

Universal not regional

Root account is account you created when you created the AWS account (don't use)

New users have 0 permissions when first created.

New users are assigned access keys.secret acces keys (not the same as a password to login) used for progammatic access
Can only see once when the user is created (SAVE)
recommended MFA
Customize password rotation policies

## EC2

Elastic Compute Cloud
Secure Resizable compute capacity in the cloud (VM hosted in AWS)

pay only for what you use
no wasted capacity

have servers in minutes (not in months)
Grow and shrink as you need
Select capacity you need now and not have to guess for the future

Pricing options:
On Demand
 - pay by hour or second depending on the instance
 - flexible
 - low cost
 - no upfront payment
 - no long term commitment
 - Good for short term/spikey/unpredicatable workloads but that you don't want to be interrupted
 - Good for applications in deveopment/testing
Reserved instance
 - reserve for 1 or 3 years. 72% discount (Regional, if you reserve 10 instances in one region but want to spin up one in another region, it won't count as part of your rservation and you will have to pay extra)
 - Great for predicatable usage
 - Good for specific capacity requirements
 - good if you can pay up front (cheaper! Woohoo!)
 - Standard: up to 72% off on-demand (supposedly), Cons: What if demand changes and you need to upgrade
 - Convertible: up to 54% off on -demand can be changed to a different reserved instance of equal or greater value
 - Scheduled: launch within a time window you define. if you want only in a time frame, predicatable time you know
Spot
 - purchase unused capacity. Discount up to  90%. 
 - price fluctuates
 - You set a maximum price you are willing to pay. If cost exceeds that price, instance can be terminated or hibernated
 - Not suitable for every applications
 - Good for flexible start/end times
 - Applications only feasible if you don't have large computing costs
 - good for urgent jobs
 - image rendering/calculations
Dedicated
 - Physical server dedicated for YOUR use.
 - Most expensive
 - Great for compliance/regulatory requiremnts
 - Licensing (which desn't support cloud deployments)
 - can purchase as on-deman or reserved

Savings Plans
- save up to 72% regardless of type/region
- Commit to 1 or 3 years use of specific compute power (calculated in $ per hour)
- also covers Lbda, fargate, serverless technologies as well as ec2

Pricing Calculator: great for estimating costs
Turn off if you no longer need a service


Instance types
1. Hardware type determines the hardware of the host for the instance
2. Capabilities (Cpu, memory, storage grouped in "families" awww cute)
3. Requirements, select type based on the requirements of your application that you plan on running on it

Family
General Purpose:
- type t
AWS says
- provides balance of compute/memory and network resources
- recommended: small and medium databases, dataprocessing tasks that require additional memory, caching fleets and or for running backend servers for SAP, Microsoft Sharepoint and other enterpricse applications 

Instance name: t2, t is type and number is generation

Micro Instance:
-type t3.micro
AWS says:
- eligible for free tier for the first 12 months of your account, 750 hours of micro insances each month 
- great for starting out
- if you exceed then pay as you go
- low cost/low throughput or small applications

Compute optimized:
- type C
AWS says:
- higher ratio of vCPU to memory than other families
- Recommend for running CPU-bound scale out applications. applications include high traffic front end fleets, on-demand batch processing, distributed analytics, web servers, batch processing and high performance science and engineering applications

FPGA (field programmable gate arrays):
- type f
AWS says:
- can be programmed to create application specific hardware acceleration, along with high CPU performance, large memore and high network bandwitdh for applications requiring massively paralel processing power such as denomics, data analytics, video processing and financial computing
- example accelerate analytics on trading

GPU (graphics processing units)
- type g and p
AWS says:
- high CPU and network performance for applications benefitting from highly parallelized processing, including 3D graphics, HPC, rendering, and media processing applications

Machine learning ASIC (application specific integrated circuit):
- type inf (inferenture)
Aws says:
- powered by chips custom built by AWS
- optimized for machine learning applications such as image recog, speech recog, natural language processing and personalization

Memory optimizes:
- type r, x and z
AWS says:
- Memory optimized
- lowest cost per GB of RAM among ec2 types
- Recommended for database application, memcached and other distributed cases and larger deployments of enterprise applications like SAP and Microsoft Sharepoint

Storage Optimized:
- type d, i and h
Aws says:
- Provides direct-attahed storage options optimized for applications ith specific I/O and storage capacity requirements.
- Recommend I instances for NoSQL database which benefit from very high random I/O performance, low request latency of direct-attached NVMe SSDs
- Recommend D instances for running large-scale data-warehouse or parallel file systems


AMI - Amazon machine image
includes OS, utilities/libraries 

## EBS

Elastic Block Store

Storage volumes (disks) you can attach to your ec2 instance
When you first launch ec2 theres a volume attached where the os lives

can use to store data, run os, database, file systems, etc.

1. Designed for mission critical workloads
2. Highly Available (automaticall replicated within single AZ)
3. Scalable (dynamically increase capacity or change type with no downtime or performance to live systems

Types:
General Purpose SSD
GP2

- general purpose, a balance of price and performance
- 3 IOPS per GB up to a max of 16,000 IOPS per volume
- gp volumes smaller than 1tb can burst up to 3000 IOPS
- Good for boot volumes or development and testing applications which are not latency sensitive

Provisioned IOPS SSD
io1

- high performance
- most expensive
- up to 64 IOPS per volume, 50 iops/gb
- use if you need more than 16000 IOPS
- for I/O intensive applications like large databases and latency sensitive workloads

io2

- latest generation
- higher durability and more IOPS
- same price as io1
- 500 IOPS/gb
- up to 64,000
- 99.999%durability whereas rest of ebs volumes are designed for 99.9%
- same use cases for io1 but for also those with a higher durability

Throughput Optimized HDD
st1

- hard disk disk volume
- low-cost
- for huge amounts of data (big data, data warehouse, etl, log processing)
- Baseline throughput of 40MB/TB
- ability to burst to 250 MB/TB
- Maximum throughput of 500 MB/s per volume
- frequently-accessed, throughput-intensive workloads
- cost effecctive way to store mountains of data
- cannot be a boot volume

Cold HDD
sc1

- lowest cost option
- Baseline throughput of 12 MB/tb
- burst up to 80 MB/tb
- Max 250 MB/s per volume
- good choice for colde data requiring fewer scans per day
- good for applications that need th lowest cost and performance is not a factor
- cannot be a boot volume

IOPs
- Measure the number of read/write operations per second
- Metric for quick transations, low latence apps, transactional workloads
- Ability to action reads an writes very quickly
- Choose provisioned

Throughput:
- Measure the nuber of bits read or writen per second
- Metric for large datasets, large I/O sizes, complex queries
- ability to deal with large datasets
- Choose throughput optimized

if the volume is from a snapshot
if you create a volume from an encrypted snapshot then the volume is automatically encrypted
same for unencrypted


## ELB
Elastic Load Balancer
a load balancer istributes network traffic across a group of servers

Application LB
- http https
- Operate at layer 7
- application-aware
- can do advanced request routing
- can look at http header

Network LB
- tcp
- high performance
- where extreme performance is required
- operate at layer 4, transport layer
- millions of request per second while ultra low latencies
- most expensice options

Classic
- can do both
- legacy
- support x-forwarded-for headers and stick sessions for layer 7 features
- supports layer 4 tcp as well

Gateway LB
- using Geneve


X-Forwarded-For header: identify the originating IP address of the client connecting through a load balancer
finds ipv4 address of end user
app only will see ip of load balancer but maybe want to kknow where the ip is coming from (whitelisting/blacklisting/geolocation)
will pass the originating ip address to the application

errors:
504 Gateway timeout: 
- the target (downstream server, lambda, database) could not establish a connection
- application is having issues
- Will then need to find where the application is failing to resolve

## Route 53
Amazon's DNS service

can map a domain name that you own to ec2s, s3 buckets, load balancer
Hosted Zone - A container for DNS records for your domain.
Alias - Allows you to route traffice addressed to the zone apex, or the top of the DNS namespace e.g. googl.com and send it to the resource within AWS, e.g. an elastic load balancer


## AWS CLI
Least privelidge principle - least amount of access to do their job
Use groups is best practice -Use policies to groups and iam users to groups

secret access key can be shown once, will have to delete regenerate and use aws configure to use new one
supports linux, maco, windows
each dev should have their own, don't share

pre installed on ec2s

### Pagination
contro the nuber of items included in the output when you run cli command
by default uses a "page size" of 1000
for a 3000 item thing, cli needs to make 3 api calls but will show everything
default pagesize of 100 might be too high might lead to timed out errors
fix default page size using flag --page-size to request smalled number of items
cli still retrieves the full list but performs a large number of API calls in the background and retrieves a smalled number of items with each call
--page-size 100
--max-items 100 (returns fewer items)

## Roles
IAM -> roles -> create role -> trusted idendity -> EC2 -> add permissions policy 
->launch instance, add IAM role
Roles are preferred options from security perspective
always avoid hard coding credentials
policies control roles permissions
can update a policy attached to a role
can attach and detach roles without terminating ec2

## RDS
relational databse service

tables - traditional spreadsheet
rows data items
columns fields in the data space

when to use: Generealy used for OLTP (online transaction Processing) workloads

compatible with 
SQL Server (microssoft)
Oracle
MySQL
PostgreSQL
MariaDB
Aurora (amazon)

up and running in minutes (used to take for all this take 8 days manually or longer)
multi az
failover compatability
automated backups

OLTP: processes data from transactions in real-time, eg customer orders, banking transactions, payments and booking systems
data processing and completing large amount of transaction in rel time

OLAP (Online Analytics Processing): 
process complex queries to anazyze historical data e.g. net profit figures from the past 3 years and sales forecasting
all about data analysis using l arge complex queries that take long t ime to complete
ex. net profit analysis
large amounts of data
analysis not transactional

RDS is for OLTP workloads
not suitable for OLAP (use redshift)

Multi-AZ not available in free tier

### Multi AZ
it is an exact copy of your production database in another availability zone
you have a primary rds (ex us-east-1a) production (connected to application (ec2s))
and a standby rds in a different az (ex us-east-1b) replicated from primary invisbile and inaccesible from accplication servers. if something goes wrong with primary (hardware issue/az issue) we have this standby and rds with automatically failover to this standby instance
AWS handles all the replication between, no config necessary
Write to primary will automatically sync to standby rds
All rds (SQL server, oracle, MySQL, PostgreSQL, MariaDB) can enable multi az
Main pupose :provide resilience and keep application running if failure/or maintenance. RDS will automaticall failover to standby so operations can resume quickly
Disaster recovery, not for scaling out/improving performance (application servevrs can't connect to primary and standby rds at the same time)
An exact copy of your production db in another AZ
used for DR
in event of failure RDS will automatically failover to standby



### Read Replicas
A read only copy of the primary database
Great for read heavy workloads, take read workloads off of primary
can be in the same AZ as primary, cross az (different AZ) or cross region(different region)
each read replica has its own DNS endpoint, different to that of the primary
Read Replicas can be promoted to their own database however this will break the replication from the primary database will give us 2 completely independent databases with read/write capabilities.
Scaling read performance (used for scaling, NOT for DR)
Requires automatic BAckup enabled (enabled by default)
Multiple read replicas are supported (up to 5 read replicas to each databse instance
Read only copy of your primary in the same AZ, cross AZ or cross region
used to inscrease or scale read performance
great for heavy workloads and takes the oad off your primary db for read-only workloads,  e.g. business intelligence reporting jobs.


### RDS Backups
1. Database snapshots
manual, ad-hoc, user-initated. Provides a snapshot of the storage volume attached to the DB instance.
not automated, user-initiated
no retention period (not deleted even after you delete the original RDS instance, including any automated backups) 
backup to a known state. Can restore to that specific state at any time

2. Automated Backup
Enabled by default
Creates daily backups or snapshots that run during a backup window that you define
Transaction logs are used to replay transactions
Allows a Point-in-time Recovery. recover your db to any point in time within your defined "retention period of 1-35 days.
Full daily backup. RDS takes a full daily backup/snapshot into an s3 and also stores transaction logs throughout the day
Recovery. When you do a recovery, AWS will first choose the most recent daily backup and then apply transaction logs relevant to that day up to the recovery point
stored in s3
free storage space equal to the size of your backup 
Defined backup window. during this time storage I/o may be suspended for  few seconds while the bckup process initializes and you may experience increased latency at this time


General
Restored version of the database wil always be a new RDS instance with a new DNS endpoint
Encryption at rest. Can enable the encryption in the console with encryption option at creation time
Encryption is done using aws' KMS service. AES-256 encryption
This encryption includes all the underlying storage, automated backups, snapshots, logs and read replicas
Can only be enabled at creation time. and you canot enable encryption on an uncrypted RDS DB instance (and vice cersa)
To do this. Take a snapshot of an encrypted DB, encrypt the snapshot then restore that db from that snapshot and the db will be encrypted
only enable at creation can not do later, neet to ^^



Automated snapshot
automated by default, you define backup window, daily backup, point-in-time snapshot plus transaction logs
retention period 1-35 days
Can be used to recorer your database to any point in time within that retention period

DB snapshot (manual)
user initiated, ad hoc, point-in-time snapshot only, no retention period, stored indefinitely
Used to backup your DB instance to a known state and restore to that specific state at any time, e.g. before making a change to the database


## Elasticache
Memory is faster than disk
in-memory cache (key-Value)
makes it easy to deploy, operate and scale in-memory cache in the cloud
Improves db performace it allows you to retrieve information from fast, in-memory caches instead of sower disk-based storage.
Great for read-heavy Db workloads
Caching the results of I/O intenensive db queries also for retaining session data for distributed applications
can use elasticache cluster to cache freequently accessed data for faster application, application servers can retrwieve data from there

### Memcached
Great for object caching
Scales horizontally but no persistence, multi az or failover.
good choice for basic caching and you want you caching model to be as simple as possible.
in memory key-value data store
if object cahing is your primary goal
keep it as simple as possible
dont need persistence/multi az
dont need to support advanced data types or sorting

### Redis
More sophisticated solution with enterprise features like persistence, replication, Multi-AZ and failover
Supports sorting and ranking data (e.g. for gamin leaderboards) and complex data types like lists and hashes
in memory key-value data store
performing data sorting and ranking, ,such as leaderboards 
have advanced data types, such as lists and hashes
need data persistence
need multi az



if a db is under a lot of stress, know when to use elasticache, if db is particular read heavy and data is not changed very frequently (elasticache will struggle to have the latest data available)
When elasitcache can't help
wont help alleviate heavy write loads (will need to scale up db)
Olap queries(think about using redsihft instead)


## Systems Manager Parameter store
Scenario:
Needs a place to store parameters for your application for example, licence keys, db connection information, user names and passwords, etc.
This info needs to be passed to your ec2 as a bootstrap script
avoid harcoding sensitive info
maintain the confidentiality of the information  

store confidential info 
Plain text or encryted using kms
can refernence your parameters using the parameter name (e.g. bootstrap script)
integrated with AWS service, you can use Parameter store with EC2, Cloud Formation, Lambda, Codebuild, CodePipeline and Code Deploy

## S3
simple storage service
provides secure, durable and highly-scalable object storage.
Simple
scalable
Object-based storage - manages objects rather than in file systems or datablocks
can upload any files, photos,videos, code, documents,text file
cannot be used to run an operating system
largest object that can be uploaded in a single PUT is 5 gb
1. Unlimited storage
total vloume of data and the number of objects you can store is unlimited
2. objects can range in size from a minimum of 0 bytes to a max of 5 tb
3. S3 buckets
stores files in buckets (similar to folders) 

working with s3 buckets
1. Universal Namespace
all aws accounts share the s3 namespace. Each s3 bucket name is globally nuique
Example:
https://{bucket-name}.s3-Region.amazonaws.com/{key-name}
uploading files
when you upload using the cli you will receive an http code of 200 if its a successful upload

S3 objects are
Key: The name of the object
Value: This is the data itself, which is made up of a sequence of bytes
Version ID: Important for storing multiple versions of the same object
Metadata: Data about the data you are storing, e.g. content-type, last modified, etc.

S3 is a saf place to store your files
data is spread across multiple devices and facilities to ensure availability and durability 
Highly Available and highly Durable.
Built for availability - 99.95% - 99.99% service availability depending on the s3 tier
Built for durability (data being stored safely and not being corrupted) Designed for 99.999999999% (11 9s) durability for data stored in s3

Tiered storage (different tiers for different uses/costs)
Lifecycle Management (transition to different tiers or even delete objects) 
Versioning (all versions of an object are stored and can be retrieved, including deleted objects)

Secure:
Server side encryption: You can set default encryption on a bucket to encrypt all new objects when they are stored in the bucket)
Access Control Lists: Define which aws accounts or groups are granted access and the type of access. You can attach s3 ACLs to individual objects within a bucket.
Butcket Policies: S3 bucket policies specify what actions are allowed or denied (e.g allow user Alice to Put but not delete objects in the bucket)

signed urls/signed cookies used for if wanting to make sure authorized users only have access


## Serverless
Serverless allow you to run application code in cloud without having to worry about managing any servers
AWS handles the infrastructure management tasks so that you can focus on writing code

Aws takes care of
Capacity provisioning
patching
auto scaling
highg availability
you just focus on writing code

Gives competitve Advantage
Speed to market
By eliminating the over head servers, you can release code quickly and get yyour application to market faster
Super Scalable
You can have a million users on your website and everything will scale automatically
Lower Costs
You never pay for over-provisioning. Serverlesss applications are event-driven and you are only charged when your code is executed
Focus on your application. 
Aws offers a range of serverless technologies which integrates seamlessly. Allowing you to focus on building great applications.

Lambda
Enables you to run your code as functions withhout provisioning any servers
SQS (simple queue service)
A message queuing service that allows you to decouple and scale your applications
SNS (simple notification srvice)
A messaging service for sending text messages, mobile notifications and emails
API Gateway
Allows you to create, publish and secure APIs at any scale
DynamoDB
Fully managed NoSQL database
S3
Object storage and web hosting

## Lambda

Serverless Compute
Run your code in AWS without provisioning any servers
Labda takes care of everything required to run your code, including the runtime environment
Supported Languages
Java, Go, PowerShell, Noda.js, C#, Python and Ruby
Upload your code to lambda and you are good to go.
Enterprise Features.
Auto-scaling and high availability are already baked-in to the Lambda service

You are charged based on the number of requests, their duration and the amount of memory used by your lambda function
1. Requests
first 1 million requests per month are free
then $0.20 per month per 1 million requests
2. Duration
You are charged in millisecond increments
The price depends on the amount of memory you allocate to your lambda function
3. Price per GB-second
$0.00001667 per GB-second 
a function that uses 512 MB and runs for 100ms  0.5Gbx 0.1 s = 0.05 GB-s, we would be charged $0.0000000083 
first 400,000GBs per month are free

Event driven architecture
1. Event driven
Lambda functions can be automatically triggered by other AWS services or called directly from any web or mobile app
2. Triggered by events
These events could be changes made to data in an s3 bucket, or Dynamodb table
3. Triggered by User Requests.
You can use API Gateway to configure an HTTP endpoint allowing you to trigger your function at any time using an HTTP request

AWS services that can invoke Lambda functions.
dynamoDB, Kinesis, SQS, Application Load Balancer, Api gateway, Alexa, Cloudfront, S3, SNS, SES, Cloudformation, CloudWatch, CodeComomit, CodePipeline 

Extremely cost effective
Pay only when your code executes
Continuous Scaling
Lambda scales automatically
Event Driven
Lambda functions are triggered by an event or action
Independent
Lambda functions are independent. Each event will trigger a single function
it is Serverless technology
know triggers

## API Gateway
application programming Interface
We use APIs to interact with web applications and applications use APIs to communicate with each other
API gateway
1. Ability to publish, maintain and monitor APIs
API gateway is a service which allows you to publish, maintain, monitor and secure APIs at any scale
2. A Front Door
An API is like a front door for applications to access data, business logic, or functionality from your backend services, e.g. applications running on ec@, Lambda
3. Supported API types
RESTful APIs are optimized for stateless, serverless workloads
Websocket APIs are for real-time, two-way, stateful communication e.g., chat apps
4. Can rollback to previous deployment by selecting a stage that has the last verion of your deployment

Restful Apis
REpresentational State Trnsfer
Optimized for serverless and web applications
Stateless : Nothing is persisted in the application or the API
support JSON (a notation language that uses key-value pairs)

API gateway provides a single endpoint for all client traffic interacting with the bakend of your application

it allows you to connect to applications running on Lambda, EC2 or elastic beanstalk and services like DynaoDB and Kinesis

Supports Multiple Endpoints and targets
Send each API Endpoint to a different target

Supports multiple versions
Allows you to maintain multiple versions of your API so you can have different versions for your dev, testing and prod environments

Serverless :So cost effective and scalable
Cloudwatch: API Gateway logs API calls, latencies and error rates to CloudWatch
Throttling: API Gateway helps to manage traffic with throttling so that backend operations can withstand traffic spike and denial of service attacks

Importing APIs in API Gateway
1. Importing API Definition files. You can use API Gateway Import API feature to import an API using a definition file
2. Supported PRotocols. OpenAPI, formerly known as swagger, is supported
3. Create New and Update Existing APIs. You can use an OpenAPI definition file to create a new API or update an existing API
Legacy Protocol
For example SOAP (Simple Object Access Protocol) which returns  response in XML format instead of JSON, is a legacy protocol
You can congifure API gateway as a SOAP web service passthrough
You can also use API Gateway transform the XML response to JSON

API Gateway Cachine
1. Caches your endpoint response. This reduces the number of calls made to your endpoint and can also improve the latency for requests to your to your API
2. TTL. When you eneable caching, API Gateway caches responses from your endpoint for a specified time-to-live (TTL) period in seconds. The default is 300 seconds
3. API Gateway Returns the cached response. API gateway then responds to new requests by looking up the response from the cache, instead of making a new request to your application.

API Gateway can help you improve the performance of your APIs, and the latency your end users experience, by caching the output of the API call to avoid your backend application every time

API Gateway Account Level Throttling.
The purpose of API Gateway throttling is to prevent your API from being overwhelmed by too many requests.
Default limits. By default, API Gateway limits the steady-state requests to 10,000 requests per second, per region
Concurrent Requests. The maximm concurrent requests is 5,000 requests across all APIs, per region. You can request an increase on these limits
429 error. If you exceed 10,000 requests per second or 5,000 concurrent requests, you will receive a 429 TooManyRequests error message
example. 10,000 requests in the first ms
API Gateway will serve 5,000 of those requests immediately
It throttles the rest within the one-second period
Amazon API Gateway throttles requests to your API using the token bucket algorithm, where a token counts for a request. You can enable usage plans to restrict client request submissions to within specified request rates and quotas. This restricts the overall request submissions so that they don't go significantly past the account-level throttling limits in a Region. Amazon API Gateway provides Per-client throttling limits that are applied to clients that use API keys associated with your usage policy as client identifier.












## Lambda versions

When you create a Lambda function, there is only one version: $LATEST
When you upload a new version of the code to Lambda, this version will become $LATEST
You can create multiple versions of your function code and use aliases to reference the version you want to use as part of the ARN
e.g. In a development environment you might want to maintain a few versions of the same function as you develop and test your code
And alias is like a pointer to a specific version of the function code

if your application uses an alias, remember to update the ARN that you are using if you want to use the new code

Use Lambda versioning and aliases to point your applications to a specific version if you don't wnat to use $LATEST
If your application uses an alias, instead of $LATEST remember tht it will not automatically use new code when you upload it

Concurrent Executions
There is a concurrent execution limit for lambda
Safety feature to limit th number of concurrent executions across all functions in a given region per account
default 1000/s per region
TooManyRequestsException
HTTP status code: 429
Request throughput limit exceeded
you can request for a higher limit
reserved concurrency guarantees a set number of concurrent executions are always available to a critical function

If you have many Lambda functions running in the same region and you suddenly start seeing new invocation requests being rejected, then you many have hit you limit
At ACG, daily usage is around 6.5m Lambda invocaion per day in us-east-1
Request increase on this limit by submitting a request to the AWS Support Center
Reserved concurrency guarantees that a set number of executions which will always be available for your critical function, however also acts as a limit

Know tht the limit exists, 1000 concurrent executions per second
If you are running a serverless website like ACG, it's likely you will hit the limit at some point
If you hit the limit you will start to see invocations being rejected - 429 HTTP status code
The remedy is to get the limit raised by AWS support
Reserved Concurrency guarantees a set number of concurrent exectutions are always available to a critical function

Lambda accessing your VPC resources
by default your lambda will not be able to acess resources in a private VPC
To enable this, you need to allow the function to connect to the private subnet
Lambda needs the following VPC Configuration information so that it can connect to the VPC:
Private subnet ID
Security group ID (with required access)
Lambda uses this information to set up ENIs using an availablet IP address from your private subnet

You add VPC information to your lambda function config using the vpc-config parameter
aws lambda update-function-configuration --function-name my-function --vpc-config SubnetIds=subnet-1122aabb,SecurityGroupIds=sg-51530134

## Step Functions
Provide a visual interface for serverless applications, which enables you to build, and run serverless applications as a series of steps.
Each step in your application executes in order, as defined by your business logic
The output of one steap can be the input of the next step
Applications can have many Lambda functions, This helps manage the logic of your application. Including sequencing, error handling, retry logic so your application executes in order and as expected.
Step functions also loge the state of each step so when things go wrong you can diagnose and debug the problems quickly
State machines  (a workflow) consisting of tasks (a single step in the workflow)
1. Visualize. Great way to visualize your serverless application
2. Automate. Step functions automatically trigger and track each step. The output of one step is often the input to the next.
3. Logging. Step functions log the state of each step, so if something goes wrong you can track what went wrong and where


## X-Ray
xray is a tool which helps developers analyze and debug distributed pplications.
Allowing you to troubleshoot the root cause of performance issues and error.
Provides a visualiztion of your application's underlying components
Provides and end-to-end view or requess as they travel through your application
captures latency, HTTP status code and errors
Integrations
1.AWS services. You can use xray with EC@, Elastic Container SErvice, Lambda, Elastic Beanstalk, SNS, SQS, DynamoDB, Elastic Load Balancer and API GAteway
2. Integrate with your apps. You can use X-ray with applications written in Java, Node.js, .NET, Go, Ruby, Python
3. API Calls. The x-ray SDK automtically captures metadata for API calls made to AWS  services using the AWS SDK.
Architecture
1. Install the x-ray agent. Instll the agent on your ec2
2. Configure. Instrument your application using the X-ray SDK
3. The X-ray SDK. Gathers information from request and response headers, the code in your application, and metadata about the AWS resources on which it runs, and sends this trace data to X-ray. e.g. incoming HTTP requests, error codes, latency data

Used to help developers analyze and debus distributed applications.
Service Map. The service map is a visual representation of your application.
Xray Agent and SDK. Agent must be installed on your ec2 instance. Use the SDK to instrument your application to send traces to x-ray

The AWS X-Ray SDK sends the data to the X-Ray daemon (noth on your system) which buffers segments in a queue and uploads them to x-ray (the aws service) in batches
You need both x-ray SDK and the x-ray daemon on your systems
High level requirements/config steps
you need the xray SDK nd the xray daemon.
You use the SDK to instrument your application to send the required data e.g. data about incoming and outgoing HTTP requests that are being made to your java application

Configurations
1. On premises and Ec2 instances. Install the X-Ray daemon on your EC2 instance or your on premises servers
2. Elastic Beanstalk. Install the xray daemon on the ec2 instance inside your elastic beanstalk environment
3. Elastic container service. Install the x-ray daemon in its own docker container on your ECS cluster alongside your app

Annotations and Indexing
Annotations, when instrumenting your application, you can record additional information about requests by using annotations. Can record application specific information
These annotations are Key-value pairs that are indexed for use with filter expressions, so that you can search for traces that contain specific data and group related traces together in the console - e.g. game_name=snake, game_id=1243524542



## DynamoDB
Fast and flexible NoSQL Databse (no need to define a schema upfront)
Consistent, single-digit millisecond latency at any scale
Fully Managed. Supports key-value data models. Supports document formats are JSON, HTML and XML
Use Cases: A great fit for mobile, web, gaming, ad tech, IoT and many other applications

Serverless
Integrates well with Lambda
Dynaodb can be configured to automatically scale. A popular choice for developers and architects who are designing serverless applications.
Perrformance : SSD storage
Resilience: Spred across 3 geographically distinct data centers
Consistency: Eventual consistent reads (default), strongly consisten reads
Eventually Consistent Reads: Consisteny across all copies of data is usually reached within a second. Best for read performance.
Strongly Consistent Reads: A strongly consistent read always reflects all successful writes. Writes are reflected across all 3 locations at once. Best for read consistency.
ACID Transactions: DynamoDB Transactions provide the ability to perform ACID transactions (Atomic, Consistent, Isolated, Durable). Read or write multiple items across multiple tables as an all or nothing operation

Primary Keys
DynamoDB stores and retrieves data based on a primary key
Two types: partition key, composite key (partition key + sort key)

Partition key
based on a unique attribute (like a customer id, ,product id, email address, etc.)
value of the partition key is input to an internal hash function which determines the partition or physical location on which the data is stored.
If you are using the partition key as your primary key, then no 2 items can have the same partition key
Composite key
Partition key +Sort key
if partition key is not unique. (ex forum posts, users post multiple messages. -> Combination of User_id and sort key(timestamp))
a unique compbination: Items in the table may have the same partition key, but they must have a different sort key.
Storage: All items with the same partition key are stored together and then sorted according to the sort key value

Access Control
IAM: authentication and access control is managed using AWS IAM
IAM Permissions: you can create IAM users within your AWS account with specific permissions to access and create dynamoDB tables
IAM Roles: You can also create IAM roles, enabling temporary access to DynamoDB

Restricting User Access
You can also use a spcial IAM condition to restrict user access to only their own records
This can be done by adding a condition to an IAM Policy to allow access only to items where the partition key value matches their User_ID
the condition in the policy has "dynamodb:LeadingKeys" allows users to access only the items where the partition key value matches their user ID 
fine grained access control with IAM

Secondary Indexes
Flexible querying: Query based on an attribute that is not the primary key
Dynamodb allows you to run a query on non-primary key attributes using global secondary indexes and local secondary indexes
A secondary index allows you to perform fast queries on specific columns in a table. You select the columns that you want included in the index and run your searches on the index, rather than on the entire dataset

Local Secondary index
Primary Key: Same partition key as your orignial table but a different sort key
A different view: Gives you a different view of your data, organized according to an alternative sort key
Faster queries: Any queries based on this sort key are uch faster using the index than the main table
Add at creation time: Can only be created when you are creating your table. Tou cannot add, remove or modify it later
ex, user_id (same partition key) sort key:country

Global secondary index:
A completely different Primary Key:
Different partition key and sort ky
View your data differently: Gives you a completely different view of the data
Speeds up queries: Speeds up queries relating to this alternative partition and sort key
Flexible: You can create when you create your table or add it later
ex email address (different partition key) sort key: last login
There is an initial quota of 20 global secondary indexes per table. To request a service quota increase

query:
A query operation finds items in a table based on the primary key attribute and a distinct value to search for.
For example, selecting an item where the user ID is equal to 212 will select all the attributes for that item (e.g., first name, surname, email address)
Refine Queries
Use an optional sort key name and value to refine the results.
For example, if your sort key is a timestamp, you can refine the query to only select items with a timestamp of the last 7 days
By default, a query returns all the ttributes for the items you select, but you can use the ProjectionExpression parameter if you want to only return the specific attributes you want (e.g. if you only want to see the email address rather than all the attributes)
Sort Key: Results are always sorted by the sort key
Numeric order: By defaul in ascending numeric order (1,2,3,4,5)
ASCII: ASCII character code values
Reverse the Order: You can reverse the order by seting the ScanIndexForward parameter to False (only works for queries despite the name)
Eventually Consistent: By Default, queries are eventually consistent.
Strongly Consistent: You need to explicitly set the qery to be strongly consistent

scan:
A scan operation examined every item in the table. By default, it returns all data attributes
Use the ProjectionExpression parameter to refine the scan to only return attributes you want. (e.g., if you only want to see the email address rather than all the attributes)
Can use filters but with a scan it still goes over the whole table, dumping the data then refines the scan based on the filter.
Sequential by default: A scan operation processes dat sequentially, returning 1MB increments before moving on to retrieve the next 1 MB of data. Scans one partition at a time
Parallel is possible: You can configure DynaamoDB to use parrallel scans instead by logically dividing a table or index into segments and scanning each segment in parallel.
Beware. It is best to avoid parallel scans if your table or index is already incurring heavy read or write activity from other applications.
Isolate scan operations to specific tables and segregate them from your mission-critical traffice. Even if that means writing data to 2 different tables

Query or Scan:
1. Query is more efficient than a scan: A scan dumps the entire table and filters out the values to provide the desired result, removing the unwanted data
2. Extra Step: Adds an extra step of removing the data you don't want. As the table gorws, the scan operation takes longer
3. Provisioned Throughput: A scan operation on a large table can use up the provisioned throughput for a large table in just a single operation
Improving Performance. Set smaller page size (E.g. set the page size to return 40 items). Running a large number of smaller operations will allow other requests to succeed without throttling. But in general avoid scans.
Avoid using scan operations if you can. Design tables in a way that you can use the Query, Get or BatchGetItem APIs.

When using Query, or Scan, DynamoDB returns all of the item attributes by default. To get just some, rather than all of the attributes, use a Projection Expression.
A Scan operation in Amazon DynamoDB reads every item in a table or a secondary index. By default, a Scan operation returns all of the data attributes for every item in the table or index. You can use the ProjectionExpression parameter so that Scan only returns some of the attributes, rather than all of them

DynamoDB provisioned Throughput
Measured in Capacity Units
Specify Requirements: Whenyou create your table, you can specify your requirement in terms of read capacity units and write capacity units
Write capacity units: 1x write capacity unit = 1 x 1KB write per second
Read capacity units: 1 read capacity unit = 1 x strongly consistent of 4 KB per second or 2x everntually consistent reads of 4 KB per seond (default)
If your application reads or writes larger items, it will consume more capacity units and cost you more as well

On-Demand Capacity
Charges apply ro reading, writing and storing data
Dynamodb instantly scales up and down based on the activity of your application (don't need to specify read/write at creation time)
Great for:
unpredictable workloads
New applications where you don't know the use pattern yet
When you pay for only what you use (pay per request)

When to use each pricing model?
On-demand:
Unknown workloads
Unpredictabl application traffic
Spiky, short-lived peaks,
A pay-per-use model is desire
It might be more difficult to predict the cost.
Provisioned Capacity:
Reand and write capacity requirements can be forecasted
Predictable application traffic
Application traffic is consistent or increases gradually
You have more control over the cost

DynamoDb Accelerator (DAX)
it is a fully managed, custered in-memory cache for DynamoDB
Delivers up to 10x read performance imporvement. Microsecond performance for millions of requests per second
Ideal for read-heavy and bursty workloads like auction applications, gaming, and retail sites during black friday sales
Dax is a write-through caching service. Data is written to the cache and the backend stoer (the Dynamodb table) at the same time
This allows you to point your DynamoB API calls at the DAX cluster.
If the item you are querying is in the cache (cache hit), DAX returns the result
If the item is not available (cache miss), then DAX performs an eventually consistent GetItem operation against DynamoDB and returns the result of the API call.
Reduces the read load on DynamoDB tables.
May be able to reduce provisioned read capacity on your table and save money on your AWS bill
What is it not suitable for?
DAX Improves response times for eventual consistent reads onlny.
Not suitable for 
applications that require strongly consistent reads.
applications which are mainly write-intensive
Applications that do not perform many read operations
Applications that do not require microsecond response times

DynamoDB TTL (time to live)
Defines an expiry time for your data
Expired items marked for deletiong (will be deleted within 48 hours)
Great for removing irrelevant or old data (e,g session data, event logs and temporary data
Reduces the cost of your table by automatically removing data which is no longer relevant
expressed in UNIx/epoch time (numerical value of s since 1970)
When the current time is greater than the TTL, the item will be expired and marked for deletion
You can filter out expired items from your queries

DynmoDB Streams:
Time ordered sequence of item level modifications (e.g. insert, update, delete)
Logs encrypted at rest and stored for 24 hours
Can use to rigger lambda function
Dedicated endpoint
Primary key by default is recorded
Before and after images can also be stored
Use Cases: Audit or archive transactions, trigger an event based on a particular transaction, or replicate data across multiple tables
Applications can take actions based on contents of the stream
A dynamoDB stream can be an event source for lambda
Lambda polls the DynamoDB stream and executes based on an event
Summary:
1. Sequence of modifications: DynamoDB streams is a time-ordered sequence of item level modifications in your DynamoDB tables
2. Encrypted and stored: Data is tored for 24 hours only
3. Lambda Event Source: Can be used as an event source for Lambda, so you can create applications that take ctions based on events in your DynamoDB table

Provisioned throughput and Exponential Backoff
ProvisionedThroughputExceededException:
Your request rate is too high for the read/write capactiy provisioned on your DynamoDB table
Using the AWS SDK:
The SDK will automatically retry the requests until successful
Not using the AWS SDK:
Reduce your request frequecy. Use exponential Backoff

In addition to simple retries, all AWS SDKs use exponential Backoff
Uses progressively longer waits between consecutive retries, for improved flow control
ex< fail, wait 50ms, retry, if fail maybe wait 100ms, retry if fail again maybe 200ms retry.

If after 1 minute this doesnt work, your request size may be exceeding the throughput for your read/write capacity

## KMS

AS Key management service
Managed: A managed service that makes it easy for you to create and control the encryption keys used to encrypt your data
Integrated: Seamlessly integrated with many AWS srvices to make encrypting data in those services as easy as checking a box
Simple: With KMS, it is simple to encrypt your data with encryption keys that you manage
KMS is multi-tenant whereas CloudHSM is dedicated hardware. S3 Encryption and Client-Side encryption are not Key Management Solutions

When to use KMS?
Whenever you are dealing with sensitive information
Sensitive data that you want to keep secret
Customerdat, financial data, credentials, passowrds, secrets, etc.
What is a CMK:
Customer Master Key
Encrypt/decrypt data up to 4 kb
What is is used for?
Generate/encrypt/decrypt the Data Key.
Data Kaey
Used to Encrypt/decrypt your data
This is known as envelope encryption

Summarys CMK:
Alias: Your application can refer to the alias when using the CMK
CreationDate: The date and time when the CMK was created
Description: You can add your own description to describe the CMK
Key State: Enabled, Disabled, pending deletion, unavailable
Key Material: Customer-provided or AWS-provided
Stays inside KMS: Can never be exported

Set up CMK:
Create alias and description. Choose key material option (KMS, own, CloudHSM)
Key Administrative Permissions: IAM users and roles that can administer (but not use) the key through the KMS API
Key Usage Permissions: IAM users and roles that can use the key to encrypt and decrypt data

AWS-Managed CMK:AWS-provided and AWS-managed CMK. Used on your behalf with the AWS services integrated with KMS
Customer-Managed CMK: You create, own and manage yourself
Data Key: Encryption key that you can use to encrypt data, including large amounts of data. You can use a CMK to generate, encrypt and decrypt keys

Understanding KMS API calls:
aws kms encrypt: Encrypts plaintext into ciphertext by using a customare master key
aws kms decrypt: Decrypts ciphertext that was encrypted by an AWS KMS customer master key (CMK)
aws key re-encrypt: Decrypts ciphertext and then re-encrypts it entirely within AWS KMs (e.g. you change the CMK or manually rotate the CMK)
aws kms enable-key-rotation: Enables autoomatic key rotation every 365 days
aws kms generate-data-key: Uses CMK to generate a data key to encrypt data >4kb

Envelope Encryption
A process for encrypting your data. it applies to file > 4kb in size
why use it?
network performance. Whe you encrypt data directly with KMS it must be transeferred over the network
Performance. With envelope encryption, only the data key goes over the network, not your data
Benefits. The data key is used locally in your application or AWS service, avoiding the need to transfer large amounts of data to KMS
Encrypting the key that encrypts our data.
The CMK is used to encrypt the data key (or envelope key)
The data key encrypts our data
Used for encrypting anything over 4 KB
Using envelope encryption avoids sending all your data into KMS over the network
Remember the GenerateDataKey API call

## SQS
simple queue service (first ever services AWS had)
a message queue service
Enables web applications to quickly and reliably queue messages that one component in the application generates for another component to consume
A queue is a temporary repository for messages awaiting processing
Features:
1. Decouple Application Componenet: Decouple the componenet of an application so they run independently, easing message management between components
2. Store messages: Any component of a distributed application can store messages in the queue. Messages can contain up to 256 KB of text in any format.
3. Retrieve Messages. Any component can later retrieve the messages programmatically using the Amazon SQS API
It acts as a buffer between the component receiving the data for processing and the component producing and saving the data
The queue resolves issues that arise if the producer is producing work faster than the consumer can process it, or if the producer or consumer are only intermittently connected to the network
Key-Facts:
Pull based: SQS is pull-based, not pushed based
256 KB: Messages are 256 KB in size
Text Data: Including XML, JSON and unformatted text
Guarantee: Messages will be processed at least once
Up to 14 days: Messages can be kept in the queue from one minute to 14 days
Default Retention: The default retention period is 4 days
It is a distributed message queueing system, Allows us to decouple the componenets of an application so that they are independent
It is pull based

Types of queues:
Standard queues:
Dafault, provides best effort ordering
Unlimited transactions: Nearly-unlimited number of transactions per second
Guarantee: Guaranteed that a message is deliveredat least once
Best-Effort Ordering: Standard queues provide best-effort ordering which ensures that messages are generally delivered in the same order as they are sent.
Occasionally (because of the highly-distributed architecture that allows hight throughput),
more than one copy of a message might be delivered out of order
occasional duplicated


FIFO (first in first out):
also available
First in first out delivery: The order in which messages are sent and received is strictly perserved
Exactly -Once Processing:
A message is delivered once and remains available until a consumer processes and deletes it. Duplicates are not introduced
300 TPS Limit: FIFO queues are limited to 300 transactions per sescond (TPS) but have all the capabilities of standard queues
good for banking transactions which need to happen in a strict order and no duplicates

Settings:
Visibility Timeout:
Visibility timeout is the amount of time that the messagge is invisible in the SQS queue after a reader picks up that message
Ideally, the job will be processed within that time, the message will become visible again and another reader will process it
Deffault: 30 seconds
Increase if necesary: Increase if the task takes more than 30 seconds
Maximum: The maxiumum is 12 hours
to change use the ChangeMessageVisibility API call

Short polling vs Long polling:
Short Polling:
Returns a response immediately even if the message queue being polled is empty
This can result in a lot of empty responses if nothing is in the queue
You will still pay for the responses
Long Polling:
long polling max is 20s
Periodically polls the queue
Doesn't return a response until a message arrvies in the message queue or the long poll times out
Can save money!
Long polling is generally preferable to short polling

Delay queues
SQS Delay Queues - postpone delivery of new messages
- Postpone delivery of new messages to a queue for a number of seconds
- Messages sent to the Delay queue remain invisible to consumers for the duration of the delay period
- Default is 0 seconds, maximum is 900
- For standard queues, changing the setting doesn't affect the delay of messages already in the queue, only new messages.
- For FIFO queue, this affects the delay of messages already in the queue
When should you use a Delay queue
- Large distributed applications which may need to introduce a delay in processing
- You need to apply a delay to an entire queue of messages
- e.g. adding a delay of a few second, to allow for updates to your sales and stock control databases before sending a notification to a customer confirming an online transaction

Best practice for Managing Large SQS Messages Using s3
-For large SQS messages -256KB up to 2GB in size
- Use s3 to store the messages
- Use Amazon SQS Extended client Library for java to manage them
- (you'll also need the AWS SDK for Jave) - provides an API for S3 bucket and object operations
SQS Extended Client Library for Java allows you to:
- Specify that messages are always stored in Amazon s# or only messages > 256 KB
- Send a message which reference a message object stored in S3
- Get a message object from s3
- Delete a message from S3 
You need:
AWS SDK for Java
SQS Extended Client Library for Java
An S3 bucket
Can't use:
AWS CLI
AWS MAnagement console/SQS console
SQS API
any other AWS SDK

## SNS

Simple Notification Service
A web service that makes it easy to set up, operate and send notifications from the cloud

Messages sent from an application can be immediately delivered to subscribers or other applications
Basically a notification service

Can create Push notifications to devices (e.g. apple, google, fire os, windows, android)
SMS and Email: SMS test messages or email to Amazon simple queue service(SQS) queues or any HTTP endpoint
Lambda: Trigger Lambda functions to process the information in the message, publish to another SNS topic or send the messages to another AWS service
Uses a pub-sub model:
Publish and subscribe model: Applications publish or push messages to a topic
Subscribers receive messages from a topic

Notifications are delivered using a push mechanism that eliminates the need to periodically check or poll for new information and updates
Its an access point, allowing recipients to subscribe to, and receive identical copies of the same notification
SNS delivers appropriately-formatted copies of the message to each subscriber (whether it's iOS, Android, SMS, etc.)
Durable Storage
Prevents messagese from being lost
All messages to SNS are stored redundantly across multiple AZs
Instantaneous: Instantaneous, push-based delivery (no polling)
Simple: Simple APIs and easy integration with applications
Flexible: Flexible message delivery over multiple transport protocols
Inexpensive: Inexpensive, pay-as-you-go model, no upfront cotsts
Easy to configure: Web Based AWS Management Console offers the simplicity of a point and click interface
Managed Service: With all the high availability and durability features for a production environemt
highly available notification service that allows us to send push notifications from the cloud
supports text messages, email, SQS queues or any HTTP endpoint
Pub-sub model whereby users subscribe to topics. Push mechanism rather than a pull (or poll mechanism)

SNS:
messaged service
SNS push based
think push notifications

SQS
Also messaging service, held in a queue instead of delivevring
SQS pull-based (polling)
Think polling the queue for messages

## SES
Simple email service
1. Scalable and highly availble email: Designed to help maarketing teams and application developers send marketing, notification, and transactional emails to their customer using pay as you go model
2. Send and REceive emails: Can also be used to receive emails with incoming mails delivered to an s3 bucket
3. Trigger lambda and SNS: Incoming emails can be used to trigger Lambda functions and SNS notifications

Automated Emails for ex:
Notification that there is a new post in a discussion you moderate for ex
Online purchases: purchase confirmations, shipping notifications and order status updates
Marketing Emails: Marketing communications, advertisements, newsletters, special offers, new products and Black Friday specials

SES:
Email messaging service
Can trigger a lambda function or SNS notification
Can be used for both incoming and outgoing email
An email address is all tht is required to start sending messages
for email only


SNS:
Pub/Sub messaging service; formats include SMS, HTTP, SQS, email
can trigger a Lambda function
Can fanout messages to a large number of recipients (replicate and push messages to multiple endpoints and formats)
Consumers must subsrcribe to a topic to receive the notifications
no used for receiving notifications

## Kinesis
a family of services which enables you to collect, process and analyze streaming data, in real-time
Allows you to build custom applications for your own business needs
Kinesis is a greek word meaning motion or movements
AMazon Kinesis deals with data that is in motion, or streaming data

Streaming Data?
Data generated continuously by thousands of data sources, which typically send in the data records simultaneously and in small sizes (KB)
- Financial transactions
- stock prices
- game data (as the gamer plays)
- social media feeds
- Location tracking data (Uber)
- IoT sensors
- Clickstream data
- Log files

Kinesis Streams:
Stream data and video to allow you to build custom applications that process data in real time
Data stream and video streams
Shards: only in kinesis streams
Kinesis streams are made up of shards
Each shard is a sequence of one or more data records ands provides a fixed unit of capacity.
5 reads per second. The max total read rate is 2MB per second
1000 writes per second. The max total write rate is 1MB per second
The data capacity of the stream is the sum total capacity of its shards
If the data rate increases, you can increase capacity on your stream by increasing the number of shards
retains data up from 24hours to a week
shards continue to consumers to process
A shard is a sequence of data records, each with their own unique sequence number
As your data rate increases, you increase the number of shards (resharding)
What abou consumers?
- Kinesis client library runs on the consumer instances.
- Tracks the number of shards in your stream
- Discovers new shards when you reshard
Kinesis Client library:
- The KCL ensures that for every shard there is a record processor
- Manages the number of record processos relative to the number of shards and consumers
- If you have only one consumer, the kcl will create all the record processors on a single consumer
- If you have 2 consumers it will load balance and create half the processors on one instance and half on another
if 4 shards to one consumer than 1 consumer/4 record processors, if 2 consumers than 2 record processors
What about scaling out the consumers?
- With KCL, generally you should ensure that the number of instances does not exceed the number of shards (except for failure or standby purposes
- You never need multiple instances to handle the processing load of one shard
- However, one worker can process multiple shard
- it's fine if the number of shards exceeds the number of instances
- Don't think that just because you reshard, that you need to add more instance
- Instead CPU utilisation is what should drive the quantity of consumer instances you have, NOT the number of shards in your kinesis stream
- Use an auto scaling group, and base scaling decision on CPU load on your consumer
Kinesis Shards:
- The Kinesis Client Library running on your consumers creates a record proccessor for each shard that is being consumer by your instance
- If you increase the number of shards, the KCL will add more record processors on your consumers
- CPU utilisation is what should drive the quantity of consumer instances you have, NOT the number of shards in your kinesis stream
- Use an auto scaling group and base scaling decisions on CPU load on your consumers

Kinesis Video streams: Securely stream video from connected devices to AWS.
Videos can be used for analytics and machine learning

Kinesis data firehose:
Capture, transform, load data streams into AWS data stores ( or other service providers like Splunk or datadog) to enable near real-time analytics with BI tools
no data retention
optional lamda can process while it comes in
then save to storage
no shards and no consumers

Kinesis Data Analytics:
Analyze, query and transform streamed data in real time using standard SQL. tore the results in an AWS data store (like S3, redshif)
sits after kinesis streams or data firehose

## Elastic Beanstalk
Deploy and scale web applications including the web application server pplatform
Widely used application server platforms
Supported languages: Java, .NET, PHP, Node.js, Python, Ruby, Go and Docker
Supported platforms: Apache http server, Tomcat, Nginx, Passenger, IIS, puma
Developers: Focus on writing code and don't worry about any of the underlying infrastructure needed to run your application

1. Infrastructure: Provisioning infrastructure, load balancing, autoscaling and application health  monitoring
2. Application Platform: Installation and management of the application stack including patching and updates to your OS and application platform
3. You are in control: You have complete administrative control of the AWS resources. No additional charges for using Elastic Beanstalk. You are only charged for the resources deployed
4. or it can take care of system admin: OS and appliaction server updates. Monitoring, etrics and health checks are all included.

Great for developers
Focus on writing code
You don't need to worry about any of the underlying infrastructure needed to run the application
Get your application to market faster
Fastest and Simplest way to deploy your application in AWS

Updating options for deployment updates
1. All at once: deploys to all hosts concurrently
deploys to all instance simultaneousle
you will experience a total outage
Not ideal for mission-critical production systems
If the update fails, you need to roll back to the changes by re-deploying the original version to all your insance, resulting in another outage to get back to the previous version.
Not good for production
involves a service interruption (and a second if rollback is required)

2. Rolling: Deploys the new version in batches
Deploys the new version in batches. Each batch is taken out of service while the deployment takes place
Your environment capacity will be reduced by the number of instances in a batch while the deployment takes place
Not ideal for performance sensitive systems
rollback will again slow down performance
reduced capacity during deployment

3. Rolling with additional Batches: Launches an additional batch of instances. Then deploys the new version in batches
Launches an additional batch of instances. 
Deploys the new version in batches
Maintains full capacity throughout the deployment
If the update fails, you need to perform an additional rolling update to roll back the changes
takes a while to rollback changes
Maintains full capacity, rolling back takes time as requires a further rolling update

4. Immutable: Deploys the new version to a fresh group of instances before deleting the old instances
Deploys the new version to a fresh group of instances
Only when the new instances pass their health checks, should the old instances be terminated
If a deployment fails, just delete the new instances
little disruption to production
This is the preferred approach for mission critical systems
maintains full capacity


5. Traffic Splitting: Installs he new version on a new set of sintance like an immutable deployment but forwards a percentage of incoming client traffic to the new application version for evaluation.
Enables Canary testing
Installs the new version on a new set of instances julst like and immutable deployment
Forwards a percentage of incoming client traffice to the new application vesion for a specified evaluation period
If the new instances stays heathly. Elastic beanstalk will forwards 100% of the traffic to them and terminated the old one
Like the canary in the coal mine, this provides an early indication that something is wrong


You can customize your elastic beanstalk environment using Elastic Beanstalk configuration files
Configuration Define packages to install, create linkux users and groups, run shellcommands, enable services, load balancer configurations.
YAML or JSON: These are files written in yaml or json format
Constraints: the file must have a .config extension and be inside a folder called .ebextensions
.ebextensions folder:
1. Location: Must be included in the top-level directory of your application source code bundle.
2. Source Control: The configuration files can be placed under source control along with the rest of your application code
myhealthcheckurl.config: ex
Configures an application health check url which will be used by an Elastic load balancer. The load balancer will make an HTTP request to the specified path to check if the instances are healthy

Elastic beanstalk supports 2 ways of of integrating an RDS database with your Beanstalk environment. Deploing RDS inside your Elastic Beanstalk environment or outside of it
Launch within the enviironment:
- Launch the RDS instance from within the ELastic Beanstalk console
- It is created within your elastic beanstalk environemtn
- If you terminate the environment, the database will also be terminated
- It is a good option for Dev and Test deployments, not so good for prod
- quick and easy


Outside:
Don't use elastic beanstalk to create your RDS database
Instead, use the RDS console or AWS cli
It allows you to tear down your application environment without affecting your database
Preffered way for production
1. And additional security group must be added to your env Auto Scaling group
2. youll need to provide connection string information to your application servers using elastic beanstalk environment properties

# CI/CD
1. Software development best practice: Continuoous integrations, continuous delivery/deployment
2. Make small changes and Automate everything: Small, incremental code changes. Automate as much as possible e.g. code integration, build, test and deployment
3. Why? Modern companies like AWS, Netflix, Google and Facebook have pioneered this approach to releasing code, successfully applying thousands of changes per day

Automations is good:
fast, repeatable, scalable and enables rapid deployment
Manual is bad:
slow, error prone, inconsisten, unscalable, complex
Small changes:
Test each code change and cach bugs while they are small and simple to fix

Workflow:
Shared code repository: Multiple developers contributing to a shared code respooistory like Git. Frequently merging or integrating code updates.
Automated build: When canges appear in the code repository this triggers an tuomated build of the new code
Automated tests: Run automated tests to check the code locally before it is commited into the master code repository
Code is merged: After successful tests, the code gets merged to the master repository
Prepared for deployment: Code is built, tested and packaged for delivery
Manual Desciesion: Humans may be involved in the decision to deploy the code. This is known as continuous Delivery
Or Fully Automated. The system automatically deploys the new code as soon as it has been prepared for deployment. This is known as Continuous Deployment

Tools:
Codecommit: Source and version control
Source control service enabling teams to collaborate on code, html pages, scripts, images and binaries
Codebuild: Automated build
Compiles source code, runs test and produces packages that areread to deploy.
Codedeploy: Automated deployment
Automates code deployments to any instance, including ec2, Lambda and on-premises
CodePipeline: Manages the workflow
End-to-end solution, build, test and deploy your application every timme there is a code change

## Codecommit

Manaages updated from multiple sources
Enables collaborations
Tracks and manages code changes
Mantains version history

## Codeploy
Automated deployment service
works with ec2, onpremisses and lambda
Quickly release new features
Avoid downtime during deployments
Avoid the risks associated with manual processes

Code deploy deployment approaches
1. In place: The application is stopped on each instance and the new release is installled. Aslo known as a rolling update
The application is stopped on the first instance (1 at a time for your instances)
Theinstance will be out of service during the deployment so capacity is reduced
You should configure your Elastic Load Balancer to stop sending the requests to the instance
Code deploy installs the new version known as a revision
the instance comes back into service
Codedeploy continues to deploy to the next instance
lambda is not supported
great when deploying the first time
Rollback:
theres no quick fix
with an inplace deployment, you will need to redeploy the previous version, through the same process
time consuming
2. Blue/Green: New instances are provisioned and the new release is installed on the new instance. Blue represents the active deployment, green is the new release
Blue represenest the current version of our application
Codedeploy provisions new instances (green)
The new revision is deployed on the green instances
The green instances are registered with the elastic load balancer
Traffice is routed away from the old environment
The blue environment is eventually terminated once everything looks good
no capactiy reduction
you pay for 2 environments until you terminate the old servers
Rollback:
easy
Set the load balancer back to the original environment
easy to switch between the old and new releases
only works if you didn't already terminate your old environment

Codedeploy AppSpec file:
Configuration fule: Defines the parametes that are going to be used during a codedeploy deployment
Ec2 and on premises systems, yaml only
Lambda, Yaml and json supported. File structure depends on whether you are deploying to lambda or ec2

version: Reservved for future use
Currently the allowed value is 0.0
OR: Operating system version
The operating system version you are using e.g. linux, windows
files: Configurations file, packages
The location of any application files that need to be copied and where they should be copied to
Hooks: Lifecycle event hooks
Scripts which need to run at set points in the deployment lifecycle. Hooks have a very specific run order

Scripts that you might run during a deployment:
1. Unzip files: unzip application files prior to deploment
2. Run tests: Run functional tests on a newly deployed application
3. Deal with Load Balancing: De-register and re-register instances with a load balancer

Typical folder setup:
in root folder appsec.yml
/Scripst
/Config
/Source
the appse.yml must be places in the root folder of your deployment

Codedeploy Lifecycle event hooks:
lifecycle event hooks are run in a specific order known as the Run Order
3 phases for in-place deployment
Phase1: Deregister instances from a load balancer
life hooks: 
BeforeBlockTraffic: Before, tasks you want to run on instances before they are deregistered from a load balancer
BlockTraffic: de-register instances from a load balancer
AfterBlockTraffic: Tasks you want to run on instances after they are deregistered from a load balancer

Phase2: The real nuts and bolts of the application deployment
lifecycle event hooks:
ApplicationStop: commands to gracefullt stop the application
DownloadBundle: Codedeploy agent copies the application revision files to a temporary location
BeforeInstall: Preinstallation scripts e.g. backing up the current version, decrypting files
Install: Copy aplication files to final locations
AfterInstall: Post-install scripts e.g. configuration, file permissions
ApplicationStart: Start any services that were topped during ApplicationStop
ValidateService: Run tests to validate the service

Phase3 : Re-register the instances with the load balancer
Lifecycle event hooks:
BeforeAllowTraffic: tasks you want to run on instances before they are registered with the load balancer
AllowTraffice: Register instances with a load balancer
After: Tasks you want to run on instances after they are registered with a load balancer

## CodePipeline
Fully managed Ci/CD service
1. Orchestrates Build, Test and Deployment: The pipeline is triggered every time there is a change to your code. Like a condtuctor to an orchestra
2. Automated Release Process: Fast, consistent, fewer mistakes. Enables quick release of new features and bug fixe.
3. Codepipeline integrates with: Codecommit, Codebuild, codedeploy, github, jenkins, elastic beanstalk, cloudformation, lambda ECS

Codepipeline: Workflow is defined"The workflow beings when there is a change detected in your source code
CodeCommit: New code appears: New source code appears in the codecommit repository
Code is built and teted: Codebuild immedieately compiles source code, runs test and produces packages
CodeDeploy: Application Deployed: The newly built application is deployed into a staging or production environment

Orchestrates your ened to end software reease process based on a workflow you define
utomated: automatically triggers your pipeline as soon as a change is detected in your sourcee code

## ECS
What are they: imilar to a virtual machine, more like a virtual operating environment
Standardized: A standarized unit with unit with everything the software needs to run e.g. libraries, system tools, code and runtime
Microservices: Applications are created using independent stateless components or microservices running in containers
Docker or Windows Containers: Use docker to create linux containers and windows containers for windows workloads

Architecutre of a conainer: code, liraries, virtual kernel (all three in a container) that then run on docker
advantages:
Highly scalable: If the pplication becomes over loaded, scale on the services you need to
Fault tolerant: a single error in one of your own containers shouldn't bring down your entire app
Easy to maintain: Easier to maintain, update than large monolithic applications

Wehere does ecs fit?
A container orchestration service which suppports docker and windows containers
Quickly deploy and scale containerized workloads without having to install, configure, manage and scale your own orchestration platform
Similar to kubernetes, but with deep integration with AWS services e.g. IAM, VPC, Route53

fargate or ec2?
ECS: Clusters of virtual machines: ECS will run your containers on clusters of virtual machines
Fargate for serverless: Use fargate for serverless containers and you don't need to worry about the underlying EC2 instances.
EC2 for more control: If you want to control the installation, configuration and management of your compute environment.

ECR: elastic container registry
This is where you can store your container images. Docker or windows container
AWS services that use ECS
sagemaker, amazon lex, amazon.com

containers:
virtual operating environment with everything the software needs to run
includes libraries, system tools, code and runtime
Allows applications to be built using independent stateless components or microservices running in multipple containers

Docker commands to build, tag (apply an alias and push your docker image to the ecr repository
Build: docker build -t myimagerepo
Tag (apply alias): docker tag myimagerepo:latest 72530006743.dkr.ecr.eu-central-a.amazonaws.com/myimagerepo:latest
push image to registry: docker push 72530006743.dkr.ecr.eu-centra-1.amazonaws.com/myimagerepo:latest

buildspec.yml File:; Use buildspec.yml to define the build commands and settings used by codebuild to run your build
Override buildspec.yml Settings: You can override the settings in buildspec.yml by adding your own commands in the console when you launch the build
Trouble shooting Failures: If your build fails, check the build logs in the CodeBuild concole, and you can also view the full Codebuild log in Cloudwatch

Deploying docker with elastic beanstalk:
1. Elastic beanstalk supports the deployment of docker containers
2.  Docker containers are self-contained and include all the conifguration information and software your web application requires to run - think libraries, system tools, code and runtime
3.  Elastic Beanstalk handles the capacity provisioning, load balancing scaling, and application health monitoring

Deploy:
- Single docker container: You can either run a single docker container on an ec2 instance provisioned by elastic beanstalk
- Deploy your code: Upload a zip file containing your code bundle and elastic beanstalk will do the rest
- Multiple Docker containers: Use elastic beanstalk to build ecs cluster and deploy multiple docker containers on each instance
- Upgrade your code: If you want to upgrade your application to a new version, it's one easy step in the console to upload and deploy
- Code can be uploaded directly from your local machine or  public s3 bucket
- You can also store your code in codeCommit - but must use the elastic beanstalk cli

## Cloudformation
Manage, configure and provision your AWS infrastructure code

resources are defined using a cloudformation template
Cloudformation interprets the template and makes appropriate API calls to create the resources you have defined
Supports YAML or JSON

Benefits:
Consistent: Infrastructure is provisioned consistently with fewer mistakes
Quick and effictient: Less time and effort than configuring things manually
Version Control: You can version control and peer review your templates
Free to use: Free to use but you are charged for the aws resources you create using CloudFormation
Manage updates: Can be used to manage updates and dependencies
Rolling back: You can roll back to a previous state and delet the entire stack as well

YAML or JSON template: YAML or JSON template used to describe the end state of the infrastructure you are either provisioning or changing
S3: After creating the template, you up load it to CloudFormation using s3
API calls: CloudFormation reads the template and makes the API calls on your behalf
CloudFormation Stack: The resulting set of resources that CloudFormation builds from your template is called a "stack"

Template:
- Resources section is Mandatory: Resources is the only mandatory section of the cloudformation template
- The transform section is for referencing additional code: The transform section is used to refernce additional code stored in S3, allowing for code re-use. E.g. Lambda code or template snippets/reusable prices of CloudFormation code

Infrastructure as code: Cloudformation allows you to maage, configure and privision aws infrastructure as YAML or JSON code
Parameters: Input custom values
Conditions: e.g. provision resources based on environment
Resources: This section is mandatory and describes the AWS resources that CloudFormation ill create
Mappings: Allows you to create custtom mapping like region:AMI
Transform: Allows you to reference code located in S3 e.g. lambda code or reusable snippets of CloudFormation code

Cloudformation and SAM (Serverless Application Model)
1. CloudFormation for serverless: The Serverless Application Model is an extension to CloudFormation to define serverless applications
2. Simplified Syntax: SAM uses a simplified syntax for defining serverless resources, APIs, Lambda functions, DynamoDB tables,etc.
3. SAM CLI: Use the SAM CLI to package your deployment code, upload it to S3 and deploy your serverless application
defines and provision serverless applications using CloudFormation, has its own cli and own commands

sam package
pacages your application and uploads to s3
sam deploy
deploys your serverless app using Cloudformation

Cloud formation Nested Stacks:
Nested stacks: Enables re-use of Cloudformation code for common cases
E.g. standard configureatin for a load balancer, web server or application server
Instead of copying out the code each, time, create a standard template for each common use case and reference from within your CloudFormation template

Correct. By default, the “automatic rollback on error” feature is enabled. This will direct CloudFormation to only create or update all resources in your stack if all individual operations succeed. If they do not, CloudFormation reverts the stack to the last known stable configuration.
The Transform section specifies one or more macros that AWS CloudFormation uses to process your template. The Transform section builds on the simple, declarative language of AWS CloudFormation with a powerful macro system. The declaration Transform: AWS::Serverless-2016-10-31 is required for AWS SAM template files.

How can you prevent CloudFormation from deleting your entire stack on failure?


Use the --disable-rollback flag with the AWS CLI

Set the 'Rollback on failure' radio button to No in the CloudFormation console

Correct. This AWS CLI option disables the rollback of the stack if stack creation fails.

Correct. This AWS CloudFormation console option disables the rollback of the stack if stack creation fails.




Transforms is used to reference code located in S3 and also for specifying the use of the Serverless Application Model (SAM) for Lambda deployments.



AppSpec files on an EC2/on-premises compute platform must be a YAML-formatted file named appspec.yml and it must be placed in the root of the directory structure of an application's source code. Otherwise, deployments fail. Reference: CodeDeploy AppSpec File reference.

## Web identity Ferderation
Simplifies authentication and authorization for web application

- User accecss to AWS resources: Users access AWS resources after successfully authenticating with a web-based identity provider like Facebook, Amazon, or Google
- Authentication: Following successful authentication, users receive an authentication code from the web ID provider
- Authorization: Users can trade this authentication code for temporary AWS security credentials. authorizing access to AWS resources

Amazon Cognito: Provides web ideneity federation including signup and signin funtiontiliy for your applications and access for guest users
Identity broker: Manages autentication between you application and web id providers, so you don't need to write any additional code
Multiple Devices: Synchronizes user data to multiple devices
Recommended for mobile: Recommended for all mobile applications that cal AWS services
AWS recommended best practice for web id federation for mobile applications 

The temporary credentials map to an iam role, allowing access to the required resource
No need for the application to embed or store AWS credentials locally on the device

User Pools: User directories to manage the signup and sign in for mobile and web applications
Signin: Users can sign in directly to the user poo or using Facebook, google or amazone
Idenetiy Pools: Identiy pools enable you to provide temporary AWS credentials. Enabling access to AWS services like s3 or Dynamodb

sign in user pool brokers with facebook then returns a JWT whhich is used for the identity pool wihich gives an IAM role

Cognito push synchronization:
synchronization across all devices
Devices: Cognito tracks the assocation between user identity and the various different devices they sign-in from
Seamless. Cognito uses push Synchronization to push updates and synchronize user data across multiple devices
SNS silent notifications: SNS notification to all the devices associated with a given user identity whenever data stored in the cloud changes
(for example updating his address on one device get synchronized across all device associated with account)

An authentication token (JWT token) is exchanged for temporary AWS credentials, allowing users to assume an IAM role, with permission to access AWS resources


User pool v idenety pools
U.P:
Userdirectory used to manage sign up and sign in functionality for mobie and web applications.
I.P:
identiy pools enable you to provide temporary AWS credentials.
enabling accesss to AWS services like S3 or Dynamodb



























































