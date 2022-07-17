## AWS GLobal Infrastructure
* AWS Regions
* AWS Availability Zones (AZ)
* AWS Points of Presence / Edge Location

## AWS Regions
* Cluster of data centres
* Most AWS services are region scoped
* Region names, ex: us-east-1, eu-west-1, ..
* Choosing an AWS Region to launch your application depends on certain factors
    * Compliance
        * The data must be compliant to the government rules
    * Cost
        * Each region has different pricing on different services
    * Service Availability
        * Cerain AWS services are not available in every region
    * Latency
        * Choosing a region close to your users provides lower latency
## AWS Availability Zone
* Each AZ is one or more discrete data centres isolated from each other
* Each Region consists a minimum of 2 AZs, usually 3, and maximum of 6 AZs
* The AZs are connected with hgh-bandwith, ultra low-latency networking
* AZs are named such as: us-east-1a, us-east-1b, us-east-1c..

## AWS Edge Locations
* Used for caching contents at the edge locations for faster content delivery for users