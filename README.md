# AWS_solution-architect
**REVIEW YOUR PERMISSIONS**
To check user have that permisstion to do changes in the aws use -> IAM Policy Simulator: https://policysim.aws.amazon.com/
**Review the security configuration of Amazon EC2 instances**
SECURITY GROUP:Security groups act at the instance level, not the subnet level. By default, no inbound traffic is allowed & all outbound traffic is allowed.
# Create your KMS master key
A KMS master key enables you to easily encrypt your data across AWS services and within your own applications.
the top of the page, in the unified search bar, search for and choose Key Management Service->Key Management Service page , choose Create a key->On the Configure key page, select Symmetric-> Next -> Define key administrative permissions page, select the user or role you’re signed into the Console with. The user is displayed at the top of the page-> key adminitrator:users or roles that manage access to the encryption key->select Key users->
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/2aab470e-c315-4c68-b9d2-cb52c87bb524)
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/fc3fa35e-15f3-4ef7-bbc6-ad484d045383)
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/7d49abe0-c2d9-4d65-a86a-fe67a72e78d0)
only using public ip we can access the aws in cmd.private ip will not change if we stop and start intance.
if we don't want our public ipv4 need to change we can make use of elastic ips: in the elastic ip-> allocate elastic ip-> create ipv4.
open elastic ip ->action-> associate elastic ip address->select instance.

# placement groups
to create placemnt group-> network & security->placment group-> create placment group->name->select placmnet strategy->select sprade level->and create 
while creating new instance in advance setting.

# EC2 Hibernate
while creating an instance in advance setting we can enable hibernate behaviour->ebs volume must be encrypte.
uptime-> we can get the information about how many mints inntance is up in cmd.
if we add hibernate to the instance and start and stop the instance it will calculate the instance time by adding all.

# LB sticky 
if we enable the elastic load balance for a user it will navigate to the particular user to particlar instance.
# elastic load balancer -SSL certificate
ssl(secured socket layer: used to encrypt connections) certification allows trafic between your clients and load balancer to be encrypted.
TLS(transport layer security). public ssl certificate is issued by certificate authority and this sslis attched to load balancer to encrypt connection between client and load balancer.
SSl certificate have expiry date you set and we need to renewed.
SNI(server name indication):sni solve the problm of loading multiple ssl certificates.works only for ALB & NLB.
How to enable SSl certificate:select load balancer->add listner->under security listner setting-> select security policy->select where this certificates are resids(we can also import the certificates)-> and done.

# AWS s3
if we upload same object/image file with same name and if we desabled the version it will override the existing one.
objects in Amazon S3 are private by default.

# Ec2 instance connect
Session Manager enables you to connect to the bastion host instance without the need for specific ports to be open on your firewall or Amazon Virtual Private Cloud (Amazon VPC) security group
command to list all of your S3 buckets: aws s3 ls
command to list all objects in your buckets: aws s3 ls s3://<bucketname>
copy a file to the S3 bucket: aws s3 cp report-test1.txt s3://<bucketname>
**error**:upload failed. This is because we have read-only rights to the bucket and do not have the permissions to perform the PutObject operation we need to chenge bucket policy.
EC2InstanceProfileRole: This is the Role that the EC2 instance uses to connect to S3, note arn for future use.
so in the bucket policy allow this role to get and put objects, so that we can add objects to s3 bucket through ec2 connect.
A blank Bucket policy editor is displayed. Bucket policies can be created manually, or they can be created with the assistance of the AWS Policy generator: https://awspolicygen.s3.amazonaws.com/policygen.html.
Principal: paste the EC2InstanceProfileRole ARN that you copied to a text file
An Amazon Resource Name (ARN) is a standard way to refer to resources within AWS. In this case, the ARN is referring to your S3 bucket. Adding /*.
In order to access a previous version of the object, you need to update your bucket policy to include the “s3:GetObjectVersion” permission. 

# aws versioning
versiion will take a files with the same name and also used if we have any accidental delete of the objects.
Notice that the sample-file.txt object is displayed again, but the most recent version is a Delete marker. parmanently delete version of the sample-file.txt object with the Delete marker it will recover our files back.
Note:  When deleting a specific version of an object no delete marker is created


Connect to database using end point,
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/74e99a11-0187-4617-80c7-d73132692f33)

# Amazon Virtual Private Cloud (VPC)
 Your VPCs->Create VPC->VPC Only-> give tag name->IPv4 CIDR block: 10.0.0.0/16-> create vpc.
# Create Your Public Subnet
Subnets->VPC ID: My VPC->Subnet name->Availability Zone->IPv4 subnet CIDR block->Create subnet->then select created subnet->action->edit subnet setting->Enable auto-assign public IPv4 address.
Enable auto-assign public IPv4 address provides a public IPv4 address for all instances launched into the selected subnet.
Even though your subnet is labeled Public 1, it is not yet a public subnet. A public subnet must have an Internet Gateway, 
#  Create an Internet Gateway
Internet gateways->Create internet gateway->select created IG ->actions->Attach to VPC->Attach internet gateway.
This attaches the Internet gateway to your VPC. Even though you created an Internet gateway and attached it to your VPC, you still have to tell instances within your public subnet how to get to the Internet.
# Create a Route Table, Add Routes, And Associate Public Subnets
To use an Internet gateway, your subnet’s route table must contain a route that directs Internet-bound traffic to the Internet gateway. You can scope the route to all destinations not explicitly known to the route table (0.0.0.0/0 for IPv4 or ::/0 for IPv6), or you can scope the route to a narrower range of IP addresses; for example, the public IPv4 addresses of your company’s public endpoints outside of AWS, or the Elastic IP addresses of other Amazon EC2 instances outside your VPC. If your subnet is associated with a route table that has a route to an Internet gateway, it’s known as a public subnet.
There is currently one default route table associated with the VPC, My VPC. This routes traffic locally. Create an additional Route Table to route public traffic to your Internet Gateway.

Route tables->Create route table->Under Route table settings->VPC select vpc->create.

Notice that there is one route in your route table that allows traffic within the 10.0.0.0/16 network to flow within the network, but it does not route traffic outside of the network. Add a new route to enable public traffic
Edit routes->Add route->Destination:0.0.0.0/0->Target: Select Internet Gateway->save.
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/b97408c9-9058-41df-b31f-59aceada699c)
Choose the Subnet associations->Edit subnet associations->select publick subnet->save
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/f11ee665-6afc-46ca-a41e-0d2a8431c7f1)
The subnet is now public because it is connected to the Internet via the Internet Gateway.

# Create a Security Group
A security group acts as a virtual firewall for your instance to control inbound and outbound traffic. When you launch an instance in a VPC, you can assign up to five security groups to the instance. Security groups act at the instance level, not the subnet level. Therefore, each instance in a subnet in your VPC could be assigned to a different set of security groups. If you do not specify a particular group at launch time, the instance is automatically assigned to the default security group for the VPC.
Security groups->Security group name->Description->VPC->Under Inbound rules->Add rule->Type: HTTP->Source: Anywhere-Ipv4->Create security group.

# Launch a Web Server in your Public Subnet
AWS Management Console->EC2->Launch instances->Name and tags section give name->Application and OS Images (Amazon Machine Image) Choose Amazon Linux 2 AMI->Key pair (login)->Proceed without a key pair->Network settings->edit->select vpc->Firewall (security groups), choose  Select an existing security group-> Advanced Details->User data-> add some of the pre request file install->Copy the Public IPv4 address ->search in new tab->you need to see as below,
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/72c4b56a-6f61-483c-bac1-b0253382e02b)
You should be able to see this page. Currently, you do not have a database. Once you create your RDS instance, you connect it to your web server.
# Create Private Subnets for your MySQL Server
To deploy your RDS database, your VPC must have at least two subnets. These subnets must be in two different Availability Zones in the AWS Region where you want to deploy your DB instance.
AWS Management Console->VPC->Create subnet->select vpc->give subnet name->select AZ and give-> IPv4 CIDR block->save
1.Create a Security Group for your Database Server
2.Create a Database Subnet Group
 A DB subnet group is a collection of subnets (typically private) that you create in a VPC and that you then designate for your DB instances. Each DB subnet group should have subnets in at least two Availability Zones in a given region. When creating a DB instance in a VPC, you must select a DB subnet group.
 AWS Management Console ->RDS->Subnet groups->Create DB Subnet Group->Name->VPc->Add subnets section->select Availability zones->In the Subnets->select ip.save.
# Create an Amazon RDS Database
 Databases->Create database->Engine options: MySQL->Version: MySQL 5.7.X->Templates section, select Dev/Test->Settings section->DB instance identifier:->Master username: ->Master password: ->DB instance class section->DB instance class: Burstable classes->Select db.t2.micro or db.t3.micro->Storage section->Storage section, de-select  Enable storage autoscaling->Connectivity section->Virtual Private Cloud (VPC)->Public access: No->Existing VPC security groups->Add the Database security group->Monitoring section, de-select  Enable Enhanced monitoring->Additional configuration->De-select  Enable automated backups,De-select  Enable auto minor version upgrade->create.
 # Connect Your Address Book Application to Your Database
 you connect the address book application (in your Public subnet) to your database (in your Private subnet).
 Before you can connect your address book application to your database, you need to know the endpoint of the RDS instance. This is the address of your RDS instance
 mydb instance->Connectivity & security->Endpoint(copy).

 # CONNECT TO YOUR DATABASE
Return to the browser tab that is displaying your web server
Endpoint: Paste your MySQL endpoint
Database: myDB
Username: admin
Password: lab-password
Choose Submit
Once connected, you should see an address book with two entries.
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/7cfb5e08-b06d-4f35-99a7-8d415c479c3f)

The only architectural difference between a public and private subnet is that a public subnet has a route to an internet gateway.
hirarchy:- aws cloud->region-> vpc->subnet->(route table we can manage the traffic)-network ACLS(access control list)->security group-> ec2 isnatnce.


# Elastic Load Balancing
All AWS load balancers are scalable and highly available. The Application Load Balancer has individual nodes running in each Availability Zone that are configured with the Application Load Balancer. Application Load Balancers can be internet-facing or internal; the difference is that internet facing Application Load Balancers will have public IP addresses and internal Application Load Balancers will have private IP addresses.
The Classic Load Balancer, the Application Load Balancer, the Network Load Balancer, and the Gateway Load Balancer, these services make up the family of products known as Elastic Load Balancing (ELB).
Network Load Balancers can allocate static IP addresses, they are easier to integrate with security and firewall products. 



# Deploying Amazon VPC for a 3-Tier Web App
1.Creating a VPC
(to create vpc we need to give vpc name and cidr ipv4 ip range)
2.Creating subnets
create 2 public and 4 public subnets(to create subnets we need to VPC ID,Subnet name,Availability Zone,IPv4 CIDR block)
3.Creating an Internet gateway
(to create IG  required only name and after creation we need to attached to vpc)
4.Creating a NAT gateway
You can use a network address translation (NAT) gateway to enable instances in a private subnet to connect to the internet or other AWS services. The NAT gateway also prevents the internet from initiating a connection with those instances
( to create NAt gateway we need name,subnet,Connectivity type,Elastic IP allocation ID)
if we Choose Allocate Elastic IP->This will generate an Elastic IP address and will allocate it to the NAT gateway.
5.Configuring route tables
(2 route table one if for private subnet and one is for publick subnet)
6.Creating security groups
(Security group name,VPC,Inbound rules)
7.CREATE THE DATABASE subnet IN THE PRIVATE SUBNETS
Create DB subnet group-to rds will be deploye(name,description,vpc,AZ,subnets)
8.Create database
(Choose a database creation method,Engine type,Available version,Templates,DB cluster identifier,Master username,Master password,DB instance class,size,Availability & durability,)
9.create EC2 instance
If you want the application to be highly available in different Availability Zones, it’s a good practice to create a launch template and reuse it for deploying the EC2 instance instead of creating from scratch every time.
create templete and make use of that templet to create mutiple ec2 instance.
10.CREATE AN APPLICATION LOAD BALANCER
(Load balancer name,Network mapping,VPC,Security Groups,Listeners and routing,Target group,Register Targets)









