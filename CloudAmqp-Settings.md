For handling rabbitmq servers we use [Cloudamqp](https://www.cloudamqp.com/)

It is service that fully maintain rabbitmq server and handles all troubles related to it.

We use peering connection to bind 3-d party tool of cloudqmap and aws.
Detailed description is here https://www.cloudamqp.com/blog/2014-11-14-amazon-vpc-peering.html

Most of the setup is already made - we need to:
1. Create new cloudqmap instance with dedicated vpc turned on(there will be a flag in a creation wizard)
2. Dont overlap the subnents - that must be different(you aws vpc and cloud one)
3. Create peering request in aws acc and accept it in cloud acc. Peering requests may be created between aws accounts only. Cloud has its acc - check for details in vpc page of instance you created in cloud(on top).
4. Create a route to CloudAMQP VPC - ip range of subnet you selected in cloud route to peering connection of aws vpc. Aws ui will dislplay options to you - select the one you need.

you dont need to set up DNS resolution since it is already made within our default vps settings

Last think you need to adjust - copy url from details page of CloudAmqp instance to task definitions for all services and then apply new TD for each service
