# MultiVPC-peering-TransitGateway

Setting up Transit Gateway and VPC Endpoints for a Multi-VPC Architecture 

 

Scenario: A large organization is migrating its on-premises infrastructure to the AWS cloud. The organization's architecture involves multiple VPCs for different departments and applications, each requiring secure communication with centralized services and external resources. The IT team needs to design and implement a scalable and efficient network architecture to accommodate the organization's growth and ensure robust connectivity between VPCs and external services. 

Objectives: Design and deploy a scalable network architecture using AWS Transit Gateway to simplify network connectivity between multiple VPCs. Configure VPC endpoints to securely access AWS services without internet gateways or NAT gateways, ensuring data privacy and minimizing exposure to external threats. 

 

Design and deploy a scalable network architecture using AWS Transit Gateway to simplify network connectivity between multiple VPCs 

 

4 regions  

Oregon us-west-2 

Tokyo ap-northeast-1 

Frankfurt eu-central-1 

Paris  eu-west-3 

 

Oregon us-west-2 

To set uo Tansit Gateway and VPC endpoints for a multi VPC Architecture 

 I have selected 4 regions in AWS 

1. Oregon : us-west-2 

2. Tokyo : ap-northeast-1 

3. Frankfurt: eu-central-1 

4. Paris: eu-west-3 

  

Considering oregon as hub and spoke region for remaining regions 

  

Consider Oregon region 

Create VPC 

Go to Amazon AWS console in that select VPC 

select create VPC 

Name: vpc-hub-oregon 

CIDR: 10.0.0.0/16 

  

Create internet gateway and attach vpc created 

  

Create Subnets: 

Public subnet :10.0.1.0/24 

Private subnet:10.0.2.0/24 

  

Create Transit Gateway 

Configure transit gateway with default ASN number 64512 

Now go to Transit Gateway attachments and create 1 attachment with created vpc-hub-oregon 

 Tokyo ap-northeast-1 

Create VPC 

Go to Amazon AWS console in that select VPC 

select create VPC 

Name: vpc-tokyo 

CIDR: 11.0.0.0/16 

  

Create internet gateway and attach vpc created 

  

Create Subnets: 

Public subnet :11.0.1.0/24 

Private subnet:11.0.2.0/24 

  

Create Transit Gateway 

Configure transit gateway with default ASN number 64513 

Now go to Transit Gateway attachments and create 1 attachment with created vpc-tokyo 

 Frankfurt eu-central-1 

Create VPC 

Go to Amazon AWS console in that select VPC 

select create VPC 

Name: vpc-frankfurt 

CIDR: 12.0.0.0/16 

  

Create internet gateway and attach vpc created 

  

Create Subnets: 

Public subnet :12.0.1.0/24 

Private subnet:12.0.2.0/24 

  

Create Transit Gateway 

Configure transit gateway with default ASN number 64514 

Now go to Transit Gateway attachments and create 1 attachment with created vpc-frankfurt 

 Frankfurt eu-central-1 

Create VPC 

Go to Amazon AWS console in that select VPC 

select create VPC 

Name: vpc-frankfurt 

CIDR: 12.0.0.0/16 

  

Create internet gateway and attach vpc created 

  

Create Subnets: 

Public subnet :12.0.1.0/24 

Private subnet:12.0.2.0/24 

  

Create Transit Gateway 

Configure transit gateway with default ASN number 64514 

Now go to Transit Gateway attachments and create 1 attachment with created vpc-frankfurt 

 Paris  eu-west-3 

Create VPC 

Go to Amazon AWS console in that select VPC 

select create VPC 

Name: vpc-Paris 

CIDR: 13.0.0.0/16 

  

Create internet gateway and attach vpc created 

  

Create Subnets: 

Public subnet :13.0.1.0/24 

Private subnet:13.0.2.0/24 

  

Create Transit Gateway 

Configure transit gateway with default ASN number 64515 

Now go to Transit Gateway attachments and create 1 attachment with created vpc-Paris 

 Multi-VPC-Peering-TGW 

Create Transit Gateway 

Configure transit gateway with default ASN number 64515 

Now go to Transit Gateway attachments and create 1 attachment with created vpc-Paris 

  

Now again select Oregon region 

Go to Transit Gateway attachments 

Create transit gateway attachment there select attachment type as peering select appropriate id and subnet ids 

create vpc peering attachment 

same repeat for other regions create transit gateway peering attachments with respective configurations 

send the request to accept to peering for transit gateway and accept in respective regions 

  

Now come to Transit gateway Route table 

check for propagation is enabled for hub region 

Now create the static Routes with resource as peering 

11.0.0.0/16 transit gateway 

12.0.0.0/16 transit gateway 

13.0.0.0/16 transit gateway 

  

Same repeat for other regions and create appropriate static routes 

 Now its time to launch the EC2 instances in AWS console 

Paris-Ec2-instance 

Now create the private ec2- instance in aws by selecting appropriate vpc created and subnet as private subnet and launch the instance. 
Frankfurt-Ec2 

 

Now create the private ec2- instance in aws by selecting appropriate vpc created and subnet as private subnet and launch the instance. 
Tokyo-Ec2 

Now create the private ec2- instance in aws by selecting appropriate vpc created and subnet as private subnet and launch the instance. 
Oregon-Ec2-Hub 

Now create the private ec2- instance and public instance in aws by selecting appropriate vpc created and subnet as private subnet  and public subnet and launch the instance. 

Now access CLI/Gitbash 

Connect your Oregon (hub) public instance using ssh –i oregon.pem ec2-user@public ip 

Once the connection is established check for remote connectivity of other region instances 

Ping private ip of tokyo instance 

Ping private ip of Frankfurt instance 

Ping private ip of Paris instance 

 

That means we have successfully Design and deploy a scalable network architecture using AWS Transit Gateway to simplify network connectivity between multiple VPCs 
Configure VPC endpoints to securely access AWS services without internet gateways or NAT gateways, ensuring data privacy and minimizing exposure to external threats 

 

VPC-Endpoint 

Now go to Oregon region vpc section 

Select Endpoint  

Create endpoint  

Provide name 

Service category as AWS service 

Services select s3 

Service name select appropriate option and type Gateway 

Click on create Endpoint 

Same procedure repeat for other regions also 

Modifiy the Private Route table and check vpc endpoint in target section 

Now we have created an endpoint attached to Private Route Table 

 

VPC-endpoint 

In Gitbash 

Ssh –i .pem ec2-user@public_ip 

Once the connection is establised use Bastian server to connect from Private EC2 of other regions 

Cat oregon.pem 

Copy the pem key 

Create a new file 

Vi test.pem 

Copy and save the file with pem key 

Chmod 400 test.pem 

Now try to connect with private ec2 

Ssh –i test.pem ec2-user@private-ip of private ec2’s of other region one by one 

Once we logged in successfully 

Use aws configure 

Access_key 

Secret_key 

Region: Enter respected regions 

Output: json 

Now check for aws s3 ls 

Because of internet access and we have created vpc endpoints we will get the output of aws s3 ls aws service 

Communication has been successfully processed. 
VPC Flow logs 

It will help us to monitor our created VPCs 

Traffic which is flowing to VPC will capture by VPC flowlogs 

VPC-Flowlogs creation 

Vpc flowlogs 

Filter (accept/reject/all 

Maximum aggregation interval as 1 min 

Destination: where we want to save the logs  

Please selct sent to Amazon S3 bucket 

S3 is one of the storage available in AWS 

Create S3 bucket 

Bucket name give some unique name 

AWS region 

Object ownership : ACLS enabled 

Create the bucket by selecting appropriate options 

Copy the ARN (amazon Resource number) 

Log record format : AWS default format 

Log file format : Text 

Click on create flow log 

 

Now check the flow logs by checking tar file which you have downloaded. 
We have successfully configured VPC endpoints to securely access AWS services without internet gateways or NAT gateways, ensuring data privacy and minimizing exposure to external threats 
