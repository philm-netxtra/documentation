### 1. Create new EC2 public instance on AWS from OPSWAT MetaDefender AMI.
1. Search for `OPSWAT MetaDefender` and choose the AMI OPSWAT MetaDefender Windows in AWS Marketplace
2. Choose Instance type as `m5.xlarge`
3. Configure Instance Details
   * Subnet: choose any Public subnet
   * Auto-assign Public IP: Enable
   * Another setting set as default.
4. Add Storage
   * Set 150Gb for /root  device
   * Choose General Purpose SSD
5. Add tags if you need
6. Configure Security Group
   * Create new group
   * Add new rule: All traffic, CIDR 0.0.0.0/0 - because we will need to connect to this instance by RDP one time, but this AMI uses not standard unknown port and we need accept all connections.
7. Review and Launch instance
8. Create and keep a new AWS key for this instance.

### 2. Connect to this new instance by RDP
1. Go to EC2 Dashboard, Running Instances
2. Find and choose your new instance.
3. Press Connect button on Dashboard, press Get Password and choose your AWS key for this instance, decrypt password and download the Remote Desktop file
4. Open any RDP desktop application and import this Remote Desktop file and connect to the instance using the decrypted password above
5. After connection it will open the desktop for Windows Server 2012R2. Press Start menu -> Administrative Tools -> Computer Management
6. In opened window choose Disk Management in the list. Choose the Disk 0 drive, select the Drive C:/ volume part and click the right mouse button -> Extend Volume. Select the needed options to extend and use the full space, then start the process.
7. Now we can disconnect from RDP.

### 3. Configure the OPSWAT Malware Server.
1. Go to EC2 Dashboard, Running Instances
2. Find and choose your new instance.
3. Find and copy the Public DNS(IPv4) address (e.g. ec2-52-215-36-156.eu-west-1.compute.amazonaws.com)
4. Open any browser and go to this address in addition with 8008 port at the end(e.g. ec2-52-215-36-156.eu-west-1.compute.amazonaws.com:8008).
5. On opened page press Login button in the right top corner.
6. Login with credentials - login: admin, pass: admin
7. It will open the Metadefender Dashboard. Choose the Settings menu -> Password
8. Set the new password for Admin.
9. Choose Settings -> User Management. Create new user for yourself.
10. Go to Settings -> License. Press Activate. Choose Online mode, Single-Node deployment, input activation key.
11. After the license been activated the system will start the malware engines installation. You can track this process on Inventory -> Engines. You have to wait until all engines will be installed.
12. Check that malware scanner works fine. Go to Process page. Upload any files for tests. 
13. Close the browser.

### 4. Create new AMI from this instance 
1. Go to EC2 service -> ELASTIC BLOCK STORE -> Volumes.
2. Find in the list the volume that attached to your instance by instance-id
3. Create a snapshot.
4. Go to  ELASTIC BLOCK STORE -> Snapshots. After the snapshot will be ready choose it in the list and press Actions - Create Image.
  * Input the image name
  * Set the Virtualization type: Hardware Assisted Virtualization
  * Set another options as default
  * Press create

### 5. Create new private instance from our AMI created above
1. Go to EC2 Service -> Images and choose our image that created before and press Luanch
2. Choose Instance type as `m4.xlarge`
3. Configure Instance Details
   * Subnet: choose any Private subnet
   * Auto-assign Public IP: disable
   * Enable termination protection
   * Enable CloudWatch detailed monitoring
   * Another setting set as default.
4. Add Storage
   * Set 150Gb for /root  device
   * Choose General Purpose SSD
5. Add tags if you need
6. Configure Security Group
   * Create new group or choose the existed one (e.g. OPEN-SG, Security group for open communication)
   * Add new rule if needs: All TCP, Source 0.0.0.0/0
7. Review and Launch instance
8. Create and keep a new AWS key for this instance.
9. Name it as `OPSWAT Metadefender Core Win Server`

### 6. Configure infrastructure for instance
1. Go to EC2 -> Load Balancing -> Target groups
2. Choose Create target group
  * Name: opswat-core-management
  * Protocol: HTTP
  * Port: 8008
  * Target type: Instance
  * VPC: OSO
3. Choose this new opswat-core-management target group in the list, and in the Details choose Targets, Edit
4. In the instances list choose your new private instance `OPSWAT Metadefender Core Win Server` and press 
`Add to registered` with port 8008.
5. Press save
6. Choose Services -> Networking & Content Delivery -> Route 53
7. Choose Hosted Zones
8. Find your main domain address (e.g. os-test.me) and `Create Record Set`
  * Name  - opswat 
  * Alias - yes
  * Alias target  - choose existed load balancer group for your staging environment (e.g. oso-api-alb-staging)
  * Save record set
9. Go back to hosted zones menu. And press `Create Hosted Zone` 
  * Name - opswat.vpc
  * Type - Private
  * Create
10. Create four Record sets for this hosted zone with names below.
  * Names: opswat.vpc, staging.opswat.vpc, sandbox.opswat.vpc, production.opswat.vpc
  * Alias target  - choose existed load balancer group for your staging environment (e.g. oso-api-alb-staging)
  * Save record set
11. Go to EC2 -> Load Balancing -> Load Balancers, and choose your ELB group for staging (e.g. oso-api-alb-staging)
12. In the details for ELB group choose tab `Listners`, and choose port 443. Press `Edit/View rules`. Add new rule
  * IF Host is `opswat.os-test.me` THEN Forward to `opswat-core-management`
  * Save
13. In the details for ELB group choose tab `Listners`, and choose port 80. Press `Edit/View rules`. Add new rule
  * IF Host is `*.opswat.vpc` THEN Forward to `opswat-core-management`
  * Save
14. Then in description to this ELB group find the Security group or Go to `Network & Security -> Security Groups` and find the SG for oso-api-alb-staging and choose it
15. In Inbound tab needs to create one new Item.
  * Custom TCP 
  * port 8008
  * Source 0.0.0.0/0
  * Save

### Done
***
Now you can specify this server in environment variables for private server as *.opswat.vpc (e.g. staging.opswat.vpc). Also you can get access to UI Dashboard on the main domain address that we specified (e.g. https://opswat.os-test.me)

***

In order to build the API integration, please follow the API integration steps available on our online documentation (https://onlinehelp.opswat.com/corev4/7._Metadefender_Core_Developer_Guide.html)