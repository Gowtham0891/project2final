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
| Asset name | Brief description | AWS size eg. t3.micro (if applicable, not all assets will have a size) | Number of nodes/replicas or just how many of a particular asset | Identify if this asset is deployed to DR, replicated, created in multiple locations or just stored elsewhere |


Ubuntu-Web - Web Front end t3.micro three EC2 instances per region for a total of six EC2 instances Three stored in us-east-2 as primary and three in us-west-1 as secondary but consumed by the LB (multi region)

EKS cluster in each region. - Currently used to run Prometheus/Grafana deployment. t3.medium Each EKS cluster containing at least two Nodes and deployed using multi AZ VPCs.

RDS (Database) Cluster - to store data for web front end / appliation db.t2.small Primary cluster in us-east-2 with a primary(writer) and secondary(read) node Replicated node in us-west-1 with a primary(writer) and secondary(read) node Multi region and Database replication used - Primary/secondary ++ Regional/Replica configuration.

Load balancer - used to provide DNS and VIP for front end services for load balance and failover. One in each region consuming the VPC for the instances.

S3 bucket - used to store terraform code and possible RDS backups Deployed to us-east-2 and us-west-1

Security keys for remote access to the EC2 instances. Terraform code and its details, where it is stored, its backup, how its deployed, when it was last deployed, and its last config change. Security groups, IAM policies, EKS policies, RDS policies, RDS backups, EC2 snapshots etc... all not part of the training but needs to be included and documented.

Billing/cost/utilization per region is an asset to be documented but not part of this project...


### Descriptions
More detailed descriptions of each asset identified above.

## DR Plan
### Pre-Steps:
The training slides lists 'Pre-steps' as 'Ensure both sides are configured the same and use IaC' Based on this and staying high level, we would consume the terraform code we created for region 1 and deploy it to region2. We did this via our zone1 and zone2 terraform folders. Consolidating the code into a larger module instead of attempting to maintain two seperate 'zone' files will be more resiliant again manual/human error. Terraform will also validate what has changed within the deployment since last deployment and allow for minor changes without needing to redeploy the entire infra or accessing the AWS GUI.
## Steps:
1 - Failover the database. The database needs to be up and online in the other region before any other services can start. 2 - Failover the Application/EC2 instance (if not already provided by your Load balancer configuration.) 3 - Failover the load balancer - the text says to do this first but if the database and EC2 instances are not avaliable, the LB will blackhole traffic or cause traffic/usage on your devices attempting to promote to primary. 4 - Failover anything running in the EKS cluster 5 - Use monitoring tools (your EKS grafana or cloudwatch) to see if traffic is now flowing to your new primary region.
