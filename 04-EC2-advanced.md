## IP

- Two types of IP - IPv4 and IPv6
- We have public IP and private IP
- Machines with Public IP can be accessed on the worldwide web (www) and machines with private IP can only be accessed over the private network
- Private IPs in a private network have a specified IP range
- Two machines can communicate with each other using the Public IP and machines with Private IP can talk to each other in the private network
- Machines inside the private network can talk to the WWW using a NAT device and an Internet Gateway
- Multiple companies can have the same set of private IP addresses

## Elastic IP

- EC2 instances change their public IP once they are restarted
- Elastic IPs do not change and have a fixed IP and can be attached only to one instance at a time
- Elastic IPs can be created and mapped to an EC2 instance. Incase of an unhealthy instance, it can be instantly mapped to another EC2 instance but this architecture is rarely used and not preferred. Instead, ELB is used to mask failure of EC2 instances
- One account can have a total of 5 Elastic IPs. (can ask AWS to increase limit if needed)
- elastic IPs are billed when they are not connected to an instance
> restarting an EC2 instance means stopping and starting it again which will result in the change of the public IPv4 but rebooting an instance will not result in IP change

## Placement Group

- AWS allows us to decide how the EC2 instances needs to be placed in their hardware in 3 different ways
  - **Cluster**
    - Instances are placed as a cluster in the same hardware, in the same rack, in the same AZ
    - Low-latency communication between instances
    - If hardware fails, all instances are down
  - **Spread**
    - Instances spread across independent hardware within an AZ
    - Limit to 7 instances per AZ per placement group. So spans across multiple AZ
    - High Availability. If one hardware fails, does not affect other instances
  - **Partition**
    - Instances are spread across multiple partitions/racks with in an AZ
    - If one rack fails, instances in other racks are not affected
    - Limits to 7 partitions per AZ per placement group. So spans across multiple AZs
    - Instances is one rack do not share the rack with instances in other partitions
    - EC2 instances get access to their partition information from their metadata

## Elastic Network Interface (ENI)

- It is a component that makes EC2 instances access the internet
- An ENI is created by default when you create an EC2 instance. However, you can create an ENI seperately and attach it to an EC2 instance
- One EC2 instance can have multiple ENIs attached to it
- An ENI comes with:
  - A Public IPv4
  - a private IPv4 
  - One Elastic IPv4 per private IPv4
  - One or more Security groups (SG) can also be attached to ENIs
- Secondary ENIs come with only private IPv4s not public IPv4s. But you can attach an Elastic IP to an ENI to get a public IP for the ENI
- You can use a secondary ENI as a failover
- ENIs are bound to a specific AZ. An ENI created in one AZ can be attached to an EC2 instance running only in that specific AZ
- You are not billed for ENIs
- UseCase (unclear): When you have an EC2 instance that is exposed to the internet by the public IP and you access the EC2 instance privately from the AWS network using private IP and you need another Private IP for to access the same EC2 instance. You can attach a secondary ENI to the instance which will provide you with the private IP. If an Elastic IP is attached to the secondary ENI, you will also get the elastic IP as the public IP for the secondary ENI

## EC2 Hibernate

- When you normally restart an instance, the OS boots from scratch
- But when Hibernating, the state of the RAM is preserved and is restored when the instance becomes alive again thus reducing boot time
- Internally, what happens is, the RAM is actually transferred to the root EBS volumes before the instance goes to the 'terminated' state and is restored back once the instance is started again (end of hibernation)
- For this to happen, the EBS volume must be larger than the size of the RAM and the EBS volume **must be encrypted**.
- Hibernation cannot take place more than 60 days and RAM size must be less than 150 GB (variable)

## EC2 Nitro

- EC2 Nitro is a label for EC2 instances that use new virtualization technology
- EC2 Nitro = Better Performance, Better Security, Better Network
- EC2 nitro instances offer high IOPS EBS Volumes
  - Upto 64,000 IOPS for nitro instances whereas only 32,000 IOPS for non-nitro EC2 instances

## vCPU

- Each EC2 instance that is launched is chose as per needs according to the number of vCPUs and RAM
- The EC2 instance has certain number of Cores (aka CPU Cores)
- These cores contains threads. The threads here are referred as vCPUs
- For each EC2 instance type, No. of cores and no. of threads per cores are defined
- This is useful for optimization options.
  - Suppose you choose an instance type that has the RAM you need but also comes with extra vCPUs that really is not of any use to you. You can reduce the **number of cores** and reduce the licensing costs
  - Similarly, if you need only a specific **number of threads** per core, you can also increase/decrease it as per your needs. Reducing no. of threads per core maybe useful for High Performance Computing (HPC)
- Both of the vCPU tweaking options must be specified during instance launch

## Capacity Reservations

- You can reserve capacity for EC2 instances in a specific AZ
- No long term commitment (1 or 3 years). You get billed as soon as you reserve capacity
