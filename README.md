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




