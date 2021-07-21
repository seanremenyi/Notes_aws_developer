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























