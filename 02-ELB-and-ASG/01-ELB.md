> Refer Scalability, High Availability, Load Balancer 
* Target Group perform the healthchecks
* The load balancer will route traffic only to targets in the selected Availability Zones.  
* When using an Application Load Balancer to distribute traffic to your EC2 instances, the IP address you'll receive requests from will be the ALB's private IP addresses. To get the client's IP address, ALB adds an additional header called "X-Forwarded-For" contains the client's IP address.
* Lifecycle Hooks are something that you attach to ASG. When ASG scales out, the new instances go to a new state after initialising before the running state. In that state certain checks are performed as defined by you. And similar thing happens when ASG scales in/
* Network Load Balancer has one static IP address per AZ and you can attach an Elastic IP address to it. Application Load Balancers and Classic Load Balancers have a static DNS name.
* The following cookie names are reserved by the ELB (AWSALB, AWSALBAPP, AWSALBTG).
* The Auto Scaling Group can't go over the maximum capacity (you configured) during scale-out events.
* For each Auto Scaling Group, there's a Cooldown Period after each scaling activity. In this period, the ASG doesn't launch or terminate EC2 instances. This gives time to metrics to stabilize. The default value for the Cooldown Period is 300 seconds (5 minutes). Similarly, there is Draining period or Deregistartion period
* ELB and EC2 bith have health checks. When you create an ELB, you assign a target group to the ELB. The target group is hwat actually performs the health checks behindthe scenes