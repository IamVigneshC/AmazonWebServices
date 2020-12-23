## Access and Enforce Data Compliance Using AWS Config

Your company’s Chief Information Security Officer (CISO) has called on you to investigate an AWS account. Router logs are showing large amounts of secure shell (SSH) traffic coming in from an unauthorized location. The CISO suspects that servers on the network are being used for crypto currency mining by someone from outside the company. They ask you to remediate any security groups that are allowing unauthorized SSH traffic and turn on detailed monitoring for each instance.

- Set up AWS Config
- Remediate the Noncompliant Security Group
- Remediate the Noncompliant EC2 Instances

1. Log in to the AWS web console and use the the Services dropdown to navigate to the AWS Config service (search for config or click on Config under the Management & Governance services grouping).

1. On the AWS Config welcome page, click the button labeled Get started.

1. On the Settings page, find the heading labeled Resource types to record and uncheck the selection for All resources. In the field next to Specific types, enter ec2 and select Instance and SecurityGroup.

1. Under the heading labeled Amazon S3 bucket, make sure the option to Create a bucket is selected. Under the heading AWS Config role, make sure Create AWS Config service-linked role is selected.  

1. At the bottom of the settings page, click Next.

1. On the AWS Config rules page, click into the text box labeled Filter by rule name, label or description. Type detailed to filter the visible rules to one named ec2-instance-detailed-monitoring-ena…. Select that rule by clicking it.  

1. Delete detailed from the text box and type in ssh to filter the visible rules to one named restricted-ssh.  Select that rule by clicking it, then click Next.

1. On the Review page, verify that your setup details include the following:

1. Two config rules, one for EC2 instance detailed monitoring and another for restricted SSH

1. Two resource types, AWS::EC2::Instance and AWS::EC2::SecurityGroup

1. An Amazon S3 bucket

1. An AWS Config role

1. Click Confirm.

1. Once complete you'll be directed back to the AWS Config dashboard. If you notice a blue alert message at the top about a redesigned AWS Config console, click the Try it out now link. If you don't notice this message, you should already be in the newest version of the console.

With your AWS Config setup complete, the service will begin finding resources and determining their compliance status based on the rules you selected. This process should take a few minutes. After a few minutes, refresh the page. 

The dashboard displays compliance status by rule and by resource. Under Compliance Status, you should see two noncompliant rules and four noncompliant resources, as in the image below.
