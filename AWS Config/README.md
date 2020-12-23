## Access and Enforce Data Compliance Using AWS Config

Your company’s Chief Information Security Officer (CISO) has called on you to investigate an AWS account. Router logs are showing large amounts of secure shell (SSH) traffic coming in from an unauthorized location. The CISO suspects that servers on the network are being used for crypto currency mining by someone from outside the company. They ask you to remediate any security groups that are allowing unauthorized SSH traffic and turn on detailed monitoring for each instance.

- Set up AWS Config
- Remediate the Noncompliant Security Group
- Remediate the Noncompliant EC2 Instances

### Set up AWS Config

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



### Remediate the Noncompliant Security Group

Using the rules you specified, AWS Config has revealed the resources that are noncompliant.  These are the resources that you will focus on for remediation before reporting back to the CISO.  Start your remediation effort with the noncompliant security group.


1. On the left side of the AWS Config Dashboard, click the Resources link to display the Resource Inventory.  

1. Click the drop-down menu for Resource type and enter securitygroup, then select AWS EC2 SecurityGroup. 

1. In the list of resources, find the security group that is noncompliant and click its resource identifier. This opens a page showing the rules that make the resource noncompliant.  

1. Click Manage resource. This opens a new tab with the noncompliant security group selected. 

1. Click the Actions dropdown menu and select Edit inbound rules.

1. Review the inbound rules, then delete the rule that allows SSH traffic from all addresses (Note: It's the only rule with a description filled out). 

1. Click Save rules.

With the wide-open SSH rule removed, the noncompliant security group has been remediated; however, it will take several minutes for AWS Config to register the new compliance status. 


### Remediate the Noncompliant EC2 Instances

Remediating the noncompliant security group is probably the most important step in mitigating the unauthorized access that was detected.  However, detailed monitoring will assist in detecting anomalies in server metrics much sooner than default monitoring.  Use the compliance status from AWS Config to find and remediate the noncompliant EC2 instances by enabling detailed monitoring.


1. Navigate back to the Config service. On the left side menu, click the Resources link to display the Resource inventory, then, if there's a blue banner at the top with a notice about a redesigned console, click the Try it out now button to get the latest UI. 

1. Click the dropdown menu for Resource type and type instance. Select AWS EC2 Instance. 

1. In the list of resources, click the resource identifier for any one of the noncompliant EC2 instances. This opens a page showing the rules that make the resource noncompliant.  

1. On the right side of the page, click Manage resource.  This opens a new tab, filtered to the noncompliant instance.  

1. Right click on the instance in the table, then select Monitoring and troubleshoot > Manage detailed monitoring.

1. Click the checkbox next to Enable, and then Save.

1. Repeat the previous two steps needed to enable detailed monitoring for the remaining instances (ie the instances still listed as disabled in the Monitoring column).

1. Now that detailed monitoring has been enabled for the EC2 instances, all noncompliant resources have been remediated.  Before you report back to the CISO, confirm the compliance status in the AWS Config dashboard.

1. Go back to the tab with the Config service. On the left side of the dashboard, click the Resources link to display the Resource inventory. 

1. Select Compliant from the Compliance dropdown. 

1. Confirm that all resources listed are compliant. This verifies that your remediation steps have been successful.

