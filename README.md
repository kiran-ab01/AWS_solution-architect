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









