## EBS
* Elastic Block Store. A  network drive that can be attached to EC2 instances
    * Since, it is a network drive, there might be a bit of latency as it uses network to communicate wih the instance 
* Bound to a specific AZ. EBS volumes in one AZ cannot be attached to EC2 instances in another AZ
* However, with the help of an EBS snapshot, EBS volumes can be used in other AZs
* Delete on Termination Attribute
    * There is an option while creating an EBS volume to persist data even after termination of the EC2 instance
    * This can be useful when you create a new EC2 instance but attach the old EBS volume to have the old data back
    * Delete on Termination is checked by default for root EBS volumes. ie- root EBS volumes are deleted on termination 
    * Whereas, non-root volumes are not deleted on termination, by default
    * Root volumes loose their **D**elete **O**n **T**ermination attribute when they are detached (DOT becomes 'No')
* One EC2 instance can have multiple EBS volumes attached to it
* But, one EBS volume can be attached to only one instance at a time
    * However, there are multi-attach feature for some EBS volumes
* You provision capacity for EBS volumes in GB and IOPS
    * You are billed for the provisioned capacity
* An EBS volume not necessarily needs to be attached to an EC2 instance
> A root EBS volume is the EBS volume that is created by default when launching a new EC2 instance

## EBS Snapshots
* Snapshots are nothing but backups
* You can take an EBS snapshot any point in time
* EBS snapshots can be moved across AZs and Regions from which EBS volumes can be easily restored
* It is not necessary to detach the EBS volumes before taking snapshots but, is recommended
    * You can only detach a root EBS volume when the instance is stopped. Force-detach also wont work
    * You can only force-detach a non-root EBS volumes while the EC2 instance is running, but not preferred

### EBS Snapshot Features
* Archive Tier
    * EBS Snapshots that are not used often can be moved to the archive tier
    * This tier has better cost savings (75% cheaper)
    * Takes about 24 to 72 hours for retrieving the snapshots
* Recycle Bin
    * Policies can be created to retain snapshots after accidental deletion
    * Snapshots can be retained anywhere from 1 day to 1 year
    * Snapshots are lost after the retention period
> **Recycle Bin is to retain deleted snapshots, not for retaining deleted EBS volumes**
## AMI
* Amazon Machine Image
* AMI is a way to customize our EC2 instances
    * You can add your own software, configuration, OS...
    * Results in faster boot time because all the software is pre packaged
* AMIs are region-scoped but can be copied across regions

* EC2 instances can be launched from AMIs:
    * Public AMI: Provided by AWS
    * Your own AMI: Created and managed by you
    * AMI from AWS Marketplace: AMIs created by others (that are sold)
        * There are actually dedicated businesses that create and sell AMIs

### Steps to create an AMI
* Launch an EC2 instance
* Customize it
* Stop the EC2 instance
* Build an AMI
    * Buliding an AMI will result in creation of EBS snapshots BTS
* Launch EC2 instance from the newly created AMI (faster boot time)

> Recycle bin option is available to recover accidental deletion of AMIs
## EC2 Instance Store
* **High performance** physical drive that is attached to the hardware that EC2 instance runs on
    * Provides better IOPS compared to EBS volumes
* But, if the EC2 instance is stopped, the storage is also lost. Hence, called an _ephemeral_ storage device
* Can be used for buffer/cache/temporary storage
* If the hardware fails, the data is lost, so replication and backup is the User's responsibility

## EBS Volume Types
* EBS volumes of 6 types: gp2/gp3, io1/io2, sc-1, st-1
* EBS volumes can be categorized on Storage Size, IOPS and Throughput
* Only gp2/gp3 and io1/io2 can be used as boot volumes

* General Purpose SSD
    * gp2 / gp3
    * Balance of price and performance
    * Cost-effective, low-latency
    * Used for boot volumes, development and environments, virtual dekstops..
    * Ranges from 1GB - 16TB
    * gp2
        * IOPS and Storage are linked (3GB per IOPS). Cannot increase seperately
        * Max of 16,000 IOPS 
    * gp3
        * Can increase IOPS and Thorughput independently from storage
        * Max of 16,000 IOPS and throughput of 1000 MB/s

    
* Provisioned IOPS (PIOPS)
    * io1 / io2
    * High performance SSDs
        * A max of 64,000 IOPS for EC2 instances with Nitro and max of 32,000 IOPS for non-nitro
    * Ranges from 4GB - 16TB
    * Can increase PIOPS and Storage seperately
    * io2 is used more as it durable and almost offered at the price of io1
    * Supports EBS Multi-Attach feature
    * IOPS Block Expressn (4GB - 64TB)
        * Sub-millisecond latency
        * Up to 256,000 IOPS with the ratio of 1000 IOPS per GB
    * Used for mission-critical business workloads, databases, high throughput workloads or any other workload that required greater than 16,000 IOPS

* HDDs
    * Ranges from 125GB - 16TB (storage)
    * st-1 (thorughput optimized)
        * Low cost
        * For frequently accessed data, and throughput-intensive workloads
        * Max of 500 IOPS and 500MB/s throughput
    * sc-1
        * Lowest cost
        * For infrequently accessed data 
        * Max of 250 IOPS and 250MB/s throughput

## EBS Multi-Attach Feature
* io1/io2 type of EBS volumes can be attached to multiple EC2 instances at the same time
* Bound to specific AZ. Can be attached to only EC2 instances running in the same AZ
* All instances have read/write permission
* Use cases:
    * Applications that require high availability in a clustered Linux application
    * Applications must manage concurrent write operations
* Must use a file system that is cluster-aware

## EBS Encryption
* When you **create** an encrypted EBS volume, the following happens:
    * Data at rest is encrypted
    * The data in-flight will be encrypted
    * Snapshots of the volume will be encrypted
    * Volumes created from the snapshot will also be encrypted
* EBS Encryption leverages KMS keys (AES-256)
* Encryption will not have much effect on latency
* Volumes created from encrypted snapshot will also be encrypted
### Steps to encrypt an unencrypted EBS volume
* Take a snapshot of the EBS volume
* Create an EBS voume from the snapshot. During this, you'll have the option to encrypt the new EBS volume
OR
* Take a snapshot of the EBS volume
* Copy the snapshot. You'll have the option to encrypt the snapshot when copying it
* Restore EBS volume from the encrypted snapshot. So, the resulting volume will be encrypted

# EFS
* Elastic File System. A managed Network File System (NFS)
* 1000s of concurrent NFS clients; High throughput
* Can be mounted to multiple EC2 instances (in different AZ) at the same time
* Highly available, scalable, pay-per-use (per GB); Expensive (3x gp2 price)
* Don't need to provision capacity in-advance. Scales on the fly
* Security groups are set up to control access to EFS
* EFS is compatible only with Linux AMI
* EFS uses POSIX based file system (~LINUX)
* Can be encrypted at rest using KMS
* Automatic Backups available (Additional Pricing)

### EFS Performance
Performance Mode
* General purpose (default): latency-sensitive use cases like webserver, CMS...
* Max I/O: High latency, thoroughput, highly parallel workloads like big data

Throughput Mode
* Burst Mode: File system throughput is proportional to the file system size
* Provisioned: Ability to set throughput regardless of storage size 

### EFS IA
* Storage tier for less frequently accessed files
* Less price compared to default EFS storage. But cost to retrieve files.
* Can set up a lifecycle policy to move files to this tier. ie- for example, if a file is not accessed for 60 days, it moves to EFS IA

### Deployment
* Multi-AZ deployment; Used for production 
* One Zone EFS is also available. Usually for development. 
    * 90% Cost savings
    * Similarly, one Zone IA also can be used

> When you create an EC2 instance that uses EFS, new Security groups are going to be created and attached to EFS referencing the EC2's security group allowing traffic only from that particular EC2 instance

## EBS vs EFS
### EBS
* Can be attached only to one instance at a time (except io1/io2)
* Bound to specific AZ
* Storage capacity must be privisioned in advance
* General Purposed disks are cost optimized
### EFS
* Can be mounted to multiple EC2 instance at the same time
* Can be attached to EC2 instances in multiple AZs
* Pay-per-use; No need for provisioing capacity in advance
* 3x epensive than EBS gp2s 
* Runs only on Linux because it uses POSIX file system

___
> ### Throughput
> Amount of data that can be transferred in unit time
> ### Latency
> Time taken for the data to be transferred
> ### Bandwidth
> The maximum amount of data that can be transferred at once