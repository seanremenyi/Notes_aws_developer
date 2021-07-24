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

1. Unlimited storage
total vloume of data and the number of objects you can store is unlimited
2. objects can range in size from a minimum of 0 bytes to a max of 5 tb
3. S3 buckets
stores files in buckets (similar to folders) 

working with s3 buckets
1. Universal Namespace
all aws accounts share the s3 namespace. Each s3 bucket name is globally nuique
Example:
https://{bucket-name}.s3.Region.amazonaws.com/{key-name}
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













