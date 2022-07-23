# EC2
* Elastic Compute Cloud
* Infrastructure as a Service (IaaS)
* With EC2, we can rent Virtual Machine 

## EC2 Configuration Options
* OS - Linux/Mac/Windows
* RAM 
* CPU or no.of cores
* Storage (EBS/EFS)
    * Physical Storage - EC2 Instance Store
* Network Card 
* User Data Script - To bootstrap EC2 instances
    * This script is run only during the first start of the EC2 instance
    * It runs with the root user. So it had has _sudo_ previliges
    * Used to:
        * Install packages
        * Update/Upgrade packages
* Firewall - Security Groups to control incoming and outgoing traffic
___
* AWS Free Tier provide 750 hrs of t2.micro and 30GB of EBS volume storage per month per Organization

## EC2 instance Types
* General Purpose EC2 Instances - Balanced; webservers and coderepos
* Compute Optimized EC2 instances - For compute-intensive tasks
* Memory Optimized EC2 instances - In-memory DBs, cache
* Storage Optimized EC2 instances - Databases
... and some moree!
```
t2.micro
```
* Here, `t` denotes the class of EC2 instance
* `2` denotes the generation (or version). There could be newer generations in the future
* `micro` is the size of the instance. It could be anything like `large`, `small`, `2xlarge`

## Security Groups
* It's basically a firewall for EC2 instances
* It controls the incoming traffic and outgoing traffic from/to the EC2 instances
* But it lives outside the EC2 instances, so if a traffic is blocked, an EC2 instance would not even see it
* SG contains only _allow_ rules
    * All inbound traffic is blocked by default
    * All outgoing traffic is allowed by default
* If you're launching a web application in an EC2 instance, and you're seeing a timeout error, it means that it is a SG issue
    * If it's a connection refused error, then it's an application issue 
* SGs are referenced by IP adresses or other SGs
    * That is, you define which IP
* One SG can be attached to multiple instances
* And similarly, one instance can have multiple SGs
* SGs are scoped to a region/VPC

## Commmon Ports
* 22 - SSH
* 21 - FTP 
* 22 - SFTP (Secure FTP). File transfer using SSH
* 80 - HTTP
* 443 - HTTPS
* 3389 - RDP (Remote Desktop Protocol) for logging into Windows instance

## Connecting to an instance
* There are three ways that you can connect to an EC2 instance
    * SSH - can be used Linux/Mac/Windows10+
    * PuTTy - can be used for any version of Windows
    * EC2 Instance Connect - Can connect to EC2 instance from the AWS console  
        * You need to have port 22 enabled to use EC2 instance connect

## EC2 Instance Purchasing Options
* On-demand
    * Billed per second, after the first minute on Linux and Windows
    * Billed per minute on other OS
* Reserved Instances
    * 1 or 3 years commitment; 
    * Here, you reserve Instance type, Region, tenancy, OS
    * High Discount | full upfront > partial upfront > no upfront | Low Discount
    * _Convertible Reserved Instance_ option available, where instance type, instance family, region, tenancy can be changed 
* Savings Plan
    * 1 or 3 years commitment;
    * High Discount || full upfront > partial upfront > no upfront || Low Discount
    * Committed to **certain amount**. Billed on On-demand price after exhausting savings plan
    * Locked to Instance family and Region. Flexible across Instance Size, OS, Tenancy
* Spot Instance
    * Highest Discount
    * Used to run applications resilient to failure
    * Instance is lost if bid price is greater than current price 
* Dedicated Host
    * Most expensive option
    * Complete physical server dedicated to use
    * Controls instance placement
    * Reserving options available for more discount
    * This option allows to use server-bound software licenses 
* Dedicated Instances
    * Instances run on dedicated hardware
    * Hardware may be shared with instances in the same account
* Capacity Reservations
    * Allows to reserve capacity for EC2 instances in a specific AZ
    * No time commitment
    * You are billed even if instance is not running because the capacity is reserved
    * Charge at On-demand rate

## Spot Instances
* To initiate a spot request, you specify the following
    * Max Price
    * No. of instances
    * Launch Configuration
    * Request type -> One-time / Persistent
    * Valid From, Valid Until

* If the spot price is greater than the max price, the instance is lost
    * You are given two operations to perfrom within a grace period of two minutes after the price becomes greater
        * Stop
        * Terminate
    * Stopping the instance will allow you to restart the instance if the spot price drops below the max price again
    * However, fresh instances are started if the instances are configured to be terminated
* Spot prices do vary based on AZ

* There is another option called _Spot Block_ which is outdated and only available till Dec 2022
* In this, the spot instances are reserved from anywhere between 1 to 6 hours 

* If the request type is Persistent, then the Spot Request keeps on requesting for spot instances once the instances are terminated. 
    * So, if you need to actually stop an instance, you need to cancel the spot request first and then stop the instances
    * Because, if you stop the instance first, the request is automatically going to request another instance
    * And cancelling a spot request does not terminate spot instances. You need to terminate them seperately 
* Whereas, if the request type is 'one-time', then it only requests for spot requests once until the request is satisfied

### Spot Fleet
* Spot Fleet = Spot instances + On-demand instances
* You define a number of launch pools for Spot fleet to choose from
    * Launch pools consists of variations of OS, AZ, Instance type (Ex: M5 large)
* Spot fleet chooses from the best launch pool according to the launch strategy and meet the target capacity
* Launch startegies can be
    * lowestPrice: Launches from the optimal pool which provides the lowest price
    * Diversified: Launches from multiple pools
    * capacityOptimized: Pool with optimal capacity   

## Extra Notes
### Burstable performance instances
* low-to-moderate CPU utilization workloads lead to wastage of CPU cycles and, as a result, you pay for more than you use.
* To overcome this, you can leverage the low-cost burstable general purpose instances are the T instances.
* The T instance family provides a baseline CPU performance with the ability to burst above the baseline at any time for as long as required. 
* This ensures that you pay only for baseline CPU plus any additional burst CPU usage resulting in lower compute costs 
* The EC2 burstable instances consist of T4g, T3a and T3 instance types, and the previous generation T2 instance types.
    * T4G instances provide the best price for performance, and provide you with the lowest cost of all the EC2 instance types. The T4g instance types are powered by Arm-based AWS Graviton2 processors
    * T3a has AMD Processors and T2 has Intel Processors
> https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-performance-instances.html 
### CPU Credits
* Each burstable performance instance continuously earns credits when it stays below the CPU baseline, and continuously spends credits when it bursts above the baseline.
    * If the CPU utilization is equal to baseline, then credits earned are equal to credits spent.
```
When the credits earned are greater than credits spent, then the difference is called accrued credits, which can be used later to burst above baseline CPU utilization. Similarly, when the credits spent are more than credits earned, then the instance behavior depends on the credit configuration mode—Standard mode or Unlimited mode.

In Standard mode, when credits spent are more than credits earned, the instance uses the accrued credits to burst above baseline CPU utilization. If there are no accrued credits remaining, then the instance gradually comes down to baseline CPU utilization and cannot burst above baseline until it accrues more credits.

In Unlimited mode, if the instance bursts above baseline CPU utilization, then the instance first uses the accrued credits to burst. If there are no accrued credits remaining, then the instance spends surplus credits to burst. When its CPU utilization falls below the baseline, it uses the CPU credits that it earns to pay down the surplus credits that it spent earlier. The ability to earn CPU credits to pay down surplus credits enables Amazon EC2 to average the CPU utilization of an instance over a 24-hour period. If the average CPU usage over a 24-hour period exceeds the baseline, the instance is billed for the additional usage at a flat additional rate per vCPU-hour.
```
> https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-credits-baseline-concepts.html