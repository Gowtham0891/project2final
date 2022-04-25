# Infrastructure
Two S3 buckets created one in us-east-2 and one in us-west-1, Three Web instances of EC2 in each region (us-east-2, us-west-1). two EKS nodes in each cluster deployed in the us-east-2 and in us-west-1. 
Load balancer is delpoyed to distribute traffic and failover's. The RDS deployment will also have 2 nodes in each cluster [us-east-2 and in us-west-1]
and the backup is set to 5 days.
Grafana dashboards to monitor infrastartuture and application performance.


## AWS Zones


command used to find the AZ [Availability Zones]: aws ec2 describe-availability-zones --region us-east-2 
us-east-2a us-east-2b us-east-2c added in terraform code in VPC and in RDS files

aws ec2 describe-availability-zones --region us-west-1
us-west-1b us-west-1a azs = ["us-west-1b", "us-west-1a"] added in terraform code in VPC and in RDS files

## Servers and Clusters

Three Ec2 instances for Web in each Us-east-2 and for us-west-1. each cluster one in us-east-2 and another in us-west-1 will have two nodes each.

RDS Database will have a cluster in us-east-2 and in us-west-1 with two nodes in each cluster.
the cluster will be distributed across AZ provided in terraform files as like above.


### Table 1.1 Summary
| Asset      | Purpose           | Size                                                                   | Qty                                                             | DR                                                                                                           |
|------------|-------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Ec2 instances | Used as a virtual server for configuring security and networking and to manage storage | t3.micro | 3 EC2 instances in each region us-east-2 and us-west-1 | replicate the same in the us-west-1 |
| EKS Cluster | Container platform for managing, deploying and scaling the containerized application | t3.medium | 2 cluster nodes in each region us-east-2 and us-west-1 | replicate the same in the us-west-1 |
| EKS Cluster | Container platform for managing, deploying and scaling the containerized application | t3.medium | 2 cluster nodes in each region us-east-2 and us-west-1 | replicate the same in the us-west-1 |
### Descriptions


S3 bucket - one in us-east-2 and another in us-west-1 used to store terraform code data and RDS data thats been Deployed

Ubuntu-Web - Web Front end EC2 insstances of t3.micro each region will have three EC2 instances one for us-east-2 as primary and three in us-west-1 as secondary.

EKS cluster with two nodes each for the us-east-2 and for us-west-1 used to run Prometheus/Grafana deployment. t3.medium size as per the deployment file.

RDS (Database) Cluster - to store data for web front end / appliation db.t2.small Primary cluster in us-east-2 with a primary(writer) and secondary(read) node Replicated node in us-west-1 with a primary(writer) and secondary(read) node Multi region and Database replication used - Primary/secondary ++ Regional/Replica configuration.

Load balancer - One in each region us-east-2 and for us-west-1 consuming the VPC for the instances for load balancing and for the failover

Key pairs to be created as "Udacity" in us-east-2 and us-west-1 to prove your identity when connecting to an Amazon EC2 instance.





## DR Plan
### Pre-Steps:
Copy the same set of code of terrform in to another folder and make the changes to the regions and to the corresponding availability zones.
Creaste the S3 Bucket and update the same in the config.tf file and the region specific ami in the ec2 file. and deploy the terraform file to replicate the same in the another region.

## Steps:
Create the replication for the instances and for the DB cluster in another region. Ensure the other region is up and runnning.
To check the failover is working you can restart/reboot the instances and the cluster nodes in the primary region and the traffic will automatically redirects to the secondary region this is achieved with the help of the load balancer.

