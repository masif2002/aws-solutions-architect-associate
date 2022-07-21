## Scalability and High Availability
* A system is said to be scalable if it can handle greater loads by adapting
### Vertical Scaling
* Increasing the size of the instances (ie- increase in RAM and vCPUs )
* Ex: from t2.micro to t3.2xlarge (doesn't have to be the same instance family)
* In vertical scaling, you scale up/down
* Vertical scaling usually happens in databases, to handle high workloads as ypur application grows
### Horizontal Scaling (aka Elasticity)
* Increasing the no. of instances
* In horizontal scaling, you scale out/in
* Ex: ASG scaling out EC2 instances to match workload for your web application
### High Availability
* An application is said to be highly available if it is running on multiple AZs
* The goal of high availability is to survive a data centre loss

## Elasting Load Balancing (ELB)
* Load balancers are servers that balance the load (or traffic) it receives, among the backend EC2 instances (or servers)
    * It exposes as a single point of access (DNS) for the users
    * It allows to seamlessly handle failure of instances 
    * It performs health checks. It only forwards traffic to healthy EC2 instances  
    * Can enforce stickiness with cookies
* You can also set up your own Load balancer in AWS which doesn't cost much but it you need to put more effort
* An Elastic Load Balancer is an AWS managed Load balancer. So, AWS takes care of the upgrades and maintenance. And it is integrated with a ton of AWS services
* ELB performs health checks using specific **protocols** (HTTP,TCP..) on specific **ports** (80, 443..) and **route** (/login, /home..) expecting a response (200 status code). If the EC2 instance is able to reply, then it is considered as a healthy instance
* There are 4 types of ELBs
    * Classic Load Balancer (old generation)
        * Supports HTTP, HTTPS, TCP, SSL (secure TCP) Protocols
    * Application Load Balancer (new gen)
        * Supports HTTP, HTTPS, WebSocket Protocols
    * Network Load Balancer (new gen)   
        * Supports TCP, TLS (Secure TCP), UDP 
    * Gateway Load Balancer
        * Operates on Network Layer (Layer 3). Uses IP
* It is also possible to set up ELBs for internal and external use 
* The EC2 instances running behind the ELB only receive traffic from ELB. 
    * So the SGs of EC2 instances is configured referencing the SG of ELB to only allow traffic from it

## Classic Load Balancer
* Supports TCP (Layer 4) and HTTP, HTTPS (Layer 7)
* Uses TCP or HTTP based health checks
* Provides fixed hostname

## Application Load Balancer (ALB)
* Runs on Layer 7 (HTTP)
* Support for HTTP/2 and WebSocket
* Supports redirects (ex: HTTP -> HTTPS)
* Provides DNS hostname
* Suitable for microservices and container based applications (like Docker & Amazon ECS)
    * ALB provides a port mapping feature to redirect to a dynamic port in ECS
<br/><br/>
* You can use an Application Load Balancer:
    * as a load balancer for multiple applications running across multiple target-groups
    * as a load balancer for multiple applications/containers in the same machine
> You need to create multiple LBs for the same application if you use CLB

<br/><br/>
* The ALB can route traffic to a target-group based on:
    * routes (/home, /login..)
    * hostnames (asif.techin48.com, ash.techin48.com..)
    * Query strings, headers(tech48.com/signin?platform=mobile)
    * ALB balances the load among instances in the specific target-group
<br/><br/>
* Target group is a group of instances to which the ALB can route the traffic to 
    * Target groups can run an application regardless of what other target groups are running
    * A target-group can be a group of EC2 instances, ECS tasks, Lambda functions, IP addresses (private IPs)
<br/><br/>
* Health checks in ALB happens at target-group level
<br/><br/>
* The backend servers (EC2 instances) running behind the ALB don't have access to the client IP. But they can be found in the headers of the HTTP request
    * `X-forwarded-for` contains client IP
    * `X-forwarded-port` contains the port
    * `X-forwarded-proto` contains the protocol

## Network Load Balancer
* Layer 4 load balancer 
* Handles millions of request per second
* Low-latency (ALB has slightlu higher latency)
* Provides the feature to have a static IP per AZ (Elastic IP)
    * Helpful for whitelisting IPs

</br></br>
* A target group in an NLB can consist of   
    * EC2 instances
    * IP address
        * Must be static private IP; Useful when using servers in own data centre
    * Application Load Balancers
        * You can chain NLBs with ALBs to leverage the static IP feature of NLB along with HTTP functionalities from ALB

## Gateway Load Balancer 
* Layer 3 Load Balancer
* This is actually used to host 3rd party virtual appliances that filters all of your network traffic and checks for intruders before the traffic enters your application
* THis basically consists of two components:
    * Transparent Network Gateway: The gateway that acts a point of entr/exit for all the traffic
    * Load Balancer: Distributes traffic across 3rd party appliances (not the actual application)
</br></br>
* Target group can consist of EC2 instances and private IPs
* GLB uses *GENEVE* protocol on port 6081
![](img/GLB.png)  


___ 
> Layer 3: Network Layer; Layer 4: TCP; Layer 7: HTTP & HTTPS