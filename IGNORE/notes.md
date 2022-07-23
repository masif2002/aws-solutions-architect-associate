## Notes

- In EC2 Reserved Instances, you reserve Instance type, Region, tenancy, OS and in convertible reserved instances, you can change everything except the OS
- In Savings plan, you are lockes to locked to Instance family and Region
- Dedicated host allows to use server-bound software licenses
- Spot instance prices vary based on AZ
- ENIs are bound to AZ
- EBS is bound to AZ
- You provision capacity for EBS volumes in GB and IOPS (and thoughput)
- EC2 instance is chose as per the needs of RAM and vCPU
- Buliding an AMI will result in creation of EBS snapshots BTS
- Only gp2/gp3 and io1/io2 can be used as boot volumes
- In gp2, IOPS and Storage are linked
- EFS can be mounted on EC2 instances in multi-AZ where as EBS Multi Attach can be used only on EC2 instances in the same AZ
- The ALB can route traffic to a target-group based on: routes, hostnames, Query strings, headers
- A target-group in ALB can be a group of EC2 instances, ECS tasks, Lambda functions, IP addresses (private IPs)
- A target-group in NLB can be a group of EC2 instances, IP addresses (private IPs) and ALBs
- Target group in GLB can consist of EC2 instances and private IPs
- SNI works for ALB, NLB and CloudFront. Not for CLB
- To create an ASG, you need to specify launch template for your EC2 instances, Scaling policy, Desired, min, max capacity of EC2 instances
