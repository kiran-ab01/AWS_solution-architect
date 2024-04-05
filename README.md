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

# Gateways in AWS
1.Internet gateway

Internet getway is VPC level
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/dbf6ce1e-4549-4af7-b869-32f4051f1fde)
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/0bad3fdf-a548-44b8-9394-6b5a14946fc9)

2. NAT gateway

Nat gateway is subnet level
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/c0c75c97-6c9a-4548-837d-507f9ff99129)
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/43cabdc3-b3ba-48b0-a11b-ee9fe43ef418)

 VPC peering
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/e4af1800-51dc-4f02-90f3-50ec0ef7c8bf)

3.virtual private getway

by defult any resource created inside the vpc is don't have any cpmmunication to on-promis datacenter/any other, so we need to establish a connection we can make use of virtual private gateway.
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/848fffe7-e346-4f99-a748-a51d79ff6d58)
we can add multiple connection to vpc through virtual private getway up to 10(on primis/application) connection and only one virtual private getway in vpc.
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/224c64b7-1919-4e5e-b63c-0d16ddc40ff0)
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/ae980131-439c-4593-87f2-c81c2934172e)
alternative way is make use of public&private virtual interfaces to connect as mentiond above

4.customer getway
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/2d6e85fd-2159-4eed-89b8-4165801744a1)

5.Direct connect getway
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/02b7fb20-1d2e-4ed8-ac5c-aa6d786327a5)
Mainly used to connect mutiple aws account in diff region.

we can't make use direct connect getway to connect 2 vpc in the 2 diff aws account.

6.Transit gateway

by make use of transit getway we can connect one vpc to other vpc but we can't communicate if the connection is not there.
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/f0e10441-ccd8-4b95-98e6-04ec21b729ab)
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/a268acbe-f515-41d6-af20-144e2f2ad95e)
we can have 20 route table defined in the subnet

7.Local getways

vlan

![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/acf3db48-9e4b-4438-9503-f2ac66bb4a85)

# Virtual Private Cloud (VPC) peering and AWS Transit Gateway
1.CONFIGURE A VPC PEERING CONNECTION BETWEEN VPC1 (OREGON REGION) AND VPC2 (OREGON REGION)

to create vpc peering->vpc->create peering connection->name of the peering->vpc id(requestor)->select my account->region-this region -if vpc is in another region select that region vpc->vpc id(accepter).->accept the peearing connect. create mesh vpc peering from one vpc to other.

Task 2: Configure network traffic routing across a VPC peering connection

Amazon VPC provides multiple ways to route VPC traffic. In this lab, you use a Custom route table, which is a route table that you create

CONFIGURE ROUTING IN each VPCs

In the custom routetable we can add Nat gateway/internet getway to internet access and also we need to add routs to peering connection on that particular vpc.

Destination- 10.20.0.0/16 (This ip is vpc2 CIDR, add here to enable vpc3 private subnet to reach it- through the VPC peering connection).Target- select Peering Connection from the drop-down list.Choose vpc3-vpc2-peering->add

Task 3: Test network connectivity across VPC peering connections

This lab uses AWS Systems Manager to give you access to the EC2 instances through an interactive one-click browser-based shell. This service helps customers manage EC2 instances without the need to open inbound ports like SSH, maintain bastion hosts, or manage SSH keys.

select ec2 instance->connect->sestion manger->it will login to backend of ec2

To test the vpc connection select another ec2 instance and copy the ip address and use ping <isnstance_ip> to check the connection
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/f1c9b7d5-a79e-4aa7-a0a3-631b4a505b63)

Task 5: Remove VPC peering connections

Task 6: Setup AWS Transit Gateway connections

In the VPC peering scenario tasks (Task 1 - Task 5), a full-mesh configuration was required to allow communication between the three VPCs. Which ended up with three VPC peering connections. With AWS Transit Gateway, you can share a Transit Gateway resource with VPCs in the same region. Hence the end configuration uses two Transit Gateway resources instead of three

CREATE A TRANSIT GATEWAY IN EACH REGION

Transit Gateways->name->Amazon side Autonomous System Number (ASN): 64513 (you can choose any range between 64512 - 65534. We recommend using a unique ASN ->Default route table association: Uncheck->Default route table propagation: Uncheck-> create.
create required transit getway currently i need 2 getway

Task 7: Configure AWS Transit Gateway Attachments

The VPC attachment enables the transit gateway to route packets to and from a VPC. While the Transit Gateway peering attachment enables routing between transit gateways. A Transit Gateway peering connection attachment is required when routing traffic across multiple regions using transit gateway.

CONFIGURE A TRANSIT GATEWAY VPC ATTACHMENT IN  EACH VPC

Transit Gateway Attachments->name->Transit Gateway ID->Attachment type: VPC->VPC ID:->Subnet IDs: Choose avialbel subnets->create.
Transit Gateway Peering Attachment is required when routing traffic across multiple regions using AWS Transit Gateway.

CONFIGURE A TRANSIT GATEWAY PEERING ATTACHMENT IN both REGION

to connection between two transit getway we need to establish peering

Create transit gateway attachment->name->Transit Gateway ID->Attachment type: Peering Connection->Account: My account->Region->chose another region->Transit gateway (accepter):Paste the ID for another Transit Gateway.

Task 8: Configure network traffic routing across AWS Transit Gateway connection

CONFIGURE ROUTE TABLES IN  EACh VPC as we did earlyer

on each vpc->route table->select rout table->route->in destination ->ip->select transit getway->chose transit attachment->save 

When you attach a VPC to a Transit Gateway, a route entry must be added in the VPC route table for traffic to route through the Transit Gateway.

In addition, when you peer Transit Gateways, a static route entry must be added in the Transit Gateway route table to enable traffic to route between the peers.

Hence in this lab, configure both the VPC and Transit Gateway Route tables.

CONFIGURE TRANSIT GATEWAY ROUTE TABLES IN OHIO REGION

create route table for each attachment,Transit gateway route tables->name->Transit Gateway ID->create.

You have completed setting up both the VPC and transit gateway route tables. There are 3 transit gateway route tables in Oregon region and 2 transit gateway route tables in Ohio region.

CONFIGURE TRANSIT GATEWAY ASSOCIATIONS AND ROUTE PROPAGATION IN all REGION

VPC service->choose Route tables->select ->Associations->Create association->Create association

choose Propagations-> Create propagation->attachment to propagate->create.

Routes->Create static route->CIDR->Active->Choose attachment->Create static route.

Task 9: Test network connectivity across AWS Transit Gateway connection

Test connectivity ->ping < vpc2publicInstanceIP >

![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/433bfacc-3210-4c3e-9a9c-3a23f410e22d)

# Deploying Amazon VPC for a 3-tier Web App

Tools are available on the internet to help you calculate and create IPv4 subnet CIDR blocks; for example, IPv4 Address Planner.

1.create vpc and 2 publick subnet and 4 private subnets,Creating an Internet gateway,Creating a NAT gateway.Configuring route tables->CONFIGURE PUBLIC ROUTE TABLE->CONFIGURE PRIVATE ROUTE TABLE,Creating security groups(security group for the EC2 instances. Only the ALBSecurityGroup will be allowed to talk to the EC2 instances,you will create an RDSSecurityGroup so the EC2 instances can communicate to the RDS instances,EC2 instances to communicate with the RDS instances on port 3306),Launch web app instances and database resources, and deploy the application->CREATE THE DATABASE IN THE PRIVATE SUBNETS->CREATE AN APPLICATION LOAD BALANCER


# CloudFormation
CloudFormation->Create stack ->With new resources (standard)->Prerequisite - Prepare template ->Template is ready->Specify template ->Upload a template file(ymal file)->next->Stack name ->Next->specify Tags, Permissions, Stack failure options, and Advanced options->Stack failure options->Preserve successfully provisioned resources.

In cloudformation if i delete stack it will atoumatically delete resource a that is created.to avoide we need add DeletionPolicy: Retain in ymal file

AWS CloudFormation, you can change the properties of an existing resource in the stack.

# AWS Lambda Foundations
Serverless

AWS Lambda is a compute service. You can use it to run code without provisioning or managing servers. Lambda runs your code on a high-availability compute infrastructure.

**AWS serverless platform**

1.aws lambda edge, 2.aws step function, 3.amazon s3 4.amazon dynamodb, 5.amazon eventbridge, 6.amazon sns, 7.amazon api getway, 8.aws cloud developemnt kit.

Lambda function?

the code we run on aws lambda is called lambda function.

Lambda functions need two permisstions to run,
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/bc2ca5c2-4e09-4fd7-b360-93ec2acd93f3)

Cloud9->open

Verify that necessary packages are installed->aws --version && sam --version && docker --version && node --version && npm --version

command to initialize a new SAM application->

Choice: AWS Quick Start Templates

Package type: Hello World Example

Use the most popular runtime and package type?: No

Which runtime would you like to use?: nodejs18.x

What package type would you like to use?: Image

Would you like to enable X-Ray tracing on the function(s) in your application? [y/N]: N

Would you like to enable monitoring using CloudWatch Application Insights? [y/N]: N

Project name: getletter

A new directory named getletter will appear and AWS SAM templates are an extension of AWS CloudFormation templates, two values that are required when working with container images:

PackageType: Image tells AWS SAM that this function is using container images for packaging.

Metadata helps AWS SAM manage the container images.
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/1120763d-e0f7-4b27-b9b1-c8871fa3c275)
Build the Dockerfile into a Docker container image by running ->docker build -t getletter .

List the Docker images available by running->docker images

Since the Docker image has been built, we can now run a new Docker container from the image->docker run -p 9000:8080 getletter:latest

test invocation->curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'

command to navigate to getletter directory and build your container image ->sam build

you’ve built your conainer image using AWS SAM and tested your function locally running on a container, let’s get it ready to deploy to a Lambda function which can be invoked using the Lambda service. First we’ll create a repository to host a copy of our local container image so Lambda can refer to that copy
# Deploying and Testing Your App
CREATING YOUR ECR REPOSITORY AND PUSHING YOUR IMAGE

Create an Amazon Elastic Container Registry (ECR)->aws ecr create-repository \
    --repository-name getletter \
    --image-scanning-configuration scanOnPush=true
    
to login to docker : aws ecr get-login-password \
    | docker login \
    --username AWS \
    --password-stdin <repositoryUri>

push the local image into the registry->docker push <repositoryUri>:latest

USING SAM TO UPLOAD YOUR ECR IMAGE AND DEPLOY YOUR LAMBDA FUNCTION



# Migrating to server less

three migration patterns that you might follow to migrate your legacy applications to a serverless model

1.Leapfrog

As the name suggests, with the leapfrog pattern, you bypass interim steps and go straight from an on-premises legacy architecture to a serverless cloud architecture.

2.organic

With the organic pattern, you move on-premises applications to the cloud in more of a lift-and-shift model. In this model, existing applications are kept intact, either running on Amazon Elastic Compute Cloud (Amazon EC2) instances or with some limited rewrites to container services such as Amazon Elastic Kubernetes Service (Amazon EKS), Amazon Elastic Container Service (Amazon ECS), or AWS Fargate.

3.strangler 

With the strangler pattern, an organization incrementally and systematically decomposes monolithic applications by creating APIs and building event-driven components that gradually replace components of the legacy application.New feature branches can be serverless first, and legacy components can be decommissioned as they are replaced. This pattern represents a more systematic approach to adopting serverless, allowing you to move to critical improvements where you see benefit quickly but with less risk and upheaval than the leapfrog pattern.

# Migration considerations
the most common for moving complex applications is the strangler pattern, where you refactor and rewrite parts of your application with serverless. 

What does this application do and how are its components organized?

how can you break up your data based on the command query responsibility segregation, or CQRS, pattern? What belongs on the control plane and what belongs on the data plane?

How does the application scale and what components drive the capacity you need?

Do you have schedule-based tasks?

Do you have workers listening to a queue? This may be an easy place to introduce an Amazon Simple Queue Service (Amazon SQS) queue without requiring a lot of untangling of the existing code.

Where can you refactor or enhance functionality without impacting the current implementation?

You can use both Application Load Balancer and API Gateway to direct traffic to different targets to easily incorporate new components without disrupting the existing system.
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/01b17928-e788-4849-b16a-74fff0f780e0)
three factors when comparing costs of ownership:

The infrastructure cost to run your workload. For example, the costs for your provisioned EC2 capacity compared to the per-invocation cost of your Lambda functions.

The development effort to plan, architect, and provision resources on which the application will run.

The costs of your team’s time to maintain the application once it is in production.

# AWS Fargate is another serverless compute option
Fargate is a managed service, purpose built as a compute engine for containers. With Fargate, you don’t need to manage the underlying EC2 clusters for your hosted containers.

Considerations for choosing Fargate or Lambda for serverless compute
![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/f03f7904-5962-4309-b12b-55c224eed9d3)

![image](https://github.com/kiran-ab01/AWS_solution-architect/assets/132429361/6a4fa1a9-72eb-4fb9-bb36-b4620e1ebdf2)


