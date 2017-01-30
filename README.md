AWS Notes
---

Use tags

service = name of service this could be accounts, ETL, frontend api etc  
environment = production, test, development  
version = version number

AWS IAM (access based on tags)

AWS Identity and Access Management is a web service that allows organizations to manage users and user permissions for various AWS services. The user can add conditions as a part of the IAM policies. The condition can be set on AWS Tags, Time, and Client IP as well as on various parameters. If the organization wants the user to access only specific instances, he should define proper tags and add to the IAM policy condition. The sample policy is shown below.

```
"Statement": [
    {
      "Action": "ec2:*",
      "Effect": "Allow",
      "Resource": "*",
      "Condition": {
         "StringEquals": {
            "ec2:ResourceTag/InstanceType": "Production"         
         }
      }
    }
  ]
```
http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html

With AWS IAM a user is creating an application which runs on an EC2 instance and makes requests to AWS, such as DynamoDB or S3 calls. Here it is recommended that the user should not create an IAM user and pass the user's credentials to the application or embed those credentials inside the application. Instead, the user should use roles for EC2 and give that role access to DynamoDB /S3. When the roles are attached to EC2, it will give temporary security credentials to the application hosted on that EC2, to connect with DynamoDB / S3.

http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html


###Create a filesystem on an EBS Volume
---

New EBS volumes have no filesystem, so you can't start storing data immediately after attaching them to a specific instance.

Initializing a volume with a specific filesystem type is easy using Linux and can be done by issuing the following commands.

Issue the following command to create an ext3 filesystem on the new volume:
    sudo mkfs -t ext4 /dev/sdf
Make the directory for mounting the new storage:
    sudo mkdir /mnt/ebs-store
Mount the new volume:
    sudo mount /dev/sdf /mnt/ebs-store
To configure the Linux instance to mount this volume on boot, open /etc/fstab in an editor by typing the following:
    sudo nano /etc/fstab
Append the following line to /etc/fstab:
    /dev/sdf /mnt/ebs-store ext4 defaults,noatime 1 2
In the text editor, hit Ctrl+O, then Ctrl+X to save the file and exit the editor.

This directories contains local copies of ec2.py and ec2.ini, but you may want to grab
the latest ones.


Playbook explained
---

tag-old-nodes.yml

```
All this does is, using the dynamic inventory, goes through the existing nodes and adds the tag "oldMyApp" (set this tag to something more relevant to your app).
```

destroy-old-nodes.yml

```
All this does is remove any instance that has the tag "oldMyApp". This tag is actually passed in from the kick off script.
```

###Docker
---

docker-on-elasticbeanstalk


###Random

What does baseline iops mean?

```
As you probably know IOPS are a unit of measure representing input/output operations per second. Operations are measured in KiB (KiB being the SI for the binary kibibyte which represents 1024 bytes). The underlying drive technology determines the maximum amount of data that a volume type counts as a single I/O.

With Amazon EBS, I/O size is capped at 256 KiB for SSD volumes and 1,024 KiB for HDD volumes. This value is referred to as the baseline IOPS for those drive types.

The key word here is “capped”. AWS does make it possible for you to access IOPS performance above these baselines by using an I/O credit balance system. As with most AWS services the I/O credit system works out ok for the end user. The initial credit balance is there to provide decent speeds for boot volumes and to ensure a good bootstrapping experience for other applications.

I/O credits represent the available bandwidth that your volume can use to burst large amounts of I/O when more than the baseline performance is needed. The more credits your volume has for I/O, the more time it can burst beyond its baseline performance level and the better it performs when more performance is needed.
```

Amazon CloudWatch Alarms have three possible states:
```
OK: The metric is within the defined threshold
ALARM: The metric is outside of the defined threshold
INSUFFICIENT_DATA: The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state
```

###References / Acknowledgements
---

Exam blueprint
https://d0.awsstatic.com/training-and-certification/docs-dev-associate/AWS_certified_developer_associate_blueprint.pdf"

Subnets / VPC / security
http://cloudacademy.com/blog/aws-network-acl-vpc-subnets-network-security/

Architecting for the Cloud
https://d0.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf

Gobal infrastructure
https://aws.amazon.com/about-aws/global-infrastructure/

Event notification
https://aws.amazon.com/blogs/aws/s3-event-notification/

High Availability Architectures
http://www.slideshare.net/AmazonWebServices/high-availability-application-architectures-in-amazon-vpc-arc202-aws-reinvent-2013?next_slideshow=1AWS

Well Architected Framework
https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf

Loosely coupled design
https://aws.amazon.com/blogs/aws/on-using-aws-in/

Building fault tolerant applications
https://d36cz9buwru1tt.cloudfront.net/AWS_Building_Fault_Tolerant_Applications.pdf

Architectural best practices
https://media.amazonwebservices.com/architecturecenter/AWS_ac_ra_ftha_04.pdf

High Availability Architectures
http://www.slideshare.net/AmazonWebServices/high-availability-application-architectures-in-amazon-vpc-arc202-aws-reinvent-2013?next_slideshow=1

Cloudwatch Developer Guide
http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/ec2-metricscollected.html#ec2-metric-dimensions

Simple Queue Service FAQ
https://aws.amazon.com/sqs/faqs/

Building Fault Toerant Applications in the Cloud
http://www.slideshare.net/AmazonWebServices/building-fault-tolerant-applications-in-the-cloud-aws-summit-2012-nyc

High Availability and Scalability
http://cloudacademy.com/blog/aws-vpc-with-high-availability-and-scalability-for-your-cms/

AWS Back up and recovery
https://media.amazonwebservices.com/AWS_Backup_Recovery.pdf

RTO/ RPO
https://media.amazonwebservices.com/AWS_Disaster_Recovery.pdf

Routing in VPC design
https://aws.amazon.com/vpc/faqs/

Web Identity Federation 1
https://aws.amazon.com/blogs/aws/federated-users-temp-creds-aws-cloudformation/

Web Identity Federation 2
https://media.amazonwebservices.com/blog/2013/cloudformation_iam_role_auth_flow_1.png
