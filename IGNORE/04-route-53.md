* You cant set TTL in an Alias Record
* An Alias Record maps host name to an AWS resource whereas in CNAME type of record, you map one hostname to another hostname
* TTL - Time To Live
* Route 53 has health checks except Simple ROuting Policy

### Route53 vs ELB
```
Both Route53 and ELB are used to distribute the network traffic. These AWS services appear similar but there are minor differences between them.

ELB distributes traffic among Multiple Availability Zone but not to multiple Regions. Route53 can distribute traffic among multiple Regions. In short, ELBs are intended to load balance across EC2 instances in a single region whereas DNS load-balancing (Route53) is intended to help balance traffic across regions.
```
* Read [More](https://stackoverflow.com/questions/57321793/elastic-load-balancer-elb-and-route-53-in-aws#:~:text=Route53%20can%20distribute%20traffic%20among,traffic%20to%20only%20healthy%20resources.).
