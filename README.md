# Secure File Transfer using AWS Transfer Service

AWS Transfer Family is a fully managed AWS service that you can use to transfer files into and out of Amazon Simple Storage Service (Amazon S3) storage over the following protocols:

- Secure Shell (SSH) File Transfer Protocol (SFTP) (AWS Transfer for SFTP)

- File Transfer Protocol Secure (FTPS) (AWS Transfer for FTPS)

- File Transfer Protocol (FTP) (AWS Transfer for FTP)

AWS Transfer Family creates a file transfer protocol-enabled server and then assignes users to use the server. To service your AWS Transfer Family users' transfer requests, you create an AWS Identity and Access Management (IAM) role to access your Amazon S3 bucket.

### Benefits of using AWS SFTP

-	File sharing and transfer service 
-	Single sign on with credential management of PB level
-	Role based access control according to ITSG 33, 22 standards
-	Access management of directories/data based on Role 
-	Large Number users 
-	Large file transfers with unlimited storage facility 
-	High availability and data redundancy
-	Data residency and encryption according to PB level 
-	folder structure should be designed and depending on data classification 
-	Custom domain name attached
-   Can be setup in a [VPC endpoint private configuration behind NLB](https://dzone.com/articles/aws-transfer-for-sftp-explained-a-vpc-use-case) which can then be complaint with enterprise firewall configuration 

In simple words it provides a secure file transfer service using multiple protocols like SFTP, FTPS and FTP backed by Amazon S3 block storage which has SLA of five 9s. If a team is planning to host PB data SFTP service which is based on SSH protocol which can be used with Ephemeral Diffie-Hellman (DHE) the recommended encryption algorithm for data in transit and S3 storage service with DHE for data at rest is a good option. 

This service can be connected to Active Directory services for user management and role based access control to the directories within S3 buckets. This is managed by the SFTP service using the Logical Directories. All of the above services sit behind secure AWS API Gateway service which is made available using the custom domain name.

All of the above is explained in great detail in the following AWS official documentation and links: 

[Official AWS Documentation](https://aws.amazon.com/aws-transfer-family/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)

### AWS Logical Directories

With logical directories you can customize how S3 bucket paths are visible to your SFTP end users, enabling you to:

- Easily limit access to files and folders in S3 buckets (e.g. subscription-based access)
- Preserve file and folder paths referenced in existing applications and scripts
- Distribute files to multiple consumers without creating copies
- Prevent S3 bucket names from being visible to SFTP end users for compliance/regulatory purposes

The [CloudFormation template](sftp.yaml) is based on
[AWS SFTP With Logical Directories Blog](https://aws.amazon.com/blogs/storage/using-aws-sftp-logical-directories-to-build-a-simple-data-distribution-service/) along with some changes to the code that creates a role and S3 buckets with customized folder mapping. 

This is also a good blog for [Chroot and logical directories](https://aws.amazon.com/blogs/storage/simplify-your-aws-sftp-structure-with-chroot-and-logical-directories/)

## Steps to Run The Template

1. Download the [CloudFormation template](sftp.yaml) and save it on your computer 
3. Login to AWS managment console 
4. Select CloudFormation Service from the Services 
5. Clik Create Stack and upload the template file and click next. 
6. Provide Stack name, this could be any name e.g. test-sftp-01
7. you can leave the rest of the parameters to default or you can specify your own username and password and click next
8. Leave the UserPublicKey parameter as is, this is not used but required to step through Lambda code conditions. 
9. Click next again on the configure stack options page with no changes
10. On the Review page select the checkbox to create Role and click Create Stack

The above steps will start creation of a stack, to track progress click on events refresh page:

Once the creation of stack is complete follow the manual steps.

## Manual Steps through AWS Web Console

1. Navigate to S3 Service and select the newly created S3 Bucket: ccoe-sftp-185299205377-ca-central-1
2. Ceate two directories/folders with exact names as below and put some files for demo.
- test
- test2
3. Upload demo files for display 

## Test the service
1. To Test the service navigate to Transfer Service 
2. Click on the service 
3. copy the endpoint name
4. Using Filezilla or FTP from command prompt run connect to the service

## Destroying the environment 
To save cost after testing is done delete the stack but before deleting make sure that the S3 bucket is empty. 
1. Once done the demo and before deleting the stack make sure that the S3 directory is empty from S3 web console

